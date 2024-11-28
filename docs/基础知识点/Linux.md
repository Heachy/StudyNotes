# Linux系统

# 目录解释

- **/bin**:  bin是Binary的缩写, 这个目录存放着最经常使用的命令。

- **/boot**:  这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。(不要动)

- **/dev**:  dev是Device(设备)的缩写, 存放的是Linux的外部设备, 在Linux中访问设备的方式和访问文件的方式是相同的。

- **/etc**:  这个目录用来存放所有的系统管理所需要的配置文件和子目录。

- **/home**:  用户的主目录, 在Linux中, 每个用户都有一个自己的目录, 一般该目录名是以用户的账号命名的。

- **/lib**:  这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。(不要动)

- **/lost+found**:  这个目录一般情况下是空的, 当系统非法关机后, 这里就存放了一些文件。(存放突然关机的一些文件)

- **/media**:  linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

- **/mnt**:  系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。(我们后面会把一些本地文件挂载在这个目录下)

- **/opt**:  这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

- **/proc**:  这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。(不用管)

- **/root**:  该目录为系统管理员，也称作超级权限者的用户主目录。

- **/sbin**:  s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。

- **/srv**:  该目录存放一些服务启动之后需要提取的数据。

- **/sys**:  这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统sysfs。

- **/tmp**:  这个目录是用来存放一些临时文件的。用完即丢的文件，可以放在这个目录下，安装包!

- **/usr**:  这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。

- **/usr/bin**:  系统用户使用的应用程序。

- **/usr/sbin**:  超级用户使用的比较高级的管理程序和系统守护程序。

- **/usr/src**:  内核源代码默认的放置目录。

- **/var**:  这个目录中存放着在不断扩充着的东西, 我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

- **/run**:  是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。


## 查看命令使用文档

你可以使用 `man [命令]` 来查看各个命令的使用文档，例如：`man cp`。

## 网络配置目录

进入网络配置目录的命令：
```shell
cd /etc/sysconfig/network-scripts
```

### 网络脚本文件列表

```shell
[root@kuangshen network-scripts]# ls
ifcfg-eth0   ifdown-ppp   ifup-eth   ifup-sit
ifcfg-lo     ifdown-routes ifup-ippp  ifup-Team
ifdown       ifdown-sit    ifup-ipv6  ifup-TeamPort
ifdown-bnep  ifdown-Team   ifup-isdn  ifup-tunnel
ifdown-eth   ifdown-TeamPort  ifup-plip  ifup-wireless
ifdown-ippp  ifdown-tunnel ifup-plusb init.ipv6-global
ifdown-ipv6  ifup          ifup-post   network-functions
ifdown-isdn  ifup-aliases  ifup-ppp    network-functions-ipv6
ifdown-post  ifup-bnep     ifup-routes 
```

### 查看网络配置

使用 `ifconfig` 命令查看网络配置。

```shell
ifconfig
```

# 目录操作指令

## 文件列表命令 `ls`

在Linux中 `ls` 可能是最常被使用的命令！

- `-a` 参数：all，查看全部的文件，包括隐藏文件
- `-l` 参数：列出所有的文件，包含文件的属性和权限，没有隐藏文件

## `cd` 切换目录命令

- `./` : 当前目录
- `cd ..` : 返回上一级目录
- `cd /` : 切换到根目录
- `cd ~` : 返回到当前用户的主目录

### 示例操作

```shell
[root@kuangshen ~]# cd /
[root@kuangshen /]# ls
bin  dev  home  lib64  boot  etc  lib  lost+found  mnt  media  opt  proc  root  sbin  srv  sys  tmp  usr  var

[root@kuangshen /]# cd home
[root@kuangshen home]# mkdir kuangstudy
[root@kuangshen home]# ls
install.sh  kuangshen  kuangstudy  www

[root@kuangshen home]# cd ..
[root@kuangshen /]# cd /home/kuangshen
[root@kuangshen kuangshen]# cd ~
[root@kuangshen ~]# pwd
/root
```

## `pwd` 显示当前用户所在的目录

