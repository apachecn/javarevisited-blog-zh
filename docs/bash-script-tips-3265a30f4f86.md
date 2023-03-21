# Bash 脚本技巧

> 原文：<https://medium.com/javarevisited/bash-script-tips-3265a30f4f86?source=collection_archive---------0----------------------->

我不是 Linux 系统管理员的专家，但是我每天都在工作和使用基于 Linux/Unix 的系统，我发现自己熟悉大多数 shell 脚本命令行，并且非常有效地使用它们。

今天，我想分享我使用这些命令的经验和最佳实践。

![](img/4ddfc30cb5eb2ab0ec12be7bb8fe8783.png)

# BASH 基础

```
env                 # displays all environment variablesecho $SHELL         # displays the shell you're using
echo $BASH_VERSION  # displays bash version whereis bash        # locates the binary, source and manual-page for 
                      a command
which bash          # finds out which program is executed as 'bash' 
                      (default: /bin/bash, can change across 
                       environments)clear               # clears content on window 
```

# 文件/目录

```
ls  ## listing current files/directories
ls -options ## list with opertions cd  ## change directory
mkdir ## make directory
pwd   ## output current location path
cp src dest  ## copy files/directories from src to dest
mv src dest  ##  move files/directories from src to dest
rm  files/dir ## remove files directories
touch file  ## create empty file
```

# **搜索**操作

```
find ## find files and directories in your system. example
**find ./directoryname -name file.txt
find ./directoryname -name *.txt**# find all folders/files but excluding any tar files and excluding #the sub folder test and all files in test folder
**find /dbdata/backup  ! -path ".tar"  ! -path "/test" ! -path "/test/"**grep  ## search for content text in files/directories#grep -i to make case insensitive searches
**$grep -i "grape" file.txt**#grep to count the number of times a word appears in a file **$grep -c "text to search" file.txt**
```

# **快捷命令**

**别名**是一个 Linux 命令，用于 bash 命令的快捷方式/别名。这是一种快速的方法，可以将长命令行映射成短的，容易记忆的命令行。语法是

```
alias alias_name="command_to_run"
ex : 
alias gitc="git commmit"
alias top="htop"
```

# **生产率**

为了使别名命令永久地与您的帐户配置文件一起工作。您应该在~/中添加 alias 命令，如下所示。bashrc:

用下面的内容创建一个文件~/bash_aliases:

```
*## Colorize the ls output ##*
**alias** ls='ls --color=auto'

*## Use a long listing format ##*
**alias** ll='ls -la'

*## Show hidden files ##*
**alias** l.='ls -d .* --color=auto' alias lt='ls --human-readable --size -1 -S --classify'

alias gh='history|grep'

##count files
alias count='find . -type f | wc -l'

##top with htop instead

alias top='htop'
```

将下面几行添加到 fille 中。/bashrc

```
if [ -e $HOME/.bash_aliases ]; then
    source $HOME/.bash_aliases
fi
```

# 生产环境的别名

有些别名命令非常安全，尤其是在生产环境中。

**dd 命令**

```
##dd command : is a command to copy data from one file ore device to anotherexample : dd **if**=/dev/zero **of**=/dev/sda
- **if** option is the source and /dev/zero is an unlimited source of zero bytes
- **of** is the target and /dev/sda is a disk drive or volumeYou see what is is going with above. It’s an excellent way to erase the contents of a whole disk and a pretty quick way to lose a lot of data. This is how hacker can put a bomb in your server just one command line.#How to prevent it?You can disable the whole dd command with the following alias:**alias dd='echo "no dd command available"'**If the dd command isexecuted on your system, it only prints out *no dd command available*.
```

**chmod | chown 命令**

这些命令经常在 LINUX 系统中使用。但是，您可能会很容易在以下情况下犯下打字错误:

