# Linux 终极指南——创建 Sudo 用户

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-linux-creating-a-sudo-user/>

`sudo`代表“超级用户 do”或“切换用户 do”，`sudo`用户可以使用 root/管理权限执行命令，甚至是恶意命令。小心你授予`sudo`权限的人——你实际上是把你房子的钥匙交给了他们。

在创建新的`sudo`用户之前，您必须先创建一个新用户。

## 如何创建新用户

### 使用`adduser`或`useradd`添加新用户

```
sudo adduser username
```

确保用您想要创建的用户替换`username`。另外，请注意，要创建一个新用户，您自己也必须是一个`sudo`用户。

### 使用`passwd`更新新用户的密码

```
sudo passwd username
```

强烈建议使用强密码！

## 授予新用户 Sudo 权限

创建新用户后，使用`usermod`命令将他们添加到适当的组。

### 在 Debian 系统(Ubuntu / Linux Mint / ElementryOS)上，将用户添加到`sudo`组

```
sudo usermod -aG sudo username
```

### 在基于 RHEL 的系统(Fedora / CentOS)上，将用户添加到`wheel`组

```
sudo usermod -aG wheel username
```

## 如何删除用户

要删除用户，请使用以下命令。

### 基于 Debian 的系统(Ubuntu / Linux Mint / ElementryOS)

```
sudo deluser username
```

### 基于 RHEL 的系统(Fedora / CentOS)

```
sudo userdel username
```

关于在 Linux 中创建新的`sudo`用户，这就是你需要知道的全部。记住，“权力越大，责任越大。”