# 使用 Python 发送电子邮件

> 原文：<https://www.freecodecamp.org/news/send-emails-using-code-4fcea9df63f/>

阿琼·克里希纳·巴布

# 如何使用 Python 发送电子邮件

![1*qTKddOEkMZiri9ldfzBauQ](img/c401d322d6d4ef13dc706b5380044bcd.png)

作为一次学习练习，我最近钻研了 Python 3，看看如何发送一堆电子邮件。在生产环境中，可能有更简单的方法，但是下面的方法对我来说很有效。

所以，这里有一个场景:你有一堆联系人的名字和电子邮件地址。并且您想给这些联系人中的每一个都发送一条消息，同时在消息的顶部添加一个*“亲爱的[姓名]”*。

为了简单起见，您可以将联系信息存储在文件中，而不是数据库中。您也可以将想要发送的消息的模板保存在文件中。

Python 的 [smtplib](https://docs.python.org/3/library/smtplib.html) 模块基本上就是你发送简单邮件所需要的全部，没有任何主题行或者这样的附加信息。但是对于真正的*电子邮件，你确实需要一个主题行和大量信息——甚至可能是图片和附件。*

这就是 Python 的[电子邮件包](https://docs.python.org/3/library/email.html)的用武之地。请记住，单独使用`email`包发送电子邮件是不可能的。你需要同时拥有`email`和`smtplib`。

请务必查看这两者的全面官方文档。

以下是使用 Python 发送电子邮件的四个基本步骤:

1.  设置 SMTP 服务器并登录到您的帐户。
2.  创建`MIMEMultipart`消息对象，并用适当的标题为`From`、`To`和`Subject`字段加载它。
3.  添加您的邮件正文。
4.  使用 SMTP 服务器对象发送邮件。

现在让我向你介绍一下整个过程。

假设您有一个联系人文件`mycontacts.txt`，如下所示:

```
user@computer ~ $ cat mycontacts.txt
john johndoe@example.com
katie katie2016@example.com
```

每条线代表一个联系人。我们的名字后面是电子邮件地址。我用小写存储所有内容。如果需要的话，我会让编程逻辑将任何字段转换成大写或句子大小写。所有这些在 Python 中都很简单。

接下来，我们有消息模板文件`message.txt`。

```
user@computer ~ $ cat message.txt 

Dear ${PERSON_NAME}, 

This is a test message. 
Have a great weekend! 

Yours Truly
```

注意到“`${PERSON_NAME}`”这个词了吗？那是 Python 中的一个[模板字符串](https://docs.python.org/3.5/library/string.html#template-strings)。模板字符串可以很容易地用其他字符串替换；在这个例子中，`${PERSON_NAME}`将被替换为这个人的真实姓名，您很快就会看到这一点。

现在让我们从 Python 代码开始。首先，我们需要从`mycontacts.txt`文件中读取联系人。我们不妨把这位概括成它自己的函数。

```
# Function to read the contacts from a given contact file and return a
# list of names and email addresses
def get_contacts(filename):
    names = []
    emails = []
    with open(filename, mode='r', encoding='utf-8') as contacts_file:
        for a_contact in contacts_file:
            names.append(a_contact.split()[0])
            emails.append(a_contact.split()[1])
    return names, emails
```

函数`get_contacts()`将文件名作为其参数。它将打开文件，读取每一行(即每个联系人)，将其分为姓名和电子邮件，然后将它们添加到两个单独的列表中。最后，函数返回两个列表。

我们还需要一个函数来读入一个模板文件(如`message.txt`)并返回一个由模板内容组成的`Template`对象。

```
from string import Template

def read_template(filename):
    with open(filename, 'r', encoding='utf-8') as template_file:
        template_file_content = template_file.read()
    return Template(template_file_content)
```

就像前面的函数一样，这个函数以文件名作为参数。

要发送电子邮件，您需要使用 [SMTP(简单邮件传输协议)](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)。如前所述，Python 提供了处理这项任务的库。

```
# import the smtplib module. It should be included in Python by default
import smtplib
# set up the SMTP server
s = smtplib.SMTP(host='your_host_address_here', port=your_port_here)
s.starttls()
s.login(MY_ADDRESS, PASSWORD)
```

在上面的代码片段中，您导入了`smtplib`，然后创建了一个 [SMTP 实例](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP)，它封装了一个 SMTP 连接。它将主机地址和端口号作为参数，这两个参数完全取决于您的特定电子邮件服务提供商的 SMPT 设置。例如，在 Outlook 中，上面的第 4 行应该是:

```
s = smtplib.SMTP(host='smtp-mail.outlook.com', port=587)
```

你应该使用你的特定电子邮件服务提供商的主机地址和端口号，以使整个工作正常进行。

上面的`MY_ADDRESS`和`PASSWORD`是两个变量，用于保存您将要使用的帐户的完整电子邮件地址和密码。

现在是使用我们上面定义的函数获取联系信息和消息模板的好时机。

```
names, emails = get_contacts('mycontacts.txt')  # read contacts
message_template = read_template('message.txt')
```

现在，对于每个联系人，让我们分别发送邮件。

```
# import necessary packages
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

# For each contact, send the email:
for name, email in zip(names, emails):
    msg = MIMEMultipart()       # create a message

    # add in the actual person name to the message template
    message = message_template.substitute(PERSON_NAME=name.title())

    # setup the parameters of the message
    msg['From']=MY_ADDRESS
    msg['To']=email
    msg['Subject']="This is TEST"

    # add in the message body
    msg.attach(MIMEText(message, 'plain'))

    # send the message via the server set up earlier.
    s.send_message(msg)

    del msg
```

对于每个`name`和`email`(来自联系人文件)，您将创建一个 [MIMEMultipart](https://docs.python.org/3/library/email.mime.html#email.mime.multipart.MIMEMultipart) 对象，将`From`、`To`、`Subject`内容类型头设置为关键字字典，然后将消息正文以纯文本形式附加到`MIMEMultipart`对象。您可能希望阅读文档，以找到更多关于您可以尝试的其他 MIME 类型的信息。

还要注意，在上面的第 10 行，我使用 Python 中的[模板机制将`${PERSON_NAME}`替换为从联系人文件中提取的真实姓名。](https://docs.python.org/3.5/library/string.html#template-strings)

在这个特殊的例子中，我删除了`MIMEMultipart`对象，并在每次循环迭代时重新创建它。

一旦完成，您就可以使用您之前创建的 SMTP 对象的便利的 [send_message()](https://docs.python.org/3/library/smtplib.html#smtplib.SMTP.send_message) 函数来发送消息。

以下是完整的代码:

```
import smtplib

from string import Template

from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

MY_ADDRESS = 'my_address@example.comm'
PASSWORD = 'mypassword'

def get_contacts(filename):
    """
    Return two lists names, emails containing names and email addresses
    read from a file specified by filename.
    """

    names = []
    emails = []
    with open(filename, mode='r', encoding='utf-8') as contacts_file:
        for a_contact in contacts_file:
            names.append(a_contact.split()[0])
            emails.append(a_contact.split()[1])
    return names, emails

def read_template(filename):
    """
    Returns a Template object comprising the contents of the 
    file specified by filename.
    """

    with open(filename, 'r', encoding='utf-8') as template_file:
        template_file_content = template_file.read()
    return Template(template_file_content)

def main():
    names, emails = get_contacts('mycontacts.txt') # read contacts
    message_template = read_template('message.txt')

    # set up the SMTP server
    s = smtplib.SMTP(host='your_host_address_here', port=your_port_here)
    s.starttls()
    s.login(MY_ADDRESS, PASSWORD)

    # For each contact, send the email:
    for name, email in zip(names, emails):
        msg = MIMEMultipart()       # create a message

        # add in the actual person name to the message template
        message = message_template.substitute(PERSON_NAME=name.title())

        # Prints out the message body for our sake
        print(message)

        # setup the parameters of the message
        msg['From']=MY_ADDRESS
        msg['To']=email
        msg['Subject']="This is TEST"

        # add in the message body
        msg.attach(MIMEText(message, 'plain'))

        # send the message via the server set up earlier.
        s.send_message(msg)
        del msg

    # Terminate the SMTP session and close the connection
    s.quit()

if __name__ == '__main__':
    main()
```

这就对了。我相信代码现在已经相当清楚了。

如有必要，请随意复制和调整它。

除了官方的 Python 文档，我还想提一下[这个资源](http://naelshiab.com/tutorial-send-email-python/)，它对我帮助很大。

快乐编码:)

*我最初发表这篇文章[在这里](https://arjunkrishnababu96.gitlab.io/post/send-emails-using-code/)。如果你喜欢这篇文章，请点击下面的小心脏。谢谢！*