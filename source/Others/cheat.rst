.. _cheat:

==========
Cheat
==========

安装
==========

`cheat` 使用 `Python` 语言编写，所以需要先安装 `Python`

**Debian/Ubuntu**

::

    apt-get install python python-pip

**CentOS/RedHat**

::

    yum install python python-setuptools && easy_install pip

安装 `cheat`

::

    pip install cheat

配置
==========

编辑 `~/.bashrc` 添加以下可选配置

设置默认的编辑器

::

    export EDITOR='/usr/bin/nano'

自定义的命令备忘录存储路径

个人 cheatsheets （命令备忘录） 默认存储在 `~/.cheat` 目录里，可以通过 `DEFAULT_CHEAT_DIR` 指定为其他目录

::

    export DEFAULT_CHEAT_DIR='/path/to/my/cheats'

补全功能

::

    wget https://github.com/chrisallenlane/cheat/raw/master/cheat/autocompletion/cheat.bash
    mv cheat.bash /etc/bash_completion.d/

启用高亮显示

::

    export CHEATCOLORS=true

使用
==========

默认使用

::

    cheat ls

在 `cheat` 后面加上指定的命令即可查看命令使用的实例信息

::

    # Displays everything in the target directory
    ls path/to/the/target/directory
    
    # Displays everything including hidden files
    ls -a
    
    # Displays all files, along with the size (with unit suffixes) and timestamp
    ls -lh
    
    # Display files, sorted by size
    ls -S
    
    # Display directories only
    ls -d */
    
    # Display directories only, include hidden
    ls -d .*/ */

添加命令备忘录

默认情况下 Cheat 只支持基本的 Linux 命令和一些很常用的命令。
可以通过以下命令创建并编辑自定义的命令备忘录。

::

    cheat -e xyz

列出所有的命令备忘录

::

    cheat -l

查看帮助信息

::

    cheat -h

::

    Usage:
      cheat <cheatsheet>
      cheat -e <cheatsheet>
      cheat -s <keyword>
      cheat -l
      cheat -d
      cheat -v
    
    cheat allows you to create and view interactive cheatsheets on the
    command-line. It was designed to help remind *nix system
    administrators of options for commands that they use frequently,
    but not frequently enough to remember.
    
    Examples:
      To look up 'tar':
      cheat tar
    
      To create or edit the cheatsheet for 'foo':
      cheat -e foo
    
    Options:
      -d --directories  List directories on CHEATPATH
      -e --edit         Edit cheatsheet
      -l --list         List cheatsheets
      -s --search       Search cheatsheets for <keyword>
      -v --version      Print the version number
