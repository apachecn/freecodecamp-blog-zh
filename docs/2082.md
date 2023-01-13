# 如何进行谷歌表单调查——谷歌表单教程

> 原文：<https://www.freecodecamp.org/news/how-to-make-a-google-form-survey-google-forms-tutorial/>

Google Forms 是一个有用的工具，可以让您创建具有各种定制选项的调查。在本指南中，我们将看到制作和定制您自己的 Google 表单的最常见方法。

## 从模板开始

当您准备好创建一个新的调查时，您可以选择从一个空白文档开始，或者从许多可用的模板之一开始。

这些模板分为三类:个人、工作和教育。有现成的可以使用，让您不必自己设计表单，例如，客户反馈表单或派对邀请函。

![image-36](img/6cde089d4fc03f5383def6a98b6504ca.png)

In the image you can see examples of the templates available when creating from a model.

## Google 表单的一般功能

在页面的右上角有一些按钮，可以访问设置和定制选项。

![image-58](img/6914399d122a378a99259e43b9b5af93.png)

From left to right the buttons are Customize Theme, Preview, Settings, Send, More, Google Account

### 设置

这些设置允许您自定义各种功能，例如

*   是否收集了受访者的电子邮件地址
*   如果回答者可以稍后返回来更改他们的答案
*   如果他们可以多次提交或只提交一次(在这种情况下，回答者必须使用他们的帐户登录)
*   如果它显示进度条，并且
*   如果问题被随机打乱。

![image-57](img/59fb5a227cae7cec221baa5cfdb66c6f.png)

General tab in the Settings with options: Collect email addresses, Limit to 1 response, Edit after submit, See summary charts and text responses.

![image-59](img/d5ab1a45637d437a2a8eb2b0884b8bc3.png)

Presentation tab in Settings with options: Show progress bar, Shuffle question order, Show link to submit another response.

### 自定义主题

您还可以使用各种选项自定义模板的主题，如更改表单中使用的主色、背景色和字体。

您还可以添加标题图像，上传一个，或在许多可用选项中进行选择。

![image-60](img/8f961d7ee9a4fd744e71eca432c5b196.png)

Theme Option menu, with options of changing header image, theme color, background color, font style.

## Google 表单问题和问题类型

您可以使用右侧浮动菜单中的第一个按钮添加新问题。每个问题都可以定制标题和描述(通过问题的三点菜单)，也可以定制图像或视频。

![image-72](img/4f774fb9562adb5fe7bd001897ec2f52.png)

The floating menu, with the Create Question button marked

您还可以根据需要设置每个问题，要求回答某些问题。这样的话，不填那个答案是不可能提交表单的。对于某些问题类型，还可以自定义响应验证。

![image-62](img/4bcaa881f451600369e22c21b6cc45ee.png)

Three dots menu on questions, with Description and Response validation menu items.

有各种可能的问题，我将在下面逐一描述。

### 简答形式问题

简答题允许单行回答。从三点菜单中，可以验证这个答案:

