# Bashrc 定制指南——如何添加别名、使用函数等等

> 原文：<https://www.freecodecamp.org/news/bashrc-customization-guide/>

自定义您的。bashrc 文件可以大大改善你的工作流程，提高你的工作效率。

的。bashrc 是一个位于 Linux 主目录中的标准文件。在这篇文章中，我将向你展示有用的。bashrc 选项、别名、函数等等。

配置的主要好处。bashrc 文件有:

*   添加别名可以让您更快地键入命令，从而节省时间。
*   添加函数允许您保存和重新运行复杂的代码。
*   它显示有用的系统信息。
*   它定制 Bash 提示符。

## 如何入门编辑？bashrc

以下是编辑。带有文本编辑器的 bashrc 文件:

```
$ vim ~/.bashrc
```

您可以向 bash 历史添加日期和时间格式。

```
HISTTIMEFORMAT="%F %T "
```

```
# Output

$ history
 1017  20210228 10:51:28  uptime
 1019  20210228 10:52:42  free -m
 1020  20210228 10:52:49  tree --dirsfirst -F
 1018  20210228 10:51:38  xrandr | awk '/\*/{print $1}'
```

添加此行以忽略历史中的重复命令。

```
HISTCONTROL=ignoredups
```

要设置活动历史中的行数和保存在 Bash 历史中的行数，请添加这两行。

```
HISTSIZE=2000
HISTFILESIZE=2000
```

您可以将历史设置为追加，而不是覆盖 Bash 历史。 **`shopt`** 代表“外壳选项”。

```
shopt -s histappend
```

要查看所有默认的 shell 选项，请运行`shopt -p`。

```
# Output

$ shopt -p

shopt -u autocd                   
shopt -u assoc_expand_once        
shopt -u cdable_vars              
shopt -u cdspell                  
shopt -u checkhash                
shopt -u checkjobs                
shopt -s checkwinsize             
[...]
```

创建一些变量来为 Bash 提示符添加颜色，如下所示:

```
blk='\[\033[01;30m\]'   # Black
red='\[\033[01;31m\]'   # Red
grn='\[\033[01;32m\]'   # Green
ylw='\[\033[01;33m\]'   # Yellow
blu='\[\033[01;34m\]'   # Blue
pur='\[\033[01;35m\]'   # Purple
cyn='\[\033[01;36m\]'   # Cyan
wht='\[\033[01;37m\]'   # White
clr='\[\033[00m\]'      # Reset
```

这是给 Vim 爱好者的。这将允许您在命令行上使用 vim 命令。这总是我添加到我的. bashrc 的第一行。

```
set -o vi
```

## 如何在中创建别名？bashrc

您可以为经常运行的命令使用别名。创建别名将使您打字更快，节省时间并提高生产率。

创建别名的语法是`alias <my_alias>='longer command'`。要找出哪些命令适合用作别名，请运行此命令来查看您最常运行的前 10 个命令的列表。

```
$ history | awk '{cmd[$2]++} END {for(elem in cmd) {print cmd[elem] " " elem}}' | sort -n -r | head -10 
```

```
# Output

171 git
108 cd
62 vim
51 python3
38 history
32 exit
30 clear
28 tmux
28 tree
27 ls
```

因为我经常使用 Git，所以这将是一个很好的创建别名的命令。

```
# View Git status.
alias gs='git status'

# Add a file to Git.
alias ga='git add'

# Add all files to Git.
alias gaa='git add --all'

# Commit changes to the code.
alias gc='git commit'

# View the Git log.
alias gl='git log --oneline'

# Create a new Git branch and move to the new branch at the same time. 
alias gb='git checkout -b'

# View the difference.
alias gd='git diff'
```

以下是一些其他有用的别名:

```
# Move to the parent folder.
alias ..='cd ..;pwd'

# Move up two parent folders.
alias ...='cd ../..;pwd'

# Move up three parent folders.
alias ....='cd ../../..;pwd'
```

```
# Press c to clear the terminal screen.
alias c='clear'

# Press h to view the bash history.
alias h='history'

# Display the directory structure better.
alias tree='tree --dirsfirst -F'

# Make a directory and all parent directories with verbosity.
alias mkdir='mkdir -p -v'
```

```
# View the calender by typing the first three letters of the month.

alias jan='cal -m 01'
alias feb='cal -m 02'
alias mar='cal -m 03'
alias apr='cal -m 04'
alias may='cal -m 05'
alias jun='cal -m 06'
alias jul='cal -m 07'
alias aug='cal -m 08'
alias sep='cal -m 09'
alias oct='cal -m 10'
alias nov='cal -m 11'
alias dec='cal -m 12'
```

```
# Output

$ mar

     March 2021      
Su Mo Tu We Th Fr Sa 
    1  2  3  4  5  6 
 7  8  9 10 11 12 13 
14 15 16 17 18 19 20 
21 22 23 24 25 26 27 
28 29 30 31 
```

## 如何在中使用函数？bashrc

