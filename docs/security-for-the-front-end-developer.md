# 如何设计安全的 Web 表单:验证、净化和控制

> 原文：<https://www.freecodecamp.org/news/security-for-the-front-end-developer/>

虽然网络安全通常被认为是数据库和架构方面的，但强大的安全态势在很大程度上依赖于前端开发人员领域的元素。

对于某些潜在的破坏性漏洞，如 [SQL 注入](https://www.owasp.org/index.php/Top_10-2017_A1-Injection)和[跨站脚本(XSS)](https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)) ，一个精心设计的用户界面是第一道防线。

这里是前端开发人员关注的几个领域，他们希望帮助打好这场仗。

## 控制用户输入

当开发人员构建一个不能控制用户输入的表单时，会发生很多疯狂的事情。为了对抗像注入这样的漏洞，验证或净化用户输入是很重要的。

您可以通过将输入约束为已知值来验证输入，比如在表单中使用[语义输入类型](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation#Semantic_input_types)或[验证相关属性](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation#Validation-related_attributes)。像 [Django](https://www.djangoproject.com/) 这样的框架也为此提供了[字段类型](https://docs.djangoproject.com/en/3.0/ref/models/fields/#field-types)。可以通过删除或替换上下文危险的字符来净化数据，比如使用白名单或对输入数据进行转义。

虽然这可能不直观，但即使是用户提交到网站上自己区域的数据也应该被验证。扩散最快的病毒之一是 MySpace 上的 Samy 蠕虫病毒(是的，我老了)，这要感谢 Samy Kamkar 能够将代码注入他自己的个人资料页面。没有经过彻底的验证或净化，不要直接返回任何输入到你的站点。

有关对抗注入攻击的更多指导，请参见 [OWASP 注入防御备忘单](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Injection_Prevention_Cheat_Sheet.md)。

## 小心隐藏的领域

添加`type="hidden"`是在页面和表单中隐藏敏感数据的一种非常方便的方法，但不幸的是这不是一种有效的方法。

有了像 ZapProxy 这样的工具，甚至是简单的网络浏览器中的检查工具，用户可以很容易地点击来揭示一些看不见的信息。

隐藏复选框可能是创建 CSS 专用开关的巧妙方法，但是隐藏字段对安全性没有什么帮助。

## 仔细考虑自动填充字段

当用户选择给你他们的[个人身份信息](https://en.wikipedia.org/wiki/Personal_data) (PII)，这应该是一个有意识的选择。自动填充表单域对用户和攻击者来说都很方便。[利用隐藏字段可以获得以前由自动完成字段捕获的 PII](https://freedom-to-tinker.com/2017/12/27/no-boundaries-for-user-identities-web-trackers-exploit-browser-login-managers/) 。

许多用户甚至不知道他们浏览器的自动填充功能储存了什么信息。尽量少用这些域，并对特别敏感的数据禁用自动填充表单。

权衡你的风险状况和权衡利弊也很重要。如果您的项目必须符合 [WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/) 标准，禁用自动完成功能会中断您对不同设备的输入。详见 [1.3.5:识别 WCAG 2.1](https://www.w3.org/WAI/WCAG21/Understanding/identify-input-purpose.html) 中的输入目的。

## 保持错误的一般性

虽然让用户知道数据是否存在似乎很有帮助，但这对攻击者也很有帮助。在处理账户、邮件和 PII 的时候，出错是最安全的(？)少在一边。不要返回“您的帐户密码不正确”，尝试更模糊的反馈“不正确的登录信息”，并避免透露用户名或电子邮件是否在系统中。

为了更有帮助，提供一个重要的联系方式，以防出现错误。避免透露不必要的信息。如果没有别的，看在上帝的份上，不要建议与用户输入非常匹配的数据。

## 做一个坏人

在考虑安全性时，后退一步，观察显示的信息，并问问自己恶意攻击者将如何利用它是有帮助的。唱反调。如果一个坏人看到这个页面，他们会获得什么新信息？风景中有 PII 吗？

问问你自己，对于一个真正的用户来说，页面上的所有东西是否都是必须的。如果没有，编辑或删除它。越少越安全。

## 安全从前门开始

如今，前端和后端的编码有了更多的重叠。要创建一个全面而安全的应用程序，对攻击者如何进入前门有一个大致的了解是有帮助的。