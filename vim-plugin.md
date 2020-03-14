---
title: QEMU-KVM
date: 2020-03-14 17:16:46
tags:
---
leaderF
类/方法/变量相关侧边栏
模糊查找插件LeaderF在vim7.4上就可以异步查找。
tagbar ctrlp

源文件头文件切换
操作：ctrl-R
插件：https://github.com/ericcurtin/CurtineIncSw.vim

标签跳转：
插件：https://github.com/andymass/vim-matchup
操作：%，跳转到下一个标签

搜索
插件：https://github.com/wsdjeg/FlyGrep.vim
操作：Ctrl+F，全项目搜索， 退出， 选择下一个，选择上一个

语法高亮
https://github.com/sheerun/vim-polyglot

折叠
https://github.com/tmhedberg/SimpylFold 操作：zc关闭折叠并zo打开折叠

语法检查
ale：github.com/w0rp/ale
vim-polyglot
https://github.com/vim-syntastic/syntastic

修改比较vim-signify

airline
vim-fugitive

跳转
插件：universal ctags
使用符号索引插件 ctags 以及 tags 异步生成工具 vim-gutentags:

编译运行 AsyncRun

参数提示 echodoc
写 C/C++ 时函数忘了可以用上面的 YCM 补全，若是参数忘记了则可以使用 echodoc 插件。当用 YCM 的 tab 补全了一个函数名后，只要输入左括号，下面命令行就会显示出该函数的参数信息，并且随着光标移动高亮出当前参数位置。




#####################
编辑辅助 vim-unimpaired
Plug 'tpope/vim-unimpaired'
unimpaired 插件定义了一系列括号开头的快捷键，被称为官方 Vim 中丢失的快捷键。详见插件文档。

###############################################################e.g.
" Plugins will be downloaded under the specified directory.
call plug#begin('~/.vim/plugged')

" Declare the list of plugins.
Plug 'bfrg/vim-cpp-modern'
Plug 'junegunn/seoul256.vim'
Plug 'vim-airline/vim-airline'
Plug 'airblade/vim-gitgutter'
Plug 'dense-analysis/ale'
Plug 'ajh17/VimCompletesMe'
Plug 'preservim/nerdtree', { 'on':  'NERDTreeToggle' }
Plug 'ludovicchabant/vim-gutentags'
Plug 'Shougo/echodoc.vim'
Plug 'ctrlpvim/ctrlp.vim'
Plug 'majutsushi/tagbar'
Plug 'ericcurtin/CurtineIncSw.vim'
Plug 'tmhedberg/SimpylFold'

" List ends here. Plugins become visible to Vim after this call.
call plug#end()



let g:ale_linters_explicit = 1
let g:ale_completion_delay = 500
let g:ale_echo_delay = 20
let g:ale_lint_delay = 500
let g:ale_echo_msg_format = '[%linter%] %code: %%s'
let g:ale_lint_on_text_changed = 'normal'
let g:ale_lint_on_insert_leave = 1
let g:airline#extensions#ale#enabled = 1

let g:ale_c_gcc_options = '-Wall -O2 -std=c99'
let g:ale_cpp_gcc_options = '-Wall -O2 -std=c++14'
let g:ale_c_cppcheck_options = ''
let g:ale_cpp_cppcheck_options = ''

set tags=./.tags;,.tags
" gutentags 搜索工程目录的标志，碰到这些文件/目录名就停止向上一级目录递归
let g:gutentags_project_root = ['.root', '.svn', '.git', '.hg', '.project']

" 所生成的数据文件的名称
let g:gutentags_ctags_tagfile = '.tags'

" 将自动生成的 tags 文件全部放入 ~/.cache/tags 目录中，避免污染工程目录
let s:vim_tags = expand('~/.cache/tags')
let g:gutentags_cache_dir = s:vim_tags


map <F5> :call CurtineIncSw()<CR>


###############################################################.vimrc
set nu
set relativenumber
set showcmd     " Show (partial) command in status line.
set showmatch   " Show matching brackets.
set ignorecase  " Do case insensitive matching
set smartcase   " Do smart case matching
set incsearch   " Incremental search
set autowrite   " Automatically save before commands like :next and :make
set hidden      " Hide buffers when they are abandoned
set mouse=a     " Enable mouse usage (all modes)
set cursorline             " Find the current line quickly.
" set list                   " Show non-printable characters.


set autoindent             " Indent according to previous line.
set expandtab              " Use spaces instead of tabs.  set softtabstop=4         " Tab key indents by 4 spaces.
set shiftwidth=4         " >> indents by 4 spaces.
set shiftround             " >> indents to next multiple of 'shiftwidth'.
set hlsearch
set incsearch

set backup
set backupext   =-vimbackup
set updatecount =60000
set undofile

