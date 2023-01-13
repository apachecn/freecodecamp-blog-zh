# Linux 用户组讲解:如何添加新组、新组成员和更改组

> 原文：<https://www.freecodecamp.org/news/linux-user-groups-explained-how-to-add-a-new-group-a-new-group-member-and-change-groups/>

Linux 允许多个用户同时访问系统。设置权限可以防止用户之间相互攻击。可以将用户分配到为共享特权、安全性和访问权限的用户创建的组中。可以基于特定用户或用户组授予文件和设备访问权限。

组通常用于授予成员修改文件或目录的特定权限。

两种主要类型的组是初级组和次级组。用户的主要组是帐户关联的默认组。用户创建的目录和文件将具有该组 ID。次要组是除主要组之外用户所属的任何组。

## **创建群组**

让我们创建两个组，称为“作家”和“编辑”。像这样使用`groupadd`命令(您可能必须在开始时使用`sudo`,这样您就有适当的权限来创建一个组):

```
groupadd writers
groupadd editors
```

## **创建用户**

您可能已经有用户要添加到您的群中。如果没有，下面是使用`useradd`命令创建用户的基本语法:

`useradd [options] username`

下面是创建名为“quincy”的用户的命令。`-m`将创建用户的主目录来匹配用户名。`-p p4ssw0rd`为“p4ssw0rd”的用户创建一个密码。

`useradd -m quincy -p password`

用户可以使用`passwd`命令更改他们的密码。他们必须输入当前密码，然后输入新密码。

## **向群组添加用户**

您可以使用`usermod`命令将用户添加到组中。下面是如何将用户“quincy”添加到组“writers”中。`-a`参数表示“追加”，而`-G`参数将一个组添加为次要组。

`usermod -a -G writers quincy`

当使用`adduser`命令创建一个用户时，该用户被自动分配到一个与用户名同名的主组。因此，目前用户“quincy”有一个主要组“quincy”和一个次要组“writers”。

您还可以通过用逗号分隔组名，将用户一次添加到多个组中。`-G group1,group2,group3`。

以下命令将用户 quincy 的主要组更改为“editors”:

`usermod -g editors quincy`

## 从次要组中删除用户

要从辅助组中删除用户，您需要用一组不包含要删除的组的新组覆盖用户的当前组。

首先，使用`id`命令检查用户属于哪个次级组:

`id -nG quincy`

假设这返回了`editors writers`,表明 quincy 是“编辑”和“作者”组的一部分。如果要删除“writers”组，请使用以下命令:

`usermod -G editors quincy`

该命令将 quincy 的辅助组设置为“editors”。由于没有使用`-a`标志，先前的组集被覆盖。

## 结论

现在，您应该可以开始管理用户和组了。下一步是确定每个组将拥有哪些特权。