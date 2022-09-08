---
title: 跨浏览器测试介绍
slug: Learn/Tools_and_testing/Cross_browser_testing/Introduction
---
{{LearnSidebar}}{{NextMenu("Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies", "Learn/Tools_and_testing/Cross_browser_testing")}}

本文是对跨浏览器测试的入门概述，帮助了解“什么是跨浏览器测试？”，“常见的问题都有哪些？”，以及“应该怎么测试，识别和修复问题？”

<table class="learn-box standard-table">
  <tbody>
    <tr>
      <th scope="row">阅读基础：</th>
      <td>
        熟悉 <a href="/en-US/docs/Learn/HTML">HTML</a>,
        <a href="/en-US/docs/Learn/CSS">CSS</a>, 和
        <a href="/en-US/docs/Learn/JavaScript">JavaScript</a> 语言。
      </td>
    </tr>
    <tr>
      <th scope="row">目标：</th>
      <td>了解跨浏览器测试的高级概念。</td>
    </tr>
  </tbody>
</table>

## 什么是跨浏览器测试？

跨浏览器测试（cross browser testing）是确保您的网站或 web 应用能在可接受数量的浏览器（across an acceptable number of web browsers）上正常使用的测试方法。作为网站开发者，您有责任确保项目能供所有用户使用，无论他们使用的是哪种浏览器，设备或辅助工具。您需要注意的点：

- 除了您在工作中经常使用的一两种浏览器，还有一些老旧的浏览器也会有用户在使用。这些浏览器对 CSS 和 JavaScript 的新特性支持的不够。
- 不同的设备支持的功能也不一样，有功能强大的新平板电脑，智能手机，智能电视，也有功能不全的廉价平板电脑，老旧手机。
- 残疾人士通过屏幕阅读器等辅助技术上网，可能不会使用鼠标（有些人只使用键盘）。

请记住，您不能代表产品的用户 - 您的网站能适配 Macbook Pro 或高端 Galaxy Nexus，并不意味它适用于所有用户 - 还有很多测试工作要做！

