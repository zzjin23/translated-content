---
title: 如何撰写和引用一个词汇表中的条目
slug: MDN/Writing_guidelines/Howto/Write_a_new_entry_in_the_glossary
original_slug: MDN/Contribute/Howto/Write_a_new_entry_in_the_Glossary
---
{{MDNSidebar}}

MDN [术语表](/zh-CN/docs/Glossary) 是一个定义所有被用于文档和编码的术语、行话和缩写的地方。对词汇表做出贡献是使 Web 更易于理解的简单方法。您不需要高水平的技术技能来编写词汇表条目，因为它们应该保持简单直接。

## 如何写一个条目

如果您正在寻找需要词汇表条目的主题，请查看词汇表首页末尾的[未撰写文档的条目清单](/zh-CN/docs/Glossary#Contribute_to_the_glossary)。请单击您希望创建的词汇条目的链接，进入”新的词汇表页面“准备新条目的创建。然后按照后面的步骤操作。

如果您有添加新的词汇表条目的想法，只需在新选项卡中打开以下按钮，然后按照按钮下方的步骤操作：

[向术语表中添加一个新条目](/zh-CN/docs/new?parent=4391)

### 第一步：写概要

任何词汇表页面的第一段是对该术语的简单和简短描述（最好不超过两个句子）。确保阅读说明的人能够立即了解定义的术语。

> **备注：** 请不要直接从其他地方复制和粘贴定义（尤其是维基百科，因为许可证版本范围较小，因此与 MDN 不兼容）。确保条目内容简单易懂是很重要的。值得花一些时间来撰写定义，而不是简单盲目地窃取其他地方的内容。这个词汇表应该是有用的新内容，而不是和别处重复的。

使用“链接到词汇表条目”工具可以方便读者直接看到词汇条目中的定义，不需要读者跳转到词汇条目的页面。（更多内容请浏览如何使用 \\{{Glossary}} 宏插入词汇表条目的链接。）

如果需要的话，你可以添加少量额外的段落，但是这很容易导致你写了一整篇文章的情况发生。写一个完整的文章很棒，但是请不要把它们整个放在词汇表中。如果你不确定这里是否适合放你的文章，你就要随时[在这里讨论](/zh-CN/docs/MDN/Community#Join_our_mailing_lists)。

### 第二步：扩展链接

最后，一个词汇表的条目应该总是有“了解更多”这个部分。这个部分应该包含帮助读者看得更深入的链接：探索发现更多细节、学习使用相关技术等。

我们建议您将链接分为以下三组：

- 基础知识
  - : 提供更多一般信息的链接。如：到[维基百科](https://zh.wikipedia.org/)的链接是一个很棒的点。
- 技术参考
  - : 链接到更深入的技术信息，在 MDN 或其他地方。
- 学习它
  - : 链接到教程、练习或任何其他材料，帮助读者学习使用定义术语背后的技术。

## 小建议

所以你想贡献，但你不知道需要定义哪些术语？ [这里是是建议列表](/zh-CN/docs/Glossary#Contribute_to_the_glossary)。点击一下，开始吧！

## 消除歧义

有时，根据上下文，术语有几个含义。要解决歧义，您必须遵循以下准则：

- 该术语的主页面必须是一个包含了 [`GlossaryDisambiguation`](https://github.com/mdn/yari/blob/main/kumascript/macros/GlossaryDisambiguation.ejs) 宏的消歧页面；
- 该术语具有定义给定上下文的术语的子页面。

我们以一个例子来说明。_signature_ 条目在至少三种不同的语境中具有不同的含义： _安全_、_函数_ 和 _电子邮件_。

1. [Glossary/Signature](/zh-CN/docs/Glossary/Signature) 页面是使用了 [`GlossaryDisambiguation`](https://github.com/mdn/yari/blob/main/kumascript/macros/GlossaryDisambiguation.ejs) 宏的消歧页面；
2. [Glossary/Signature/Security](/zh-CN/docs/Glossary/Signature/Security) 页面是定义安全上下文中签名的条目页面；
3. [Glossary/Signature/Function](/zh-CN/docs/Glossary/Signature/Function) 页面是定义函数签名的条目页面；
4. [Glossary/Signature/Email](/zh-CN/docs/Glossary/Signature/Email) 页面是定义电子邮件签名的条目页面。

## 如何使用 \\{{Glossary}} 宏

当人们可以在另一个文档访问到词汇的定义，而无需他们从正在阅读的内容进行跳转阅读词汇定义时，词汇表是最好用的。因此，我们敦促您随时使用 [`Glossary`](https://github.com/mdn/yari/blob/main/kumascript/macros/Glossary.ejs) 宏将词汇链接到词汇表：

| 宏                                                         | 结果                                                     | 备注                                                                            |
| ---------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------- |
| \\{{Glossary("browser")}}                         | {{Glossary("browser")}}                         | 当文本与要定义的术语匹配时，只需直接使用宏（不区分大小写）                      |
| \\{{Glossary("browser", "Web 浏览器")}}     | {{Glossary("browser","Web 浏览器")}}     | 要显示替代文本，请使用第二个参数来指定该文本                                    |
| \\{{Glossary("browser", "Web 浏览器", 1)}} | {{Glossary("browser","Web 浏览器",1)}} | 指定 `1` 作为可选的第三个参数，将链接显示为常规链接，而不是单纯的提示文本的样式 |

使用 \\{{Glossary}} 宏创建的链接将总是展示一个包含了词汇表条目的概要段落的提示文本（tooltip）。

### 使用方针

在许多情况下，在 MDN 上的任何地方使用该宏是非常安全的。但是，您应该谨慎处理的几种情况：

- 如果一个术语已经链接到 MDN 的另一部分，这样的情况请不要使用 \\{{Glossary}} 宏；
- 在文章部分（Section）中，对于相同的术语仅使用一次 \\{{Glossary}} 宏（_提示：一个部分总是以标题开始的_）。
