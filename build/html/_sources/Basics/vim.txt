.. _ls:

==========
vim
==========

多窗口操作
==========

* 使用 ``:sp`` 可以水平分割窗口
* 使用 ``:vs`` 可以垂直分割窗口
* 使用 ``Ctrl + w`` 可以快速在窗口间切换


VIM 样式 http://vimcolors.com/
::

    cd ~
    git clone https://github.com/altercation/vim-colors-solarized.git
    mkdir -p ~/.vim/colors
    mv vim-colors-solarized/colors/solarized.vim ~/.vim/colors/

编辑 ``vim ~/.vimrc``
::

    syntax enable
    set background=dark
    colorscheme solarized