当别名不起作用时，函数对于更复杂的代码非常有用。

下面是基本的函数语法:

```
function funct_name() {
	# code;
}
```

这是您在目录中查找最大文件的方法:

```
function find_largest_files() {
    du -h -x -s -- * | sort -r -h | head -20;
} 
```

```
# Output

Downloads $ find_largest_files

709M	systemrescue-8.00-amd64.iso
337M	debian-10.8.0-amd64-netinst.iso
9.1M	weather-icons-master.zip
6.3M	Hack-font.zip
3.9M	city.list.json.gz
2.8M	dvdrental.tar
708K	IMG_2600.JPG
100K	sql_cheat_sheet_pgsql.pdf
4.0K	repeating-a-string.txt
4.0K	heart.svg
4.0K	Fedora-Workstation-33-1.2-x86_64-CHECKSUM
[...]
```

您还可以向 Bash 提示符添加颜色，并显示当前的 Git 分支，如下所示:

```
# Display the current Git branch in the Bash prompt.

function git_branch() {
    if [ -d .git ] ; then
        printf "%s" "($(git branch 2> /dev/null | awk '/\*/{print $2}'))";
    fi
}

# Set the prompt.

function bash_prompt(){
    PS1='${debian_chroot:+($debian_chroot)}'${blu}'$(git_branch)'${pur}' \W'${grn}' \$ '${clr}
}

bash_prompt 
```

![bash-prompt.png](img/2b33fd451824952972d5729e28806f27.png)

Custom Bash prompt

在历史记录中搜索以前运行的命令:

```
function hg() {
    history | grep "$1";
}
```

```
# Output

$ hg vim

305  2021-03-02 16:47:33 vim .bashrc
307  2021-03-02 17:17:09 vim .tmux.conf
```

这就是你如何用 Git 开始一个新项目:

```
function git_init() {
    if [ -z "$1" ]; then
        printf "%s\n" "Please provide a directory name.";
    else
        mkdir "$1";
        builtin cd "$1";
        pwd;
        git init;
        touch readme.md .gitignore LICENSE;
        echo "# $(basename $PWD)" >> readme.md
    fi
}
```

```
# Output

$ git_init my_project

/home/brandon/my_project
Initialized empty Git repository in /home/brandon/my_project/.git/
```