```shell
[root@kuangshen ~]# pwd
/root

[root@kuangshen ~]# cd /bin
[root@kuangshen bin]# pwd
/bin

[root@kuangshen bin]# cd /usr/local
[root@kuangshen local]# pwd
/usr/local
```

## `mkdir` 创建一个目录

```shell
[root@kuangshen home]# mkdir test1
[root@kuangshen home]# ls
install.sh  kuangshen  kuangstudy  test1  www

[root@kuangshen home]# cd test1
[root@kuangshen test1]# cd ..
[root@kuangshen home]# mkdir test2/test3/test4
mkdir: cannot create directory ‘test2/test3/test4’: No such file or directory

[root@kuangshen home]# mkdir -p test2/test3/test4
[root@kuangshen home]# ls
install.sh  kuangshen  kuangstudy  test1  test2  www
```

## `rmdir` 删除目录

`rmdir` 仅能删除空的目录，如果下面存在文件，需要先删除文件，递归删除多个目录 `-p` 参数即可。

## `cp` (复制文件或者目录)

```shell
cp 原来的地方 新的地方
```

### 示例操作

```shell
[root@kuangshen home]# cp install.sh kuangstudy
[root@kuangshen home]# ls
install.sh  kuangshen  kuangstudy  www

[root@kuangshen home]# cd kuangstudy/
[root@kuangshen kuangstudy]# ls
install.sh

[root@kuangshen kuangstudy]# cd ..
[root@kuangshen home]# cp install.sh kuangstudy
cp: overwrite 'kuangstudy/install.sh'? y
```

如果文件重复，选择覆盖或者放弃。

## `rm` 移除文件或者目录

- `-f` 忽略不存在的文件，不会出现警告，强制删除！
- `-r` 递归删除目录！
- `-i` 互动，删除前询问是否删除

**警告**: `rm -rf /` 系统中所有的文件就被删除了，删库跑路就是这么操作的！

### 示例操作

```shell
[root@kuangshen kuangstudy]# ls
install.sh

[root@kuangshen kuangstudy]# rm -rf install.sh
[root@kuangshen kuangstudy]# ls
```

## `mv` 移动文件或者目录！重命名文件

- `-f` 强制
- `-u` 只替换已经更新过的文件

### 示例操作

```shell
[root@kuangshen home]# ls
install.sh  kuangshen  kuangstudy  www

[root@kuangshen home]# mv install.sh kuangstudy/  # 移动文件
[root@kuangshen home]# ls
kuangshen  kuangstudy  www

[root@kuangshen home]# cd kuangstudy/
[root@kuangshen kuangstudy]# ls
install.sh

[root@kuangshen kuangstudy]# cd ..
[root@kuangshen home]# ls
kuangshen  kuangstudy  www

[root@kuangshen home]# mv kuangstudy kuangstudy2  # 重命名文件夹
[root@kuangshen home]# ls
kuangshen  kuangstudy2  www
```

```text
请注意，`rm -rf /` 是一个极其危险的命令，它会删除系统根目录下的所有文件，这将导致系统无法正常运行。在实际使用中，应该避免使用这样的命令，除非完全了解其后果。
```



# 文件属性

在Linux中，文件属性由10个字符表示，如下所示：

- 第一个字符表示文件类型：
  - `d` 表示目录
  - `-` 表示普通文件
  - `l` 表示链接文件
  - `b` 表示块设备文件
  - `c` 表示字符设备文件

- 接下来的9个字符分为三组，每组三个字符，分别表示：
  - 属主权限
  - 属组权限
  - 其他用户权限

权限字符包括：
  - `r` 代表可读（read）
  - `w` 代表可写（write）
  - `x` 代表可执行（execute）

如果没有权限，将显示为 `-`。

## 文件权限示例

| 文件类型 | 属主权限 | 属组权限 | 其他用户权限 |
| -------- | -------- | -------- | ------------ |
| d        | rwx      | r-x      | r-x          |

## 修改文件属性

### 1. `chgrp`: 更改文件属组