*   作为一个数字，并且还具有允许数字的各种可能的约束，
*   作为文本，约束它是否包含某些内容，
*   作为 URL 或电子邮件地址，
*   使用具有最小或最大长度约束的长度，
*   使用正则表达式，这允许您进行个性化的模式验证(这个 [Google 支持页面上的正则表达式语法](https://support.google.com/a/answer/1371415?hl=en)可能很有用)，

您可以设置自定义错误消息，以便在答案验证失败时显示。

![image-63](img/eb4c2f4573e8c6b85027bb86ca74fde2.png)

The response is being validated to be a whole number, and a custom error message of "Please use a whole number" will be shown in case the response fails validation.

### 段落形式问题

段落问题允许多行文本回答。它可以用最小或最大长度或正则表达式进行验证，并且您可以设置一个自定义消息来显示验证是否失败。

![image-71](img/2e935155931ac0e21edc15a6468b689b.png)

Paragraph form question creation, the response is being validated by length, a minimum character count of 160 characters is required. If the validation fails the message "Please write more than a tweet" is displayed.

### 多项选择、复选框和下拉式表单问题

这三类问题让回答者在多个预先写好的选项中选择。多选或下拉允许选择一个答案，而复选框允许回答者选择多个选项。

多项选择和下拉菜单的区别在于，在下拉菜单中，所有选项都隐藏在菜单中，直到它被选中。在多项选择中，所有选项总是可见的。

复选框和多项选择都允许“其他”选项，回答者可以填写他们想要的内容。在所有这些类型的问题中，选项顺序是可以打乱的。

![image-39](img/71a4053169fd47af242100026923c279.png)

Screenshot of the process of creating a multiple choice question.

### 文件上传表单问题

这些问题允许用户将文件上传到表单所有者的 Google Drive。添加此问题后，回答者必须使用他们的 Google 帐户登录。

对于这类问题，您需要确认您同意让其他人访问您的 Google Drive。

![image-64](img/d4cb08cf8415073798da8bd2d7df0611.png)

The message that appear when creating a File upload question.

![image-65](img/b009422ba66bac9b618215e80f1d021d.png)

FIle upload question settings.

您可以对可以上传的文件、文件大小以及是否可以一次上传多个文件进行限制。

*   **仅允许特定的文件类型**:打开此选项将允许您选择接受哪些文件类型。

![image-66](img/fb0ea78f8b7d4c54133c59ea04572894.png)

*   **最大文件数量**:此下拉菜单允许您选择一次上传 1、5 到 10 个文件。
*   **最大文件大小**:您可以选择 1 MB、10 MB、100 MB、1 GB、10 GB 的最大文件大小。
*   该表单最多可接受 1 GB 的文件。更改:按下“更改”将调出一个设置区，您可以在这里更改从该表单上传的文件可以占用多少内存。您可以在 1 GB、10 GB、100 GB 和 1 TB 之间选择。一旦达到大小限制，表单将停止接受答案。

![image-67](img/87b9db3d701dfde7f2c4561844564344.png)

In the General tab of the Settings you can set the maximum size of files collected. This section appear only if this kind of question is present in the form.

### 线性量表形式问题

这类问题从 1 或 0 开始评分，最多 10 分。受访者将在量表上选择一个他们认为最能反映他们想法的点。

![image-40](img/fea012ed3a2c75b49803a17ec4850032.png)

Screenshot of the process of creating a linear scale question.

![image-41](img/7d5c829d5e9163c9bcb9d03ffce53879.png)

This is how a linear scale question appears to the respondents.

### 多项选择网格和复选框网格问题

这些问题创建了一个网格，其中每一行都是多项选择或复选框问题。您可以将其设置为要求每行一个响应，和/或将响应者限制为每列一个响应(如果行数多于列数，则不要同时设置两者)。你也可以设置行的顺序来打乱。

![image-43](img/57ec2eae383b66501b09db7581372ddf.png)

Building a multiple choice grid question.

![image-44](img/794f757664ed543f1ec6e00a938a3c82.png)

How a multiple choice grid appears to the respondents.

### 日期和时间表单问题

日期型问题将让回答者插入一个日期。有包括或不包括年份、包括或不包括时间的选项。时间类型问题将让回答者插入时间或持续时间。

![image-68](img/6f50d847bf9494f32cb006f9016e2429.png)

Creating a Date question.

## 如何将表单分成几个部分

可以使用节将表格分成几页，每一节都单独显示给回答者。

您可以从页面右侧浮动菜单中的最后一个按钮创建一个新部分。从节标题附近的“三点”菜单中，您可以复制当前节，将其移动到文档中的另一个位置，或者删除它。您可以使用描述来自定义每个部分。

![image-73](img/5f49fc5e00499d4df5b73009841f9b1b.png)

The floating menu with the New section button marked.

### 如何在各部分之间导航

您可以这样做，在一个部分结束时，回答者将被重定向到不是按顺序排列的下一个部分。

您可以通过部分末尾的下拉菜单进行设置。

![image-74](img/5178c50366569b13b9c003ef038ca752.png)

Menu at the end of a section from which you can choose which is the next section.

或者，您可以使用多项选择或下拉式问题的设置，根据所选答案来决定进入哪个部分。如果一个回答者选择了一个具有重定向能力的答案，那就赢得了部分结束选项。

![image-75](img/3357e8846f4a802db482a82c524fea23.png)

Three dots menu of a question, you can give redirecring powers to the answers from "Go to section based on answer"

如果多个问题具有重定向能力，则最后一个问题决定发生什么重定向(如果问题 2 指示重定向到 C 部分，问题 4 指示重定向到 D 部分，则最后一个问题决定下一个访问的部分是 D 部分)。

## 如何在谷歌表单中显示答案

答案收集在创建表单的同一页面上的第二个选项卡中。可以选择在摘要中查看答案，在“问题”选项卡中按问题查看，或在“个人”选项卡中按回答者查看。

使用 Google 工作表按钮，您可以在工作表中自动更新答案。从三点菜单，更多的答案选项可用，如下载到一个`*.csv`文件，每次提交表格时激活电子邮件通知，或打印答案。

![image-38](img/318e1d78644ad85788741b85f6b83f28.png)

The top of the Responses tab

下图显示了一个选择题的摘要。与“其他”选项一起给出的答案也会出现在侧面的图例中。相同的答案不同的拼写将创建不同的条目，所以它将需要手动计数。

![image-42](img/49f9b54cf3e25bb899bad1203887465c.png)

Summary of a multiple choice question, a cake diagram where each slice represents the percentage of respondents choosing that option.

## 其他 Google 表单功能

### 盘问

您可以随时从设置中打开测验模式。这将为每种类型的问题提供更多选项，如自动评分，为每个问题提供分数，以及显示结果的反馈。

![image-45](img/148c8ec599c8246e200d5faf3809a28e.png)

Screenshot of the settings showing the toggle to activate quiz options.

当您使用问题块左下角的“答案”将表格设置为分级测验时，您可以添加分数和问题的正确答案。您还可以设置反馈，向回答者显示他们的测试结果。

### 更加复杂

使用 Google Apps 脚本(三点->脚本编辑器)或附加组件(三点->附加组件)的选项允许您更多地定制表单。

![image-70](img/67385a7176b23b965f484f2a32a888ce.png)

Three dots menu in the upper right corner, the Script editor and Add-ons item menu are near the bottom.

例如，您可以从任何 Google 表单的列中填充多项选择、列表、复选框和网格选项，或者您可以在一定数量的提交后关闭表单。你甚至可以(在测验模式下很有用)在表单中添加一个计时器，或者摄像头人脸识别作为反作弊措施。

## 结论

Google Forms 本身提供了很多定制选项。您可以创建复杂的数据收集调查或复杂的评分测验。随着脚本和附加组件复杂性的增加，几乎没有什么是不可企及的。