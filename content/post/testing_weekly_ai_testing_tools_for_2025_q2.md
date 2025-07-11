+++
date = '2025-07-03T16:26:47+08:00'
draft = false
title = '测试周刊007: AI 测试工具2025上半年盘点'
author = "乙醇"
tags = ["测试周刊"]
weight= 1
authorLink = "https://github.com/easonhan007"
+++

2025 年已经过去一半了，这半年里，ai 与测试工具的结合有了不少进展。

今天我们就一起来盘点一下。

<!--more-->

## 🔍 Shortest

一个零配置的工具，只需指向你的网站或应用，AI 便可自动生成并执行测试。非常适合需要快速反馈循环而不依赖繁琐脚本的场景。

[项目地址](https://github.com/antiwork/shortest): https://github.com/antiwork/shortest

### 🧪 主要特性

- 基于自然语言的端到端（E2E）测试框架
- 使用 **Anthropic Claude API** 进行 AI 驱动的测试执行
- 构建于 **Playwright** 之上
- 支持 **GitHub 集成**，并兼容 **两步身份验证（2FA）**
- 通过 **Mailosaur** 实现邮件验证功能

<video width = "800" src="https://github.com/user-attachments/assets/d443279e-7364-452b-9f50-0c8dd0cf55fc" controls autoplay loop muted>
Your browser does not support the video tag.
</video>

---

## 🧪 TestZeus Hercules

一个由大型语言模型（LLM）驱动的框架，旨在自动生成和执行测试用例。面向企业级使用场景，能很好地集成到 CI/CD 流程中。

项目地址: https://github.com/test-zeus-ai/testzeus-hercules

---

## 🎥 MidScene

利用 AI 录制并重播用户流程，并生成自然语言描述。它弥合了业务级需求与测试脚本之间的鸿沟。

项目地址: https://github.com/web-infra-dev/midscene/blob/main/README.zh.md

### 主要特性

#### 用自然语言编写自动化脚本

- 描述你的目标和步骤，Midscene 会为你规划和操作用户界面。
- 使用 Javascript SDK 或 YAML 格式编写自动化脚本。

#### Web & Mobile App

- **Web 自动化 🖥️**: 可以[与 Puppeteer 集成](https://midscenejs.com/integrate-with-puppeteer.html)，[与 Playwright 集成](https://midscenejs.com/integrate-with-playwright.html)或使用[桥接模式](https://midscenejs.com/bridge-mode-by-chrome-extension.html)来控制桌面浏览器。
- **Android 自动化 📱**: 使用 [Javascript SDK](https://midscenejs.com/integrate-with-android.html) 配合 adb 来控制本地 Android 设备。

#### 工具

- **用于调试的可视化报告 🎞️**: 通过我们的测试报告和 Playground，你可以轻松理解、回放和调试整个过程。
- [**使用缓存，提高执行效率 🔄**](https://midscenejs.com/zh/caching.html): 使用缓存能力重放脚本，提高执行效率。
- [**MCP 🔗**](https://midscenejs.com/zh/mcp.html): 允许其他 MCP Client 直接使用 Midscene 的能力。

#### 三种类型的 API

- [**交互 API 🔗**](https://midscenejs.com/zh/api.html#interaction-methods): 与用户界面交互。
- [**数据提取 API 🔗**](https://midscenejs.com/zh/api.html#data-extraction): 从用户界面和 DOM 中提取数据。
- [**实用 API 🔗**](https://midscenejs.com/zh/api.html#more-apis): 实用函数，如 `aiAssert()` （断言）, `aiLocate()` （定位）, `aiWaitFor()` （等待）。

---

## 🤖 Giskard

为机器学习模型提供 AI 质量保障框架，支持测试、验证与监控。不仅仅是功能性测试，还涵盖偏差检测、模型漂移以及稳健性分析。

项目地址: https://github.com/Giskard-AI/giskard

简单来说这是用来评估大模型质量的。

可以检测下面的问题

- 幻觉
- 生成有害内容
- 提示注入
- 健壮性问题
- 敏感信息披露
- 刻板印象与歧视
- 以及更多……

---

## 🔐 PentestGPT

将渗透测试技术与 GPT-4 能力结合，自动化安全测试流程中的部分环节。非常适合增强手动安全测试的效率。

项目地址: https://github.com/GreyDGL/PentestGPT

这是做渗透测试的。

---

## 🧭 Zerostep

一款用于 Playwright 的 AI 插件，支持用自然语言编写端到端测试。无需选择器，AI 可自动识别 UI 操作和验证步骤。

项目地址: https://zerostep.com/

可以看一下代码，还是有点意思的，基本上就是用自然语言来描述用例。

```typescript
import { test, expect } from "@playwright/test";
import { ai } from "@zerostep/playwright";

test.describe("Calendly", () => {
  test("book the next available timeslot", async ({ page }) => {
    await page.goto("https://calendly.com/zerostep-test/test-calendly");

    await ai("Verify that a calendar is displayed", { page, test });
    await ai("Dismiss the privacy modal", { page, test });
    await ai("Click on the first day in the month with times available", {
      page,
      test,
    });
    await ai("Click on the first available time in the sidebar", {
      page,
      test,
    });
    await ai("Click the Next button", { page, test });
    await ai("Fill out the form with realistic values", { page, test });
    await ai("Submit the form", { page, test });

    const element = await page.getByText("You are scheduled");
    expect(element).toBeDefined();
  });
});
```

---

## 🧠 更多推荐

- **Goose (Block)** — 一个由 AI 驱动的开发助手，可以通过自然语言提示生成项目结构、编写测试并进行调试
- **Airtap** — 一个灵活的 JavaScript 测试运行器，支持在真实浏览器中运行 TAP 测试，适合 Web QA
- **SQLMap-AI** — 用 AI 驱动传统 SQL 注入测试，提供自动化与自适应流程控制

## 总结

人工智能（AI）在软件测试领域的应用已成为行业发展的主流趋势。随着 AI 模型能力的不断提升以及 Agent 模式的广泛应用，高效且精准的自动化测试时代正在加速到来。AI 技术通过自动生成测试用例、优化测试套件以及实现自愈测试脚本，正在显著提升测试效率并降低人工干预的需求。

近期，微软宣布了大规模裁员计划，据报道涉及约 9000 名员工，裁员主要集中在软件工程和项目管理等技术岗位。 虽然微软强调裁员是为了优化组织结构和减少管理层级，但其首席执行官 Satya Nadella 曾表示，公司部分项目的代码已有高达 30%由 AI 生成，这引发了外界对 AI 取代部分技术岗位的担忧。 尽管没有直接证据表明微软明确声明“不会使用 AI 的程序员将被淘汰”，但行业趋势显示，熟练掌握 AI 工具已成为程序员和测试人员保持竞争力的关键。

未来，随着 AI 驱动的开发效率提升，传统上由专门测试人员完成的验证工作可能会逐步转移至代码的开发者，形成“谁开发，谁测试”的模式。生成式 AI 和测试影响分析等技术的进步使得开发者能够更高效地验证代码更改，从而可能减少对初级测试人员的需求。 与此同时，无代码测试自动化工具的兴起进一步降低了传统编程技能在测试领域的需求，而对具备 AI 应用能力、批判性思维以及复杂系统测试技能的高级测试人员的需求正在增加。

因此，测试人员需要积极适应 AI 技术，掌握相关工具和方法，以在快速变化的行业环境中保持不可替代性。对于企业和从业者而言，拥抱 AI 不仅是提升效率的机遇，也是应对未来挑战的必然选择