```shell
chgrp [-R] 属组名 文件名
```

### 2. `chown`: 更改文件属主，也可以同时更改文件属组

shell

```shell
chown [-R] 属主名 文件名
chown [-R] 属主名:属组名 文件名
```

### 3. `chmod`: 更改文件9个属性

shell

```shell
chmod [-R] xyz 文件或目录
```

Linux文件属性有两种设置方法，一种是数字，一种是符号。

### 数字表示法

权限的数字表示法对照表：

| 权限 | 数字 |
| :--- | :--- |
| r    | 4    |
| w    | 2    |
| x    | 1    |

- 可读可写不可执行: `rw-` (6)
- 可读可写可执行: `rwx` (7)

### 符号表示法

使用符号来设置权限，例如：

- `chmod 777 文件名`：文件赋予所有用户可读可执行权限。

# 文件内容查看命令

## `cat`
- 由第一行开始显示文件内容，用来读文章，或者读取配置文件。

## `tac`
- 从最后一行开始显示，可以看出 `tac` 是 `cat` 的倒着写。

### 示例操作

```shell
[root@kuangshen network-scripts]# cat ifcfg-eth0
DEVICE=eth0
BOOTPROTO=dhcp
ONBOOT=yes

[root@kuangshen network-scripts]# tac ifcfg-eth0
ONBOOT=yes
BOOTPROTO=dhcp
DEVICE=eth0
```

## `nl`

- 显示的时候，顺道输出行号，看代码的时候，希望显示行号。

```shell
[root@kuangshen network-scripts]# nl ifcfg-eth0
1  DEVICE=eth0
2  
3  BOOTPROTO=dhcp
4  
5  ONBOOT=yes
```

## `more`

- 一页一页的显示文件内容，带余下内容的（空格代表翻页，Enter代表向下看一行，:f行号）。

## `less`

- 与 `more` 类似，但是比 `more` 更好的是，它可以往前翻页（空格下翻页，PageDown，PageUp键代表翻动页面，退出q命令，查找字符串/要查询的字符向下查询，向上查询使用?要查询的字符串，n继续搜寻下一个，N上寻找）。

## `head`

- 只看头几行，通过 `-n` 参数来控制显示几行。

```shell
[root@kuangshen etc]# head -n 20 csh.login
```

## `tail`

- 只看尾巴几行，`-n` 参数要查看几行。

```shell
[root@kuangshen etc]# tail -n 20 csh.login
```

在这个Markdown文档中，我使用了代码块（```shell）来表示终端命令和输出，以保持格式的清晰和易读。同时，我也将图片中的说明文字转换为Markdown格式，以便于阅读和理解。

# Linux 链接的概念

> Linux的链接分为两种：硬链接、软链接。
>

## 硬链接
- A---B，假设B是A的硬链接，那么他们两个指向了同一个文件。
- 允许一个文件拥有多个路径，用户可以通过这种机制建立硬链接到一些重要文件上，防止误删。

## 软链接
- 类似Windows下的快捷方式，删除的源文件，快捷方式也访问不了。

## 创建链接命令
- `ln` 命令用来创建链接。
- `touch` 命令用来创建文件。
- `echo` 命令用来输入字符串。

## 示例操作

```shell
[root@kuangshen ~]# cd /home
[root@kuangshen home]# touch f1
[root@kuangshen home]# ln f1 f2  # 创建一个硬链接 f2
[root@kuangshen home]# ln -s f1 f3  # 创建一个软链接（符号链接）f3
[root@kuangshen home]# echo "i love kuangshen" >>f1
[root@kuangshen home]# cat f1  # 查看f1
i love kuangshen
[root@kuangshen home]# cat f2  # 查看f2
i love kuangshen
[root@kuangshen home]# cat f3  # 查看f3
i love kuangshen