```
$ cd /opt/project1  # you want to set the permission to All 777 to all files and folders in this directory. you type the command below**chmod -R 777 /** instead of  **chmod -R 777 ./**what happens with command above?chmod -R applies file permissions recursively 777 permission mode to set (permit everything). ./ the directory or file which should be changedIf you don’t pay attention which directory you want to target and accidentally take the root directory (/) instead of the current directory (./) , you will screw up all permissions of your whole system with root folder, and you can not rollback it back with right permissions.How to prevent this?You can avoid the typo above, you can add the following alias:**alias chmod='chmod --preserve-root'**
**alias chown='chown --preserve-root'**This will always decline a recursive change on the root directory.
```

更多的命令别名使你的工作更有效率

```
*## Colorize the ls output ##*
**alias** ls='ls --color=auto*# do not delete / or prompt if deleting more than 3 files at a time* 
**alias** rm='rm -i --preserve-root'

*# confirmation #*
**alias** mv='mv -i'
**alias** cp='cp -i'
**alias** ln='ln -i'

*# safety Parenting changing perms on / #*
**alias** chown='chown --preserve-root'
**alias** chmod='chmod --preserve-root'
**alias** chgrp='chgrp --preserve-root'alias top="htop"alias mkdir="mkdir -pv"alias wget="wget -c"alias myip="curl http://ipecho.net/plain; echo"alias free="free -mt"
alias du="du -ach | sort -h"
alias df="df -Tha --total"
```

# 系统内存，CPU

```
*## pass options to free* 
**alias** meminfo='free -m -l -t'

*## get top process consuming memory*
**alias** psmem='ps auxf | sort -nr -k 4'
**alias** psmem10='ps auxf | sort -nr -k 4 | head -10'

*## get top process consuming cpu ##*
**alias** pscpu='ps auxf | sort -nr -k 3'
**alias** pscpu10='ps auxf | sort -nr -k 3 | head -10'

*## Get server cpu info ##*
**alias** cpuinfo='lscpu'

*## older system use /proc/cpuinfo ##*
*##alias cpuinfo='less /proc/cpuinfo' ##*

*## get GPU ram on desktop / laptop##*
**alias** gpumeminfo='grep -i --color memory /var/log/Xorg.0.log'alias dd='echo "no dd command available"'
```

# **字符串操作**

```
#Parameter expansionsSTR="/tmp/test/foo.txt"
echo ${STR%.txt}    # /tmp/test/foo
echo ${STR%.txt}.o  # /tmp/test/foo.oecho ${STR##*.}     # txt(extension)
echo ${STR##*/}     # foo.txt (basepath)echo ${STR#*/}      # tmp/test/foo.txt
echo ${STR##*/}     # foo.txtecho ${STR/foo/bar} # /tmp/test/bar.txt#Lenght
echo "**${#****STR****}**" #Length of $STR#Manipulation
STR="HELLO WORLD!"
echo ${STR,}   #=> "hELLO WORLD!" (lowercase 1st letter)
echo ${STR,,}  #=> "hello world!" (all lowercase)

STR="hello world!"
echo ${STR^}   #=> "Hello world!" (uppercase 1st letter)
echo ${STR^^}  #=> "HELLO WORLD!" (all uppercase)#Default values
${FOO:-val} #$FOO, or val if not set
${FOO:=val} #Set $FOO to val if not set
${FOO:+val} #val if $FOO is set
${FOO:?message} #Show error message and exit if $FOO is not setThe : is optional (eg, ${FOO=word} works)
```

# 重寄

```
echo "test message" > output.txt   # stdout to (file)
cat file1.txt >> output.txt  # stdout to (file), append
python hello.py 2> error.log   # stderr to (file)
python hello.py 2>&1           # stderr to stdout
python hello.py 2>/dev/null    # stderr to (null)
python hello.py &>/dev/null    # stdout and stderr to (null)python hello.py < foo.txt      # feed foo.txt to stdin for python
```

# 历史

```
history  # show all histories
```

# **功能**

```
test.sh#!/bin/bash

**function1**() {
  echo "Hello $1"
}# Same as above (alternate syntax)
function **function1**() {
    echo "hello $1"
}**function1** world
**function1** "Phnom Penh"
```

# 返回值

```
myfunc() {
    local myresult='some value'
    echo $myresult
}result="$(myfunc)"
```

# **论据**

```
$0    #reserved for the function name$* or $@  # holds all the arguments passed to the function$#    #total number of parameters passed to the function$? # return value
```

