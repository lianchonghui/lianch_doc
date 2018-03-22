**关键词**
```
PythonIDE vim+python
```
---
**环境**
> - Ubuntu 16.04/CentOS 6.5以上
> - Vim 7.3以上

---
**注**
> - windows下的git bash的vim安装 ```Valloric/YouCompleteMe```插件会失败。
---
## 最终效果图
![vimide效果图][1]
## Vim的安装和更新 
### 查看vim的是否安装和版本情况
**vim的版本必须是7.3以上版本才支持Vundle插件。**

```$ vim --version```

**如果vim未安装执行以下命令安装：**

CentOS下：```# yum install vim```

Ubuntu下：```$ sudo apt-get install vim```

**如果vim已安装,会显示如下信息：**
```
VIM - Vi IMproved 7.4 (2013 Aug 10, compiled Nov 24 2016 16:44:48)
Included patches: 1-1689
Extra patches: 8.0.0056
......
```

**vim版本低于7.3更新**

CentOS下：```# yum update vim```

Ubuntu下：```$ sudo apt-get update vim```

## 安装并配置vim的Vundle插件
Vim有多个扩展管理器，这里使用Vundle。你可以把它想象成Vim的pip。有了Vundle，安装和更新vim插件这种事情不费吹灰之力。
### 使用git安装Vundle插件

**如果git未安装先安装git：**

CentOS下：```# yum install git```

Ubuntu下：```$ sudo apt-get install git```

**安装Vundle：**

```$ git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim```

该命令将下载Vundle插件管理器，并将它放置在你的Vim编辑器bundles文件夹中**(~/.vim/budle/)**。现在，你可以通过.vimrc配置文件来管理所有扩展了。

### 配置```.vimrc```文件

在你的用户的home文件夹中新建```.vimrc```文件：```$ vim ~/.vimrc```

**添加以下内容：**
```
set nocompatible              " required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.vim'

Plugin 'tmhedberg/SimpylFold'

Plugin 'vim-scripts/indentpython.vim'

Plugin 'scrooloose/syntastic'

Plugin 'nvie/vim-flake8'

Plugin 'jnurmine/Zenburn'
Plugin 'altercation/vim-colors-solarized'

Plugin 'scrooloose/nerdtree'

Plugin 'jistr/vim-nerdtree-tabs'

Plugin 'kien/ctrlp.vim'

Bundle 'Valloric/YouCompleteMe'
" Add all your plugins here (note older versions of Vundle used Bundle instead of Plugin)

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required

"split navigations
nnoremap <C-J> <C-W><C-J>
nnoremap <C-K> <C-W><C-K>
nnoremap <C-L> <C-W><C-L>
nnoremap <C-H> <C-W><C-H>

" Enable folding
set foldmethod=indent
set foldlevel=99

" Enable folding with the spacebar
nnoremap <space> za

set encoding=utf-8
set nu

"highlight column&line
set cursorcolumn
"set cursorline
highlight CursorColumn cterm=NONE ctermbg=gray ctermfg=black guibg=NONE guifg=NONE
"highlight CursorLine   cterm=NONE ctermbg=NONE ctermfg=green guibg=NONE guifg=NONE

```

### ```.vimrc```文件配置详解

**使用Vundle的基本配置：**
```
set nocompatible              " required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.vim'

" Add all your plugins here (note older versions of Vundle used Bundle instead of Plugin)

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
```

**SimpyFold:根据缩进控制文本折叠**

```Plugin 'tmhedberg/SimpylFold'```

如果希望看到折叠代码的文档字符串,可添加：

```let g:SimpylFold_docstring_preview=1```

**identpython:符合PEP8的python代码自动缩进**

```Plugin 'vim-scripts/indentpython.vim'```

**syntastic:每次保存时检查代码语法**

```Plugin 'scrooloose/syntastic'```

**vim-flake8:PEP8风格检查**

```Plugin 'nvie/vim-flake8'```

**Zenburn:适用于终端模式下的主题；vim-colors-solarized:使用于GUI下的主题**

```
Plugin 'jnurmine/Zenburn'
Plugin 'altercation/vim-colors-solarized'

"控制GUI和终端模式下主题
if has('gui_running')
  set background=dark
  colorscheme solarized
else
  colorscheme Zenburn
endif
```

**nerdtree：文件树**

```
Plugin 'scrooloose/nerdtree'
Plugin 'jistr/vim-nerdtree-tabs'
```

启动vim以后，命令模式下：```:NERDTree```打开文件树
如果想隐藏.pyc文件：

```let NERDTreeIgnore=['\.pyc$', '\~$'] "ignore files in NERDTree```

**ctrlp:vim按crtl+p中搜索文件**

```Plugin 'kien/ctrlp.vim'```

**YouCompleteMe:支持python自动补全**

```Bundle 'Valloric/YouCompleteMe'```

**vim窗口切换,在多个vim编辑下切换，nnoremap表示按键映射，如nnoremap <C-J> <C-W><C-J>将Ctrl+w,j映射为Ctrl+j**
```
"split navigations
nnoremap <C-J> <C-W><C-J>"切换到下边编辑框
nnoremap <C-K> <C-W><C-K>"切换到上边编辑框
nnoremap <C-L> <C-W><C-L>"切换到右边编辑框
nnoremap <C-H> <C-W><C-H>"切换到左边编辑框
```

**配置支持代码折叠**

```
" Enable folding
set foldmethod=indent
set foldlevel=99
" Enable folding with the spacebar
nnoremap <space> za
```

**设置编码**

```set encoding=utf-8```

**设置显示行号**

```set nu```

**设置高亮行和列**
```
"highlight column&line
set cursorcolumn
"set cursorline
highlight CursorColumn cterm=NONE ctermbg=gray ctermfg=black guibg=NONE guifg=NONE
"highlight CursorLine   cterm=NONE ctermbg=NONE ctermfg=green guibg=NONE guifg=NONE
```

## 安装```.vimrc```中配置的插件

**打开vim：**```$ vim```

**在vim的命令模式下执行：**```:PluginInstall```

## 部分插件手动启用
**NERDTree**

vim命令模式下执行：```:NERDTree```

在文件树中：

按```o```键，目录展开或关闭，文件在右侧当前编辑框打开。

按```i```键，文件右侧新编辑框打开。

---
## 可能遇到的问题
暂无


--------------------

文章引用和转载请注明出处，如果有什么问题欢迎交流！

作者 [@练崇辉][101]

Email: `lianchonghui@foxmail.com`

2018 年 03月 22日 



[1]: https://raw.githubusercontent.com/lianchonghui/photorepository/master/markdown/2018/3/22/vim_ide.png
[101]: https://www.lianch.com

