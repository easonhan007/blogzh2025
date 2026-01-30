---
title: "完全免费！如何安装openclaw"
date: 2026-01-29T09:36:02+08:00
draft: false
---

这几天clawdbot的改名风波闹的沸沸扬扬。

从clawdbot到moltbot再到openclaw，一周装了我3个版本，搞得我都有点精神分裂了。

两个月前，clawdbot的作者peter花了一个周末搞了个叫“WhatsApp Relay”的小项目，结果现在GitHub星标破10万，一周内吸引了200万访问者。完全超出预期。

今天peter正式宣布：这个项目改名叫OpenClaw。

这个名字一路走来挺戏剧：最早叫Clawd，2025年11月诞生，就是那个大模型“Claude”加个爪子的梗，这是英文的谐音梗，我也不太懂，反正logo倒是挺可爱的。

结果火了之后，claude母公司的法务礼貌敲门了，这个名字太像了，蹭了我们的流量，能不能改？

peter表示：我们懂，向资本低头，马上整改。

然后凌晨5点在Discord脑暴，选了Moltbot的名字。molt大概是龙虾要长大就得蜕壳的意思，象征成长。很有诗意，但读起来总有点拗口。

peter不满意，几番折腾之后，现在终于定下来一个新名字：OpenClaw。

这次peter学乖了，商标没有被注册、域名也买好了、代码也迁移了。

现在的名字我觉得是很贴切的：Open代表开源、开放、社区驱动

Claw：就是爪子，保留龙虾爪子的血统，向起点致敬。

---

这次应该不会再改名字了，终于可以给大家分享一下如何安装openclaw了，对于没有技术基础的同学来说，本地安装还是很有难度的。

下面给大家演示一下如何在mac和linux上安装openclaw，这里我们的目标是让国内的同学不花一分钱把服务运行起来。

## mac 系统

先看mac系统。

首先安装nodejs的最新稳定版本。我安装的是`24.13.0`，推荐用nvm安装，我把命令放这里了。

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
nvm install --lts
```

Mac上还需要安装 Command Line Tools，命令放在下面了。

```
# 先彻底删除旧的（很重要，很多时候不删它就装错版本）
sudo rm -rf /Library/Developer/CommandLineTools

# 让系统重新拉取（会弹出安装窗口，按提示点继续）
sudo xcode-select --install

```

安装git，我们用brew安装。

```
brew install git
```

接下来把官网上的命令拷贝下来，在命令行里执行。

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

安装会花一段时间，没有报错的话，进入配置界面。

首先同意协议，按`回车`是继续，空格键是选择。

`onboard mode`选择`QuickStart`，在`model auth provider`里选择千问，千问似乎有一定的免费额度，可以拿来体验。

选择`Qwen OAuth`，这时候会自动打开浏览器，注册或者登录一下就可以了，点击`Confirm`，模型就用改，就默认。

在`select channel`选择`Skip for now`，暂时不配置im集成。

下面就是skill配置，也是openclaw最核心的能力。

随便选择几个安装一下，这一步大概要几分钟的时间。

后面就是各种API key的配置，全部选择`No`，这些后面都可以配。

然后就是`Enable hooks`，选`command-logger`和`session-memory`，前者是命令执行日志，后者是session级别的记忆，都是有用的。

接下来openclaw会安装gateway service，安装成功之后就可以打开TUI了，这时候浏览器会打开openclaw的gateway dashboard页面。

在chat里面设置一下openclaw的角色和名字，顺便设置一下自己的名字，接下来就大功告成了。

## linux 系统，无ui

我们再看一下linux里怎么安装openclaw。

我这里用的是一台多年前的赛扬N5105的机器，性能比较差，反应速度很慢，大家在安装时尽量选择配置高一点的机器或者是vps。

我的操作系统是ubuntu 24.04，纯命令行的，没有UI。

首先需要更新一下相关依赖，命令我放在下面了。

```
sudo apt update
sudo apt upgrade
```

接下来就是安装nodejs和git，跟mac上差不多，就不展开了。

```
sudo apt install git
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
nvm install --lts
```

然后使用官方脚本安装。

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

因为机器配置比较差，所以安装的很慢。

这时候顺便去注册一个智谱的账号，然后在`控制台`->`API key`里生成1个api key。

因为我用的是没有ui的linux系统，所以没办法用千问的oauth，在模型这边就用智谱的GLM了。

安装完成之后的配置步骤跟mac上是一样，只是在选择模型的时候Z.AI，也就是GLM4.7。

把之前创建的API key拷贝一下，接下来参考mac的步骤，不配置channel，随便选几个skill安装一下，不配置各种api key。

因为没有检测到系统的gui，所以在安装完gateway之后，整个安装过程就结束了。

不过我们还是要打开gateway的dashboard测试一下。

先回到本机，用下面的命令进行ssh转发。

```
ssh -N -L 18888:127.0.0.1:18789 用户名@目标机器的ip
```

然后浏览器访问`http://127.0.0.1:18888/?token=1xxxxxxxxxf911e9f8ceea1ed572a11c`

