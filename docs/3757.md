# PHP 安全漏洞:会话劫持，跨站脚本，SQL 注入，以及如何修复

> 原文：<https://www.freecodecamp.org/news/php-security-vulnerabilities/>

## **PHP 的安全性**

当编写 PHP 代码时，记住以下安全漏洞以避免编写不安全的代码是非常重要的。

### **漏洞类型**

这些是您在编写 PHP 代码时会遇到的常见漏洞。我们将在下面更深入地讨论一些。

*   [跨站点请求伪造](https://guide.freecodecamp.org/php/security/cross-site-request-forgery)应用程序中的一个漏洞，由程序员不检查请求从哪里发出引起——这种攻击被发送给高权限级别用户，以获得对应用程序的更高级别访问。
*   [跨站点脚本](https://guide.freecodecamp.org/php/security/cross-site-scripting)应用程序中的一个漏洞，由程序员在将输入输出到浏览器之前没有净化输入(例如博客上的评论)引起。它通常用于在浏览器中运行恶意 javascript 来进行攻击，例如窃取会话 cookies 以及其他恶意操作，以获得应用程序中更高级别的权限。
*   [本地文件包含](https://guide.freecodecamp.org/php/security/local-file-inclusion)应用程序中的一个漏洞，由程序员要求用户提供一个文件输入，并且在访问所请求的文件之前没有对输入进行净化而导致。这导致一个文件被包含在不该包含的地方。
*   [远程文件包含](https://guide.freecodecamp.org/php/security/remote-file-inclusion)应用程序中的一个漏洞，由程序员要求用户提供一个文件输入，并且在访问所请求的文件之前没有对输入进行净化而导致。这将导致文件从远程服务器中取出，并包含在不应该出现的位置。
*   [会话劫持](https://guide.freecodecamp.org/php/security/session-hijacking)由攻击者获得用户会话标识符的访问权限并能够使用另一个用户的帐户冒充他们而导致的漏洞。这通常用于获得管理用户帐户的访问权限。
*   [会话标识符获取](https://guide.freecodecamp.org/php/security/session-identifier-acquirement)会话标识符获取是由攻击者造成的漏洞，攻击者能够猜测用户的会话标识符或利用应用程序本身或用户浏览器中的漏洞来获取会话标识符。
*   [SQL 注入](https://guide.freecodecamp.org/php/security/sql-injection)应用程序中的一个漏洞，由程序员在将输入包含到数据库的查询中之前没有对其进行净化而导致。这导致攻击者对数据库拥有完全的读取权限，并且通常拥有写入权限。通过这种访问，攻击者可以做非常糟糕的事情。

现在让我们更详细地看看一些常见的漏洞。

## **会话劫持**

会话劫持是一种漏洞，由攻击者获得用户会话标识符的访问权限，并能够使用另一个用户的帐户冒充他们造成。这通常用于获得管理用户帐户的访问权限。

### **防御 PHP 中的会话劫持攻击**

为了防御会话劫持攻击，您需要对照存储的会话信息检查当前用户的浏览器和位置信息。下面是一个示例实现，可以帮助减轻会话劫持攻击的影响。它检查 IP 地址、用户代理，如果会话过期，则在会话恢复之前删除会话。

```
<?php
session_start();

// Does IP Address match?
if ($_SERVER['REMOTE_ADDR'] != $_SESSION['ipaddress'])
{
session_unset();
session_destroy();
}

// Does user agent match?
if ($_SERVER['HTTP_USER_AGENT'] != $_SESSION['useragent'])
{
  session_unset();
  session_destroy();
}

// Is the last access over an hour ago?
if (time() > ($_SESSION['lastaccess'] + 3600))
{
  session_unset();
  session_destroy();
}
else
{
  $_SESSION['lastaccess'] = time();
}
```

## **跨站点脚本**

跨站点脚本是 web 应用程序中的一种漏洞，它是由程序员在将输入输出到 web 浏览器(例如博客上的评论)之前没有净化输入而导致的。它通常用于在 web 浏览器中运行恶意 javascript 来进行攻击，例如窃取会话 cookies 以及其他恶意操作，以获得 web 应用程序中的高级权限。

### **跨站点脚本攻击示例**

一个博客允许用户用 HTML 标签来设计他们的评论，然而支持博客的脚本没有去掉允许任何用户在页面上运行 javascript 的`<script>`标签。攻击者可以利用这一点在浏览器中运行恶意的 javascript。他们可能让用户感染恶意软件，窃取会话 cookies，等等。

```
<script>
  alert('Cross Site Scripting!');
</script>
```

### **在 PHP 中保护您的网站免受跨站点脚本攻击**

在 PHP 中有两个主要的函数，`htmlspecialchars()`和`strip_tags()`，内置来保护自己免受跨站脚本攻击。

`htmlspecialchars($string)`函数将阻止一个 HTML 字符串呈现为 HTML，并在 web 浏览器中显示为纯文本。 ****htmlspecialchars()代码示例****

```
<?php
$usercomment = "<string>alert('Cross Site Scripting!');</script>";
echo htmlspecialchars($usercomment);
```

另一种方法是`strip_tags($string, $allowedtags)`函数，它删除所有的 HTML 标签，除了你已经加入白名单的标签。需要注意的是，使用`strip_tags()`函数时，你必须更加小心，这个函数不会阻止用户将 javascript 作为一个链接，你必须自己清理它。

****strip_tags()代码示例****

```
<?php
$usercomment = "<string>alert('Cross Site Scripting!');</script>";
$allowedtags = "<p><a><h1><h2><h3>";
echo strip_tags($usercomment, $allowedtags);
```

****设置 X-XSS-保护头:****

在 PHP 中，你可以发送`X-XSS-Protection`头，它将告诉浏览器检查反射的跨站脚本攻击，并阻止页面加载。这并不能防止所有的跨站脚本攻击，只是反射攻击，应该与其他方法结合使用。

```
<?php
header("X-XSS-Protection: 1; mode=block");
```

****编写自己的杀毒函数**** 另一个选择，如果你想对杀毒工作的方式有更多的控制，就是编写自己的 HTML 杀毒函数，这对 PHP 初学者来说是不推荐的，因为一个错误会使你的网站易受攻击。

### **使用内容安全策略保护您的网站免受跨站点脚本攻击**

防止跨站点脚本攻击的有效方法是使用内容安全策略，这可能需要对 web 应用程序的设计和代码库进行大量调整。

#### **将内容安全策略设置为 HTTP 头**

设置内容安全策略最常见的方式是直接在 HTTP 头中设置。这可以由 web 服务器通过编辑其配置或通过 PHP 发送来完成。

****HTTP 报头中设置的内容安全策略示例****

```
<?php
header("content-security-policy: default-src 'self'; img-src https://*; child-src 'none';");
```

#### **设置一个内容安全策略作为元标签**

您可以在页面的 HTML 中包含内容安全策略，并逐页进行设置。这种方法要求您在每一页上都进行设置，否则您将失去该策略的优势。

****HTML 元标签中设置的内容安全策略示例****

```
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-s
```

## **SQL 注入**

SQL 注入是应用程序中的一个漏洞，它是由程序员在将输入包含到数据库的查询中之前没有净化输入而引起的。这导致攻击者对数据库拥有完全的读取权限，并且通常拥有写入权限。通过这种访问，攻击者可以做非常糟糕的事情。

### **SQL 注入攻击示例**

下面的 PHP 脚本运行一条 SQL 语句，通过 ID 获取用户的电子邮件。但是，输入没有经过净化，因此容易受到 SQL 注入的攻击

```
<?php
$input = $_GET['id'];
$dbserver = "localhost";
$dbuser = "camper";
$dbpass = "supersecretcampsitepassword";
$dbname = "freecodecamp";

$conn = new mysqli($dbserver, $dbuser, $dbpass, $dbname);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$sql = "SELECT email FROM users WHERE id =" . $input;

$result = $conn->query($sql);

if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        echo $row["email"];
    }
} else {
    echo "no results";
}

$conn->close();
```

```
SELECT email FROM users WHERE id = `$input`;
```

因此，上面的输入不是类型强制转换的(即，用(int)强制转换输入，所以只允许一个数字),也不会被转义，从而允许某人执行 SQL 注入攻击——例如，URL `getemailbyuserid.php?id=1'; My Query Here-- -`将允许您毫不费力地运行任意 SQL 查询。

### **在 PHP 中保护您的网站免受 sql 注入攻击**

有几种方法可以保护你的网站免受 SQL 注入攻击。这些方法是白名单、类型转换和字符转义

****白名单:**** 白名单方法用于只有少量输入的情况。您可以在 PHP 开关中列出每个期望的输入，然后为无效输入设置一个默认值。您不必担心类型转换问题或字符转义旁路，但是允许的输入非常有限。它仍然是一个选项，见下面的例子。

```
<?php
switch ($input) {
  case "1":
    //db query 1
    break;
  case "2":
    //db query 2
    break;
  default:
    // invalid input return error
}
```

****类型转换:**** 类型转换方法通常用于使用数字输入的应用程序。简单地用`(int) $input`对输入进行造型，只允许一个数值。

****字符转义:**** 字符转义方式会对用户提供的引号、斜线等字符进行转义，以防止攻击。如果您使用 MySQL 服务器和 MySQLi 库来访问您的数据库，`mysqli_real_escape_string($conn, $string)`函数将接受两个参数，MySQLi 连接和字符串，并将正确地避开用户的输入以阻止 SQL 注入攻击。您使用的确切函数取决于您使用的数据库类型和 php 库。有关转义用户输入的更多信息，请查看 php 库的文档。

## 关于 PHP 的更多信息:

*   [PHP 最佳实践](https://www.freecodecamp.org/news/p/9a508d2b-fa35-4ac1-a15b-8bab8acc356d/)
*   [最佳 PHP 代码示例](https://www.freecodecamp.org/news/the-best-php-examples/)
*   [如何防止 PHP 服务器受到慢吞吞的攻击](https://www.freecodecamp.org/news/slow-loris-attack-using-javascript-on-php-server/)
*   [如何在 PHP 中设置本地调试环境](https://www.freecodecamp.org/news/set-up-xdebug-phpstorm-in-php5-7-6a8386304fc6/)