> **备注：** [Make the web work for everyone](https://hacks.mozilla.org/2016/07/make-the-web-work-for-everyone/) 文章列出了浏览器的市场份额，使用情况和相关兼容性问题。

我们先解释下术语。首先，我们所讨论的“跨浏览器使用（working cross browser）”，应该在不同浏览器中提供可接受的用户体验。虽然无法在所有浏览器上提供相同的体验，但确保核心功能使用顺畅就算可以。比如在现代浏览器上，能显示动画、3D 或闪光效果，而在较旧的浏览器上，可以呈现出相同信息的平面图片。只要网站主满意，你的工作就算完成了。

另一方面，视力正常的用户能正常浏览内容，但视力障碍的用户却因为屏幕阅读器无法读取信息而无法阅读内容。这是糟糕的体验，需要您能兼容屏幕阅读器软件。

其次，当我们说“可接受数量的浏览器（across an acceptable number of web browsers）” ，并不是说世界上 100% 的浏览器，这也是不可能。您可以通过信息收集了解用户都在使用哪些浏览器和设备，但也不能保证全都采集到（也是本专题第二篇所讨论的 — 参见 [要测试全部的吗？（Gotta test 'em all?）](/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies#Gotta_test_%27em_all)）。作为 web 开发者，您自然要确保网站主要求的浏览器都能正常工作，但除此之外，您需要防御性编程（code defensively），尽可能让其它浏览器也能正常查看内容。这是 Web 开发的重大挑战之一！

> **备注：** 后面会详细介绍防御性编程（code defensively）

## 为什么会出现跨浏览器问题？

对于为何出现跨浏览器问题，原因有很多，而且要注意，我们在此讨论的是跨不同浏览器/设备/浏览偏好时出现的表现差异的问题。你应该在遇到跨浏览器问题之前就预先修复你代码里的 Bug（如需巩固记忆，请参阅之前主题中的[Debugging HTML](/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Debugging_HTML)，[Debugging CSS](/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Debugging_CSS)，以及[What went wrong?Troubleshooting Javascript](/zh-CN/docs/Learn/JavaScript/First_steps/What_went_wrong)）。

跨浏览器问题会出现通常因为：

- 有时候浏览器有缺陷，或者实现功能的方式不同。这种情况比以前大有改观；在 IE 4 和 Netscape 4 争霸的二十世纪九十年代，浏览器厂商故意采用与其他厂商不同的方式开发功能，以此获得竞争优势，这使得开发者如陷地狱。近来各个浏览器更加注重遵循标准，但差异和缺陷仍会时而出现。
- 一些浏览器对一些技术特点的支持力度与其他浏览器不同。当你在处理一些浏览器厂商正在实现的前沿技术点，或你必须支持一些很古老的不再更新维护的浏览器时，跨浏览器问题会不可避免的出现。举个例子：你想在自己网站里用到的一些前沿 Javascript 技术在旧版浏览器中可能不会生效。如果你需要支持旧版浏览器，你可能必须放弃新技术，或在所需之处把代码转换成过时语法。
- 一些设备可能会制定一些限制来防止网站运行缓慢或显示效果糟糕。例如：若一个网站的设计效果在 PC 端很棒，在移动设备上就会显得很小，很不易于浏览。若你的网站要加载体积很大的动画，在高配设备上可能没问题，但在低配置的设备上就会迟钝又愚蠢。

此外还有很多原因。

在后续文章中，我们会探究常见的跨浏览器问题，并寻求解决方案。

## 跨浏览器测试的工作流

这一系列跨浏览器测试工作也行听起来耗时且可怕，但实际上不是——你只需仔细计划，并确保在恰当之处做足测试以防遭遇意外故障。假如你正在开发一个大型项目，你应该有规律地进行测试，确保新功能正确地服务你的目标用户，以及新增功能不会与已有功能冲突。

如果你在项目中把所有测试工作留到最后，那么会比随时发现和解决 Bug 耗时更长，成本更高。

一个项目的测试和排错工作流可以大致分为如下四个阶段（这只是粗略划分——因人而异）：

**初步规划>开发>测试/查错>修复/迭代**

步骤 2 到步骤 4 在必要时应多次重复直到开发完成。我们会在后续章节详细探讨测试程序的不同之处，但现在我们只概述每个阶段可能出现的问题。

### 初步规划

在初步规划阶段，你会和网站主人/客户（可能是你的老板或来自客户公司的人）开一些计划会议，你们会决定这个网站到底是什么——应该有什么内容和功能、什么样的外观等等。此时你也会想知道开发周期是多长——何时是他们的截止日期、他们会支付你多少酬劳。我们不会深入这些细节，但跨浏览器问题会大大影响这个计划。

一旦你了解了需求，确定了技术选型，你就该开始调查目标用户——使用这个网站的用户用什么浏览器、什么设备等等。也行客户通过前期调查已经有了这方面的数据，比如通过他们的其他网站，或你将要开发的网站的之前版本。若没有，你将要通过搜寻其他资源，诸如竞品的使用数据，或网站将要服务的国家来了解清楚。也可以凭直觉。

举个例子，你要开发一个服务北美客户的电子商务网站。该网站应该完美支持最流行的 PC/移动端（iOS、Android、Windows phone）浏览器的最新几个版本——应包括 Chrome（和相同渲染引擎的 Opera）、Firefox、IE/Edge 以及 Safari。它还应为 IE 8 和 9 提供可接受的体验，以及遵循 WCAG AA 指南。

现在你已知道你的目标测试平台，你应回顾功能需求和技术选型。例如：如果这个电子商务站点的拥有者想要在页面上实现基于 WebGl 的 3D 产品展示，那他们必须放弃 IE 11 及以前的版本。你要承诺为低版本用户提供没有 3D 展示的版本。

你应该列出这些潜在的问题。

> **备注：** 你可在 MDN——就是本网站——找到浏览器对不同技术功能的支持情况的信息。

一旦你们就这些问题达成共识，你就可以开始开发网站了。

### 开发

现在到了开发阶段。你应该把开发分成不同模块，例如你可以分成首页、产品页、购物车、支付流程等。然后你可以再进行细分——实现公共的页头页脚，实现产品页细节视图，实现购物车组件等。

有一些跨浏览器开发的普适策略，如：

- 让所有功能尽可能在所有目标浏览器上运行。这也许涉及到写不同的代码，让不同浏览器中的功能以不同方式实现，或使用一个`{{glossary("Polyfill")}}`模拟缺失的支撑，或者使用一个库，允许你写一点代码，然后根据浏览器支持的内容在后台执行不同的操作。
- 接受这个现实：一些东西在不同浏览器运行效果不同，在不支持完整功能的浏览器中提供不同的（可接受的）的解决方案。因为设备限制，有时这是不可避免的——不管你怎样设计网站，在影院宽屏上和在 4 寸屏上的视觉效果是不同的。
- 接受这个现实：你的网站在旧版浏览器上不能用，那就忽略旧版。假设你的客户/用户认为可以就没事。

一般来说你的开发会涉及到一个上述三个方法的结合。最重要的是你在提交每个小部分之前都要测试——别把测试放到最后！

### 测试/查错

在每个实现阶段之后，你需要测试这些新功能。要开始测试，你应该确保你的代码没有能够阻止功能运行的一般性错误：

1. 在一些稳定浏览器中测试，例如 Firefox、Safari、Chrome 或 IE/Edge。
2. 做一些低可用性测试，比如尝试只用键盘使用网站，或通过屏幕阅读器访问，来检查可操纵性。
3. 在移动端测试，例如 iOS 或 Android。

此时，你要修复一切发现的问题。

接下来，你应该在所有浏览器上测试并集中精力排除跨浏览器问题（关于[determining your target browers](/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies#Gotta_test_%27em_all)，请查阅下一篇文章获取更多信息）。例如

- 尝试在所有现代桌面浏览器上测试最新的修改——包括（理想条件下，Windows、Mac、Linux 的）Firefox、Chrome、Opera、IE、Edge 和 Safari。
- 在常用的手机和平板浏览器中测试（例如 iPhone/iPad 上的 iOS Safari、iPhone/iPad/Android 的 Chrome 和 Firefox）。
- 也要在你所列出的其他目标浏览器中测试。

最起码你要独自进行你能实现的所有测试（如果有团队，你可以拉队友来帮你）。你应该尽可能在真实物理设备上进行测试。

如果你无法测试所有那些不同的浏览器、操作系统和物理真机，你也可以用模拟器（在 PC 端用软件模拟设备）和虚拟机（能使你在 PC 上模拟多种操作系统/软件的软件）。这是很普遍的做法，特别是在某些环境中——例如，Windows 不允许你在同一台机器上模拟安装多个版本的 Windows，那么使用虚拟机就是唯一选择。

另一个选择是用户群组——使用开发团队之外的一组人测试你的网站。可以是一些朋友或家庭，其他员工们，一个班级或一个当地大学，或一个专业测试团队。

最终，通过使用统计或自动化工具，你的测试会更智能；当你的项目变得更大，这是个明智的选择，毕竟手动做全部这些测试会花很长时间。你可以建立你的自动化测试系统（Selenium 正在成为最流行选择），这样你就可以实现诸如在一些不同浏览器加载你的网站的功能，比如：

- 看一个按钮点击事件是否成功生效（例如显示地图），测试完成后就显示结果。
- 逐个截屏，允许你检查每个布局在跨浏览器中是否显示一致。

如果你愿意，你可以为测试投入一些资金，诸如[Sauce Labs](https://saucelabs.com/)和[Browser Stack](https://www.browserstack.com/)的商业工具都可以为你做这些事，那你就不用操心如何搭建测试环境了。亲自搭建一个属于你自己的测试环境也可以，这样以后你只要把更改后并通过了测试的代码提交到中央代码仓库就可以了。

#### 在预发布的浏览器上测试

测试即将发布的浏览器版本也是个不错的主意。请看下面的链接：

- [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)
- [Edge Insider Preview](https://insider.windows.com/)
- [Safari Technology Preview](https://developer.apple.com/safari/technology-preview/)
- [Chrome Canary](https://www.google.com/chrome/browser/canary.html)
- [Opera Developer](http://www.opera.com/computer/beta)

假如你使用了很新的技术并想测试最新版本的实现，或者你在最新版的浏览器中遇到故障，你想检查浏览器开发者是否在新版本中修复了这个问题，这会是很普遍的做法。

### 修复/迭代

当你发现一个 Bug，你需要尝试修复它。

首要之举是尽可能锁定 Bug 出现之处。从 Bug 报告者那里尽可能多的获得信息——平台、设备、浏览器版本等等。在相似环境（比如不同桌面平台的同一个浏览器版本，或同一平台中同一浏览器的小差别版本）中测试来检查该 Bug 波及多大范围。

那可能不是你的错误——如果是一个浏览器的 Bug，那就希望厂商尽快修复它。也许它已经被修复了——例如若一个 Bug 存在于 Firefox 版本 49，但 Firefox Nightly 中已经不存在了。如果还未修复，你可能需要提交一个 Bug 报告。

如果是你的错误，你需要修复它！查出导致该 Bug 的原因（再次，查阅[Debugging HTML](/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Debugging_HTML)，[Debugging CSS](/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Debugging_CSS)，和[What went wrong? Troubleshooting JavaScript](/zh-CN/docs/Learn/JavaScript/First_steps/What_went_wrong)）。当你找到原因，你需要决定如何在产生问题的浏览器中解决它——你不能直接改掉问题代码，因为这会在其他浏览器中导致问题。普遍做法是以某种方式分叉代码，例如用 Javascript 功能检测代码来检测问题功能不运行的情况，并运行一些在那些情况下生效的代码。

一旦完成修复，你应该重新测试来确保你的修复工作有效，并且没有导致网站的其他地方在其他浏览器中出问题。

## 报告 Bug

重申一遍，如果你发现浏览器 Bug，你应该：

- [Firefox Bugzilla](https://bugzilla.mozilla.org/)
- [EdgeHTML issue tracker](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/)
- [Safari](https://bugs.webkit.org/)
- [Chrome](https://bugs.chromium.org/p/chromium/issues/list)
- [Opera](https://bugs.opera.com/wizard/desktop)

## 总结

本文应该能够让你对跨浏览器测试最重要的部分有一个更高层次的理解。有了这些知识的武装，你现在可以继续并开始学习跨浏览器测试策略了。

{{NextMenu("Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies", "Learn/Tools_and_testing/Cross_browser_testing")}}

## 指南

- [跨浏览器测试简介](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction)
- [测试策略](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies)
- [处理常见的 HTML 和 CSS 问题](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS)
- [处理常见的 JavaScript 问题](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/JavaScript)
- [处理常见的无障碍问题](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility)
- [实现特征检查](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/Feature_detection)
- [自动测试简介](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing)
- [建立你自己的自动化测试环境](/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment)