# 删除f1之后，查看f2和f3的区别
[root@kuangshen home]# rm -rf f1
[root@kuangshen home]# cat f2  # f2 硬链接还在
i love kuangshen
[root@kuangshen home]# cat f3  # f3（软链接）快捷方式失效
cat: f3: No such file or directory
```

**注意：**`ln` 命令用于创建链接，`-s` 参数表示创建软链接。`rm -rf` 命令用于强制删除文件或目录，使用时需要小心，因为它不会询问确认。在上述示例中，删除了原始文件 `f1` 后，硬链接 `f2` 仍然可以访问，而软链接 `f3` 则失效了。

# 编辑文件内容

## 输入模式按键说明

在输入模式中，可以使用以下按键：

- 字符按键以及Shift组合，输入字符
- `ENTER`，回车键，换行
- `BACK SPACE`，退格键，删除光标前一个字符
- `DEL`，删除键，删除光标后一个字符
- 方向键，在文本中移动光标
- `HOME/END`，移动光标到行首/行尾
- `Page Up/Page Down`，上/下翻页
- `Insert`，切换光标为输入/替换模式，光标将变成竖线/下划线
- `ESC`，退出输入模式，切换到命令模式

## 底线命令模式

在命令模式下按下`:`（英文冒号）就进入了底线命令模式。

### 常用命令：

- `i` 切换到输入模式，以输入字符。
- `x` 删除当前光标所在处的字符。
- `:` 切换到底线命令模式，以在最底一行输入命令。

## 命令模式

用户刚刚启动 vi/vim，便进入了命令模式。

此状态下敲击键盘动作会被Vim识别为命令，而非输入字符。以下是常用的几个命令：

- `i` 切换到输入模式，以输入字符。
- `x` 删除当前光标所在处的字符。
- `:` 切换到底线命令模式，以在最底一行输入命令。

## 一般模式切换到编辑模式的可用的按钮说明

### 进入输入或取代的编辑模式

- `i` 进入输入模式(Insert mode)：从目前光标所在处输入。
- `I` 在目前所在行的第一个非空格符处开始输入。
- `a` 进入输入模式(Insert mode)：从目前光标所在的下一个字符处开始输入。
- `A` 从光标所在行的最后一个字符处开始输入。
- `o` 进入输入模式(Insert mode)：在目前光标所在的下一行处输入新的一行。
- `O` 在目前光标所在处的上一行输入新的一行。
- `r` 进入取代模式(Replace mode)：只会取代光标所在的那一个字符一次。
- `R` 会一直取代光标所在的文字，直到按下 `ESC` 为止。
- `Esc` 退出编辑模式，回到一般模式中。

### 指令行的储存、离开等指令

- `:w` 将编辑的数据写入硬盘档案中（常用）
- `:w!` 若文件属性为【只读】时，强制写入该档案。
- `:q` 离开 vi（常用）
- `:q!` 若曾修改过档案，又不想储存，使用 `!` 为强制离开不储存档案。
- `:wq` 储存后离开，若为 `:wq!` 则为强制储存后离开（常用）

## 移动光标

- `h` 或向左箭头键：光标向左移动一个字符
- `j` 或向下箭头键：光标向下移动一个字符
- `k` 或向上箭头键：光标向上移动一个字符
- `l` 或向右箭头键：光标向右移动一个字符
- `Ctrl+f` 屏幕向下移动一页
- `Ctrl+b` 屏幕向上移动一页
- `Ctrl+d` 屏幕向下移动半页
- `Ctrl+u` 屏幕向上移动半页
- `+` 光标移动到非空格符的下一行
- `-` 光标移动到非空格符的上一行
- `n space` 按下数字后再按空格键，光标会向右移动这一行的n个字符。
- `0` 或 `Home` 移动到这一行的最前面字符处
- `$` 或 `End` 移动到这一行的最后面字符处
- `H` 光标移动到这个屏幕的最上方那一行的第一个字符
- `M` 光标移动到这个屏幕的中央那一行的第一个字符
- `L` 光标移动到这个屏幕的最下方那一行的第一个字符
- `G` 移动到这个档案的最后一行
- `nG` 移动到这个档案的第n行
- `gg` 移动到这个档案的第一行

## 搜索与替换

- `/word` 向光标之下寻找一个名称为word的字符串。
- `?word` 向光标之上寻找一个名称为word的字符串。
- `n` 重复前一个搜寻的动作。
- `N` 与 `n` 刚好相反，为反向进行前一个搜寻动作。

## 复制与粘贴

- `x, X` 删除字符，`x` 向后删除，`X` 向前删除。
- `nx` 连续向后删除n个字符。
- `dd` 删除游标所在的那一整行。
- `ndd` 删除光标所在的向下n行。
- `d1G` 删除光标所在到第一行的所有数据。
- `dG` 删除光标所在到最后一行的所有数据。
- `d$` 删除游标所在处到该行的最后一个字符。
- `d0` 删除游标所在处到该行的最前面一个字符。
- `yy` 复制游标所在的那一行。
- `nyy` 复制光标所在的向下n行。
- `y1G` 复制光标所在行到第一行的所有数据。
- `yG` 复制光标所在行到最后一行的所有数据。
- `y0` 复制光标所在的那个字符到该行行首的所有数据。
- `y$` 复制光标所在的那个字符到该行行尾的所有数据。
- `p, P` 粘贴复制的数据，`p` 在光标下一行，`P` 在光标上一行。
- `J` 将光标所在行与下一行的数据结合成同一行。
- `c` 重复删除多个数据。
- `u` 复原前一个动作。
- `Ctrl+r` 重做上一个动作。

## 一般模式切换到编辑模式的可用的按钮说明

### 进入输入或取代的编辑模式

- `i, I`：进入输入模式(Insert mode)，`i` 为从目前光标所在处输入，`I` 为在目前所在行的第一个非空格符处开始输入。（常用）
- `a, A`：进入输入模式(Insert mode)，`a` 为从目前光标所在的下一个字符处开始输入，`A` 为从光标所在行的最后一个字符处开始输入。（常用）
- `o, O`：进入输入模式(Insert mode)，`o` 为在目前光标所在的下一行处输入新的一行；`O` 为在目前光标所在处的上一行输入新的一行。（常用）
- `r, R`：进入取代模式(Replace mode)，`r` 只会取代光标所在的那一个字符一次；`R` 会一直取代光标所在的文字，直到按下 `ESC` 为止。（常用）
- `[Esc]`：退出编辑模式，回到一般模式中。（常用）

### 一般模式切换到指令行模式的可用的按钮说明

#### 指令行的储存、离开等指令

- `:w`：将编辑的数据写入硬盘档案中。（常用）
- `:w!`：若文件属性为【只读】时，强制写入该档案。
- `:q`：离开 vi。（常用）
- `:q!`：若曾修改过档案，又不想储存，使用 `!` 为强制离开不储存档案。
- `:wq`：储存后离开，若为 `:wq!` 则为强制储存后离开。（常用）

# 用户管理

## 添加用户

使用 `useradd` 命令添加用户：

```shell
useradd -m qinjiang  # 创建一个用户，并自动创建主目录 /home/qinjiang
```

## 查看用户

查看当前用户列表：

```shell
[root@kuangshen home]# ls
install.sh  kuangshen  qinjiang  www
```

## 删除用户

使用 `userdel` 命令删除用户：

```shell
userdel -r qinjiang  # 删除用户 qinjiang 并删除其主目录
```

## 修改用户

使用 `usermod` 命令修改用户信息：

```shell
usermod -d /home/233 qinjiang  # 修改用户 qinjiang 的主目录为 /home/233
```

## 切换用户

1. 使用 `su` 命令切换用户：

```shell
su qinjiang  # 切换到用户 qinjiang
```

1. 从普通用户切换到 root 用户：

```shell
sudo su  # 使用 sudo 命令切换到 root 用户
```

## 主机名管理

### 查看主机名

```shell
[root@kuangstudy ~]# hostname  # 查看主机名
kuangstudy
```

### 修改主机名

```shell
hostname kuangshen  # 修改主机名为 kuangshen
```

## 用户密码设置

### 超级用户密码设置

```shell
passwd root  # 设置或修改 root 用户的密码
```

### 普通用户密码设置

```shell
passwd qinjiang  # 设置或修改 qinjiang 用户的密码
```

## 锁定账户

```shell
passwd -l qinjiang  # 锁定用户 qinjiang 的账户
```

## 取消锁定账户

```shell
passwd -d qinjiang  # 解除用户 qinjiang 的账户锁定
```

## SSH 连接

> 在 SSH 连接时，如果服务器拒绝了密码，可能需要重新尝试：

```plaintext
SSH服务器拒绝了密码，请再试一次。
```

# 用户组管理

### 属主、属组

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理（开发、测试、运维、root）。不同Linux系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对 `/etc/group` 文件的更新。

### 创建用户组

使用 `groupadd` 命令创建用户组：

```shell
[root@kuangshen ~]# groupadd kuangshen
[root@kuangshen ~]# cat /etc/group
```



# 磁盘使用量查看

## 使用 `du` 命令查看目录占用空间

```shell
[root@kuangshen home]# du -a
4       ./qinjiang/.bash_history
4       ./qinjiang/.bashrc
4       ./qinjiang/.bash_profile
4       ./qinjiang/.bash_logout
20      ./qinjiang
4       ./www/.bashrc
4       ./www/.bash_profile
4       ./www/.bash_logout
16      ./www
10676  ./kuangshen/apache-tomcat-9.0.22.tar.gz
175304 ./kuangshen/jdk-8u221-linux-x64.rpm
185984 ./
```

## 使用 `du` 命令查看根目录下每个目录所占用的容量

```shell
[root@kuangshen /]# du -sm /*
0       /bin
146     /boot
1       /dev
34      /etc
182     /home
1       /lib
0       /lib64
0       /lost+found
1       /media
1       /mnt
1       /opt
0       /patch
du: cannot access `/proc/10923/task/10923/fd/4': No such file or directory
du: cannot access `/proc/10923/task/1093/fdinfo/4': No such file or directory
du: cannot access `/proc/10923/fd/4': No such file or directory
```