# SSH、系统信息和网络命令

```
ssh user@host            # connects to host as user
ssh -p <port> user@host  # connects to host on specified port as 
                           user
ssh-copy-id user@host    # adds your ssh key to host for user to 
                            enable a keyed or passwordless login##copy file
scp user@host:file.txt /local/file # copy server file to local file
scp file user@host:/path/file     # copy local file to remote server##copy folders from server to local folder
scp -r user@host:folder local/folder 
##copy local folders from local to remote server folder
scp -r local/folder user@host:folderwhoami                   # returns your username
passwd                   # lets you change your password
quota -v                 # shows what your disk quota is
date                     # shows the current date and time
uptime                   # shows current uptime
w                        # displays whois online
finger <user>            # displays information about user
uname -a                 # shows kernel information

df                       # shows disk usage
du <filename>            # shows the disk usage of the files and  
                           directories in filename (du -s give only 
                           a total)last <yourUsername>      # lists your last logins
ps -u yourusername       # lists your processes
kill <PID>               # kills the processes with the ID you gave
killall <processname>    # kill all processes with the name
top  / htop              # displays your currently active processes ping <host>              # pings host and outputs results
whois <domain>           # gets whois information for domain
dig <domain>             # gets DNS information for domain
dig -x <host>            # reverses lookup host
wget <file>              # downloads file
curl <domain_url>        # download  and reading content from url
```

将 sudo 命令授予具有特定操作和文件夹的特定用户

```
#grant user1 with sudo chmod 700 /dbdata/backup 
$cd /etc/sudoers.d
$visudo
user1 ALL=NOPASSWD:/bin/chmod [!s] /dbdata/backup/
```

# Cron 任务

```
**#Format cron task syntax
Min  Hour Day  Mon  Weekday
*    *    *    *    *  command to be execute
┬    ┬    ┬    ┬    ┬
│    │    │    │    └─  Weekday  (0=Sun .. 6=Sat)
│    │    │    └──────  Month    (1..12)
│    │    └───────────  Day      (1..31)
│    └────────────────  Hour     (0..23)
└─────────────────────  Minute   (0..59)**###Example####
0 * * * *        every hour
*/15 * * * *     every 15 mins
0 */2 * * *      every 2 hours
0 0 * * 0        every Sunday midnight **# Adding tasks easily crontab** echo "this is my cron" | crontab**# Open in editor
crontab -e****# List cron tasks
crontab -l [-u user]**
```

# Heredoc

```
cat <<END
**************************************
Put your instruction comment text
**************************************
END
```

# 其他高级命令

还有其他命令，例如

```
cut
awk
sed
tee
xargs
kill
ps
tail
less
more
cat
...command list users who can sudo or super users in our Linux system with command:**$** [**grep**](https://ostechnix.com/the-grep-command-tutorial-with-examples-for-beginners/) **'^sudo:.*$' /etc/group | cut -d: -f4**
sophea,ubuntuor this command
**$ getent group sudo | cut -d: -f4**
```

# 查找用户是否有 sudo 权限

我们现在知道如何在我们的 Linux 系统中找到所有的 sudo 用户。如何发现某个用户是否有 sudo 权限？那很简单！

要确定一个用户是否是 sudo 用户，只需运行

```
$ sudo -l -U sophea
```

# 向 Linux 上的普通用户授予 Sudo 特权

```
sudo usermod -a -G sudo sophea  
or this commandsudo adduser sophea sudo
```

## 删除用户的 sudo 权限

```
sudo deluser sopheasudo
```

**样本输出:**

```
Removing user `sophea' from group `sudo' ...
Done.
```

我希望这篇文章能够帮助您，扩大您对 bash shell 脚本的了解。

如果这篇文章是有帮助的，请支持按下**按钮**，帮助其他读者也看到这个故事。

我期待着反馈和评论

<https://javarevisited.blogspot.com/2018/02/5-courses-to-learn-shell-scripting-in-linux.html>  </javarevisited/top-10-courses-to-learn-linux-command-line-in-2020-best-and-free-f3ee4a78d0c0> 