这里的token可以在安装结束后的命令行里看到，或者在linux机器上运行命令`openclaw dashboard`来查看。

在浏览器的聊天窗口中测试一下，openclaw能回答就证明配置成功了。

## 总结

老实说，openclaw安装和配置都是有一定门槛的，对于普通用户来说，基本上很难自己搞定。

个人观点，openclaw代表的是AI应用的一种新的方向，它让AI 拥有近乎无限的记忆，并且能直接操控电脑，真正帮人类把事情做完。

但这并不意味着openclaw目前的交互方式是适合每一个人的。

最后总结一下，在我看来openclaw是一场程序员自己的狂欢，而对绝大多数的普通用户来说，可能更适合站在场边围观。它确实代表未来，但不是每个人都需要、也并不是每个人都必须现在就去学它和使用它。

## 附录:clawedbot改名始末

大家好，今天来聊聊Clawdbot那段让人又好笑又心酸的改名史，简直是2026年AI圈最戏剧化的一幕。

故事是这样的：

就在前几天，一款超级能干的开源AI个人助理突然爆火，GitHub星标狂飙好几万，还顺手把Mac Mini都带到断货。它叫**Clawdbot**，名字灵感来自Claude Code里那个像素风小龙虾，Clawd就是它的吉祥物，读起来有点像“Claude bot”，龙虾爪子🦞+机器人，挺可爱的致敬嘛。

结果火得太快，火到被正主盯上了。

1月27日，Anthropic的法务直接找上门：你们这名字和Logo太像我们家Claude了，商标有问题，赶紧改！

创始人Peter Steinberger一开始还想挣扎，说“要不我把d去掉改Clawbot行不行？”——对方：不行。

一番拉扯后，Peter只能举白旗：“好吧，被迫改名，这不是我的决定。”

但这哥们儿脑子转得快，顺着龙虾的生物属性直接玩了个高级自黑：

龙虾要长大就得**蜕壳（Molt）**，对吧？那就叫**Moltbot**好了！

吉祥物也从Clawd变成**Molty**，Logo跟着更新。

官方公告一句话总结得绝了：

“Same lobster soul, new shell.”  
（同样的龙虾灵魂，换了新壳。）

结果改名当天更魔幻：

GitHub账号重命名搞砸了，X账号改名那10秒钟空窗期，直接被加密货币骗子抢注去发诈骗链接……

Peter在推上连发几条吐槽：“我只是想改个名，怎么比写代码还难？”

所以Clawdbot的改名史，浓缩起来就一句话：

一只想好好做事的龙虾AI，因为名字太像大佬家的产品，被逼着当众蜕了一次壳，

蜕完之后不但没萎，反而用最幽默的方式告诉大家：

“我还是那个能干活的家伙，只是……壳换了个更酷的版本而已🦞→🦐”