## 挂载设备

```shell
[root@kuangshen /]# mount  # 查看挂载情况
bin   dev  home  lib64  media  opt  proc  run  share  sys  usr  www
boot  etc  lib   lost+found  mnt  patch  root  sbin  srv  tmp  var

[root@kuangshen /]# mount /dev/kuangshen /mnt/kuangshen  # 挂载设备kuangshen
```

## 卸载

```shell
umount -f  # 强制卸载 [挂载位置]
```

# 进程

## 基本概念

1. 在Linux中，每一个程序都是有自己的一个进程，每一个进程都有一个ID号！
2. 每一个进程，都会有一个父进程！
3. 进程可以有两种存在方式：前台！后台运行！
4. 一般的话服务都是后台运行的，基本的程序都是前台运行的！

## 命令

### `ps` 查看当前系统中正在执行的各种进程的信息

- `ps -XX`：
  - `-a` 显示当前终端运行的所有的进程信息
  - `-u` 以用户的信息显示进程
  - `-x` 显示后台运行进程的参数

#### 查看所有进程

```shell
ps -aux | grep mysql
ps -aux | grep redis
ps -aux | grep java
```

#### 管道符 `|`

在Linux中，管道符 `|` 用于连接两个命令，将前一个命令的输出作为后一个命令的输入。

