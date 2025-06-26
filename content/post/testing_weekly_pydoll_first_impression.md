+++
date = '2025-06-26T16:26:47+08:00'
draft = false
title = '测试周刊006: playwrightv1.53的新功能'
author = "乙醇"
tags = ["测试周刊"]
weight= 1
authorLink = "https://github.com/easonhan007"
+++

在一个已有开发节奏的团队中，作为第一位测试人员去推行新流程，绝非易事。

你会被开发质疑：是不是要拖慢上线进度？是不是要卡死流程，让整个开发团队都不爽？

这些成见不是你的错，但你必须面对它们。

<!-- more -->

## pydoll 初体验

之前介绍过一款新的 ui 自动化测试工具----[pydoll](https://autoscrape-labs.github.io/pydoll/)。

今天抽空在 github 的 copilot 的帮助下试用了一下。

我用 pydoll 实现了一个测试[任务列表](https://todomvc.com/examples/react/dist/)的测试套件，包含 5 个测试用例。

![](https://github-production-user-asset-6210df.s3.amazonaws.com/1966376/459310738-09a6bfd3-26cb-4e98-bce5-6866f0f56343.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250626%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250626T095315Z&X-Amz-Expires=300&X-Amz-Signature=48215a1a9182263db82af63a813ce359d96be489f934bf8a6a87c64383c308f4&X-Amz-SignedHeaders=host)

具体用例如下

```python
import asyncio
import os
import unittest
from pydoll.browser.chromium import Chrome
from pydoll.browser.options import ChromiumOptions
from pydoll.constants import Key

class TestTodoMVC(unittest.IsolatedAsyncioTestCase):
    async def asyncSetUp(self):
        options = ChromiumOptions()
        options.add_argument('--start-maximized')
        options.add_argument('--disable-notifications')
        self.browser = Chrome(options=options)
        self.tab = await self.browser.start()
        await self.tab.go_to('https://todomvc.com/examples/react/dist/')

    async def asyncTearDown(self):
        await self.browser.__aexit__(None, None, None)

    async def test_add_todo(self):
        new_todo = await self.tab.find(class_name='new-todo', timeout=5, raise_exc=True)
        await new_todo.type_text("Install pydoll")
        await asyncio.sleep(1)
        await new_todo.press_keyboard_key(Key.ENTER)
        todo_items = await self.tab.find(class_name = 'view', timeout=5, find_all=True, raise_exc=True)
        found = False
        texts = []
        for item in todo_items:
            text = await item.text
            texts.append(text)
            if "Install pydoll" in text:
                found = True
                break
        self.assertTrue(found)

    async def test_complete_todo(self):
        await self.test_add_todo()
        toggle = await self.tab.find(class_name="toggle", timeout=5, raise_exc=True)
        await toggle.click()
        completed = await self.tab.find(class_name='completed', timeout=5, raise_exc=True)
        self.assertIsNotNone(completed)

    async def test_delete_todo(self):
        await self.test_add_todo()
        todo_item = await self.tab.find(class_name='view', timeout=5, raise_exc=True)
        await todo_item.click()
        destroy_btn = await todo_item.find(class_name='destroy', timeout=5, raise_exc=True)
        await destroy_btn.click()
        todo_items = await self.tab.find(class_name='view', find_all=True)
        found = False
        for item in todo_items:
            if "Install pydoll" in item.text:
                found = True
                break
        self.assertFalse(found)

    async def test_filter_todo(self):
        await self.test_add_todo()
        toggle = await self.tab.find(class_name="toggle", timeout=5, raise_exc=True)
        await toggle.click()
        active_filter = await self.tab.find(text="Active", timeout=5, raise_exc=True)
        await active_filter.click()
        await asyncio.sleep(1)
        active_items = [item for item in await self.tab.find(class_name = 'view', find_all=True)]
        self.assertEqual(len(active_items), 0)
        completed_filter = await self.tab.find(text="Completed", timeout=5, raise_exc=True)
        await completed_filter.click()
        await asyncio.sleep(1)
        completed_items = await self.tab.find(class_name = 'view', find_all=True)
        found = False
        for item in completed_items:
            title = await item.text
            if "Install pydoll" in title:
                found = True
                break
        self.assertTrue(found)

    async def test_screenshot(self):
        await self.test_add_todo()
        screenshot_path = os.path.join(os.getcwd(), 'pydoll_repo.png')
        await self.tab.take_screenshot(path=screenshot_path)
        self.assertTrue(os.path.exists(screenshot_path))

if __name__ == "__main__":
    unittest.main()
```

上面的代码实现了

- 创建 1 条任务
- 完成 1 条任务
- 删除新创建的任务
- 任务列表过滤
- 截图

这 5 条用例。

### 具体感受

pydoll 在交互上有一定的问题，比如

- 没有 `hover()` 方法，上面删除任务的用例里需要鼠标悬停到任务实例上，等删除按钮出现的时候再去点击。因为 pydoll 没有提供 hover 方法，我就只能用 click 来代替了。在使用`click`之前，我是试着用 javascript 去模拟 hover，但是没有效果，这里其实浪费了 20 分钟以上的；
- 没有 double click 方法，所以双击编辑任务的测试用例就没有实现了；

前端交互稍微复杂一点就不行了。

另外用 `query` 方法去定位一组元素似乎用不起来，因为混合着异步的关系，所以到底为什么定位不到，具体原因还是不清楚。

### 总结

pydoll 目前提供的交互形的 api 还是有较大的提升空间的。

如果你的被测项目前端交互比较丰富，比如需要大量的鼠标悬停，拖拽等操作的话，pydoll 应该可以处理，但可能会用到大量的原生 javascript 代码，有点绕路了，不推荐。

从目前情况看，用 pydoll 来实现数据爬虫可能会比较现实一点。

## copilot 进行 ui 自动化用例编写的感受

之前的项目里用到过 copilot 来进行单元测试用例的生成，在批量实现 fixture 数据上面，ai 对于编码的效率提升是极其巨大的。

这次试试用 copilot 来实现自动化测试用例，而且用的是比较新的工具--pydoll，copilot 的一些局限性就表现出来了。

### 问题 1:幻觉严重

可能是因为 pydoll 是比较新的库吧，copilot 在进行代码生成时出现了比较严重的幻觉。

比如 pydoll 没有`find_elements`方法，但 copilot 一直坚持用这个不存在的方法来定位元素。

所以在用 copilot 写代码时，最好是写成熟框架的代码。

大部分人类都不熟悉的领域，ai 自然也不会有太深的造诣。

### 问题 2:异步和同步分不清楚

我发现 copilot 生成的代码，有一种把大部分语句都当作异步代码来实现的倾向。

比如在遍历一组定位到的元素时，copilot 建议使用`async for`，而一些同步方法，copilot 也喜欢在前面加`await`。

这应该是 pydoll 比较新的缘故的，在 playwright 的异步调用代码生成方面，copilot 的准确率应该会高一些。

### 问题 3:分析不出来具体的问题

在调试代码的过程中，我尝试把报错信息扔给 copilot 进行分析，并给出修改后的代码。

不过在几个回合内，copilot 给出的解决方案不仅包含幻觉（比如调用不存在的方法），而且在我人工修正幻觉后，copilot 的建议也不能解决报错的问题。

后来是我自己根据经验进行调试，才让测试用例顺利跑通。

感觉是 copilot 并没有分析出用例无法执行的根本原因，所以给出的修复方案并不能对症下药。

copilot 有时候很努力，但有可能解决不了根本的问题。

也可能是我自己耐心不足，没准多尝试几轮，copilot 就会给出正确的解法了。

## Playwright v1.52 和 v1.53 有哪些新功能

### 🧠 VS Code 中的 AI 自动修复功能

我们在调试用例的时候，用例运行失败是家常便饭了。

根据我自己的经验，ui 自动化测试 60%以上的时间都花在了调试上。

为什么定位不到元素呢？

为什么获取不到属性呢？

为什么点击没有效果呢？

每日三省吾身。

现在使用 playwright 时， 当测试失败，只需点击错误信息旁边的 ✨ 图标，或在测试资源管理器中悬停测试名称时点击图标。

Playwright 会将足够的上下文信息提供给 Copilot，Copilot 会生成有针对性的修复建议。你可以查看、接受并重新运行，整个流程快速、高效，而且非常实用。

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg8shs35tp2e33mtmejkm.png)

> 想尝试这个功能？确保你已经安装并启用了 Playwright 的 VS Code 插件。

### 🔎 locator.describe()：增强 Trace 和报告可读性

一般情况下，在不使用 po 的前提下，ui 自动化测试的代码的可读性是不够好的。

很多时候，代码里会充斥着各种看不懂的 css 选择器或 xpath 表达式。

现在你可以通过 `.describe()` 为任何定位器添加更具可读性的描述：

```javascript
const newTodo = page
  .getByPlaceholder("What needs to be done?")
  .describe("新待办输入框");
await newTodo.fill("买牛奶");
```

这些描述会出现在：

- Trace Viewer 中
- UI 模式下
- HTML 测试报告中

![](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fzjsqapaixb7b0s7r6jdj.png)

这一小改动在调试复杂 UI 或团队协作时可以带来巨大的帮助。

---

### 📊 HTML 报告支持自定义标题

你可以为测试报告添加更清晰的标题，配置如下：

```javascript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  reporter: [["html", { title: "自定义测试运行 #1028" }]],
});
```

非常适合用于团队仪表板、CI 输出，或用于区分多轮测试的运行记录。

---

### ✅ 新增断言：`toContainClass`

有时候我们是需要断言某个元素是否包含特定的 class 的。

比如有个元素被禁用了，那么`class`里就可能会包含`disabled`这个值。

之前 playwright 是没有特定的断言来开箱即用的。

不过现在有了：

```javascript
await expect(page.getByRole("listitem", { name: "Ship v1.52" })).toContainClass(
  "done"
);
```

这种方式语法简洁、表达精准，尤其适用于基于 class 的 UI 状态检查。

---

### 🧪 快照增强：支持 `children` 和 `url`

ARIA 快照（通过 `toMatchAriaSnapshot`）现在支持：

- `/children: equal` —— 确保子元素被包含在快照中
- `/url: "https://playwright.dev"` —— 匹配特定 URL

示例：

```javascript
await expect(locator).toMatchAriaSnapshot(`
  - list
    - /children: equal
    - listitem: Feature A
    - listitem:
      - link "Feature B":
        - /url: "https://playwright.dev"
`);
```

这让 UI 快照测试在处理复杂或动态组件时更加可靠。

---

### ⚙ test runner 更新

新增了以下快捷功能：

- `testProject.workers` —— 每个测试项目可自定义并发数
- `failOnFlaky` —— 一旦检测到测试不稳定，可自动判定为失败

---

### 🆙 如何升级？

安装最新版本：

```bash
npm i -D @playwright/test@latest
```

同时别忘了将 VS Code 插件更新到最新版本，以便启用 AI 修复等新功能。

## 言论

我不确定“测试债务（Testing Debt）”这个术语在行业中是否被广泛使用！

但在我的实践中，我已经使用“测试债务”这个术语有 9 年了。

我用它来向相关干系人传达和持续提醒我们因为技术债务而不得不进行的返工工作。

在理解“技术债务”含义的基础上，我们是否可以类比出什么是“测试债务”呢？

大多数时候，系统的可测试性（testability）、可自动化性（automatability）和可观测性（observability）都会因为技术债务而受到影响。

而这种影响一旦发生，就会进一步引发测试债务，形成连锁效应。

测试债务的一个主要影响是：原本测试中植入的确定性能力（Deterministic capability 被改变。

而要对这种确定性能力进行返工，在很多情况下并不简单，尤其当业务操作、技术层或基础设施发生了变更时更是如此。

当我说“我们正在做的事情和交付的内容带来了技术债务”，这也意味着，“由此也产生了相应的测试债务”。

从工程的角度看，测试本身就是技术活动的一部分。

所以当有人把测试团队描述为“非技术”，甚至贴上某些标签时，我总觉得有些好笑。

比如，常听到的标签是“手动的”、“可重复的”、“重复性高的”等等。

但其实，测试的“重复性”恰恰就是工程周期和流程的一部分——它有其存在的意义和价值。

--[@Ravisuriya Eswara](https://x.com/testingGarage)