您也可以在命令行上获得天气报告。这需要包 **curl** ， **jq** ，以及来自 [Openweathermap 的一个](https://openweathermap.org/) [**API key**](https://openweathermap.org/api) 。阅读 [Openweathermap API 文档](https://openweathermap.org/api)，以便正确配置 URL，获取您所在位置的天气信息。

用以下命令安装 curl 和 jq:

```
$ sudo apt install curl jq

# OR

$ sudo dnf install curl jq
```

```
function weather_report() {

    local response=$(curl --silent 'https://api.openweathermap.org/data/2.5/weather?id=5128581&units=imperial&appid=<YOUR_API_KEY>') 

    local status=$(echo $response | jq -r '.cod')

	# Check for the 200 response indicating a successful API query.
    case $status in

        200) printf "Location: %s %s\n" "$(echo $response | jq '.name') $(echo $response | jq '.sys.country')"  
             printf "Forecast: %s\n" "$(echo $response | jq '.weather[].description')" 
             printf "Temperature: %.1f°F\n" "$(echo $response | jq '.main.temp')" 
             printf "Temp Min: %.1f°F\n" "$(echo $response | jq '.main.temp_min')" 
             printf "Temp Max: %.1f°F\n" "$(echo $response | jq '.main.temp_max')" 
            ;;
        401) echo "401 error"
            ;;
        *) echo "error"
            ;;

    esac

}
```

```
# Output

$ weather_report

Location: "New York" "US"
Forecast: "clear sky"
Temperature: 58.0°F
Temp Min: 56.0°F
Temp Max: 60.8°F
```

## 如何打印系统信息？bashrc

当您像这样打开终端时，您可以显示有用的系统信息:

```
clear

printf "\n"
printf "   %s\n" "IP ADDR: $(curl ifconfig.me)"
printf "   %s\n" "USER: $(echo $USER)"
printf "   %s\n" "DATE: $(date)"
printf "   %s\n" "UPTIME: $(uptime -p)"
printf "   %s\n" "HOSTNAME: $(hostname -f)"
printf "   %s\n" "CPU: $(awk -F: '/model name/{print $2}' | head -1)"
printf "   %s\n" "KERNEL: $(uname -rms)"
printf "   %s\n" "PACKAGES: $(dpkg --get-selections | wc -l)"
printf "   %s\n" "RESOLUTION: $(xrandr | awk '/\*/{printf $1" "}')"
printf "   %s\n" "MEMORY: $(free -m -h | awk '/Mem/{print $3"/"$2}')"
printf "\n"
```

输出:

![Screenshot-2021-03-15-23-39-29.png](img/46c689913715cbf3b279986dfbb5b8f4.png)

获取。bashrc 文件以使更改生效:

```
$ source ~/.bashrc
```

这些都是习俗。bashrc 设置在一起。在新系统上，我将任何定制粘贴到。bashrc 文件。

```
######################################################################
#
#
#           ██████╗  █████╗ ███████╗██╗  ██╗██████╗  ██████╗
#           ██╔══██╗██╔══██╗██╔════╝██║  ██║██╔══██╗██╔════╝
#           ██████╔╝███████║███████╗███████║██████╔╝██║     
#           ██╔══██╗██╔══██║╚════██║██╔══██║██╔══██╗██║     
#           ██████╔╝██║  ██║███████║██║  ██║██║  ██║╚██████╗
#           ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝
#
#
######################################################################

set -o vi

HISTTIMEFORMAT="%F %T "

HISTCONTROL=ignoredups

HISTSIZE=2000

HISTFILESIZE=2000

shopt -s histappend

blk='\[\033[01;30m\]'   # Black
red='\[\033[01;31m\]'   # Red
grn='\[\033[01;32m\]'   # Green
ylw='\[\033[01;33m\]'   # Yellow
blu='\[\033[01;34m\]'   # Blue
pur='\[\033[01;35m\]'   # Purple
cyn='\[\033[01;36m\]'   # Cyan
wht='\[\033[01;37m\]'   # White
clr='\[\033[00m\]'      # Reset

alias gs='git status'

alias ga='git add'

alias gaa='git add --all'

alias gc='git commit'

alias gl='git log --oneline'

alias gb='git checkout -b'

alias gd='git diff'

alias ..='cd ..;pwd'

alias ...='cd ../..;pwd'

alias ....='cd ../../..;pwd'

alias c='clear'

alias h='history'

alias tree='tree --dirsfirst -F'

alias mkdir='mkdir -p -v'

alias jan='cal -m 01'
alias feb='cal -m 02'
alias mar='cal -m 03'
alias apr='cal -m 04'
alias may='cal -m 05'
alias jun='cal -m 06'
alias jul='cal -m 07'
alias aug='cal -m 08'
alias sep='cal -m 09'
alias oct='cal -m 10'
alias nov='cal -m 11'
alias dec='cal -m 12'

function hg() {
    history | grep "$1";
}

function find_largest_files() {
    du -h -x -s -- * | sort -r -h | head -20;
}

function git_branch() {
    if [ -d .git ] ; then
        printf "%s" "($(git branch 2> /dev/null | awk '/\*/{print $2}'))";
    fi
}

# Set the prompt.
function bash_prompt(){
    PS1='${debian_chroot:+($debian_chroot)}'${blu}'$(git_branch)'${pur}' \W'${grn}' \$ '${clr}
}

bash_prompt

function git_init() {
    if [ -z "$1" ]; then
        printf "%s\n" "Please provide a directory name.";
    else
        mkdir "$1";
        builtin cd "$1";
        pwd;
        git init;
        touch readme.md .gitignore LICENSE;
        echo "# $(basename $PWD)" >> readme.md
    fi
}

function weather_report() {

    local response=$(curl --silent 'https://api.openweathermap.org/data/2.5/weather?id=5128581&units=imperial&appid=<YOUR_API_KEY>') 

    local status=$(echo $response | jq -r '.cod')

    case $status in

        200) printf "Location: %s %s\n" "$(echo $response | jq '.name') $(echo $response | jq '.sys.country')"  
             printf "Forecast: %s\n" "$(echo $response | jq '.weather[].description')" 
             printf "Temperature: %.1f°F\n" "$(echo $response | jq '.main.temp')" 
             printf "Temp Min: %.1f°F\n" "$(echo $response | jq '.main.temp_min')" 
             printf "Temp Max: %.1f°F\n" "$(echo $response | jq '.main.temp_max')" 
            ;;
        401) echo "401 error"
            ;;
        *) echo "error"
            ;;

    esac

}

clear

printf "\n"
printf "   %s\n" "IP ADDR: $(curl ifconfig.me)"
printf "   %s\n" "USER: $(echo $USER)"
printf "   %s\n" "DATE: $(date)"
printf "   %s\n" "UPTIME: $(uptime -p)"
printf "   %s\n" "HOSTNAME: $(hostname -f)"
printf "   %s\n" "CPU: $(awk -F: '/model name/{print $2}' | head -1)"
printf "   %s\n" "KERNEL: $(uname -rms)"
printf "   %s\n" "PACKAGES: $(dpkg --get-selections | wc -l)"
printf "   %s\n" "RESOLUTION: $(xrandr | awk '/\*/{printf $1" "}')"
printf "   %s\n" "MEMORY: $(free -m -h | awk '/Mem/{print $3"/"$2}')"
printf "\n" 
```

## 结论

在本文中，您了解了如何配置各种。bashrc 选项、别名、功能等等，大大改善您的工作流程，提高您的工作效率。

跟着我上 [Github](https://github.com/brandon-wallace) | [Dev 到](https://dev.to/brandonwallace)。