#### `grep` 查找文件中符合条件的字符串

对于我们来说，这里目前只需要记住一个命令即可 `ps -xx | grep 进程名字` 过滤进程信息！

### 查看父进程

```shell
ps -ef | grep mysql  # 看父进程，我们一般可以通过目录树结构来查看！
```

### `pstree` 进程树

```shell
pstree -pu
```

- `-p` 显示父ID
- `-u` 显示用户组

### 结束进程

结束进程：杀掉进程，等价于Windows结束任务！

```shell
kill -9 进程的ID  # 强制结束进程
```

如果遇到Java代码死循环的情况，可以选择结束进程。

```shell
kill -9 进程的ID  # 强制结束该进程
```

# JDK

## JDK安装

我们开发Java程序必须要的环境！

1. 下载JDK rpm。去Oracle官网下载即可！

2. 安装Java环境

```shell
# 检测当前系统是否存在Java环境！
java -version

# 如果有的话就需要卸载
rpm -qa | grep jdk  # 检测JDK版本信息
rpm -e --nodeps jdk_  # 卸载JDK

# 卸载完毕后即可安装jdk
rpm -ivk rpm包

# 配置环境变量！
```

如果存在可以提前卸载：

```shell
[root@kuangshen kuangshen]# java -version
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)

[root@kuangshen kuangshen]# rpm -qa | grep jdk
jdk1.8-1.8.0_221-fcs.x86_64

[root@kuangshen kuangshen]# rpm -e --nodeps jdk1.8-1.8.0_221-fcs.x86_64

[root@kuangshen kuangshen]# java -version
-bash: /usr/bin/java: No such file or directory
```

### 发布项目测试

#### 开启防火墙端口

```shell
firewall-cmd --zone=public --add-port=9000/tcp --permanent
```

#### 重启防火墙

```shell
systemctl restart firewalld.service
```

#### 查看所有开启的端口

```shell
firewall-cmd --list-ports
```

如果是阿里云，需要配置安全组规则！

# Tomcat

## Tomcat 安装

Tomcat 需要放到tomcat中运行！

1. 下载tomcat，官网下载即可 [tomcat9](http://tomcat.apache.org/download-90-22.tar.gz)

2. 解压这个文件

```shell
tar -zxvf apache-tomcat-9.0.22.tar.gz
```

1. 启动tomcat测试！执行 `./startup.sh` 脚本即可运行

```shell
# 执行 . startup.sh
[root@kuangshen bin]# ./startup.sh
```

Tomcat started.

### 防火墙配置

如果防火墙8080端口开了并且阿里云安全组也开放了这个时候就可以直接访问远程了！

### 查看firewall服务状态

```shell
systemctl status firewalld
```

#### 开启、重启、关闭firewalld.service服务

```shell
# 开启
service firewalld start

# 重启
service firewalld restart

# 关闭
service firewalld stop
```

### 查看防火墙规则

```shell
# 查看全部信息
firewall-cmd --list-all

# 只看端口信息
firewall-cmd --list-ports
```

#### 开启端口

```shell
# 开端口命令:
firewall-cmd --zone=public --add-port=8080/tcp --permanent

# 重启防火墙:
systemctl restart firewalld.service
```

命令含义：

- `--zone` # 作用域
- `--add-port=80/tcp` # 添加端口, 格式为: 端口/通讯协议
- `--permanent` # 永久生效，没有此参数重启后失效



# 关机部分

## 关机指令

关机指令为: `shutdown;`

- `sync` # 将数据由内存同步到硬盘中。

- `shutdown` # 关机指令, 你可以 `man shutdown` 来看一下帮助文档。例如你可以运行如下命令关机:

  - `shutdown -h 10` # 这个命令告诉大家, 计算机将在10分钟后关机
  - `shutdown -h now` # 立马关机
  - `shutdown -h 20:25` # 系统会在今天20:25关机
  - `shutdown -h +10` # 十分钟后关机
  - `shutdown -r now` # 系统立马重启
  - `shutdown -r +10` # 系统十分钟后重启
  - `reboot` # 就是重启，等同于 `shutdown -r now`
  - `halt` # 关闭系统, 等同于 `shutdown -h now` 和 `poweroff`

## 终端操作示例

```plaintext
[root@localhost Desktop]# sync
[root@localhost Desktop]# shutdown
Shutdown scheduled for Mon 2020-03-23 06:21:14 PDT, use 'shutdown -c' to cancel.
[root@localhost Desktop]#
Broadcast message from root@localhost.localdomain (Mon 2020-03-23 06:20:14 PDT):
The system is going down for power-off at Mon 2020-03-23 06:21:14 PDT!
```

**注意**: 时间允许可以先同步数据再关机。