title: 使用Vundle来管理VIM插件
date: 2013-10-09 15:40:15
categories: [ubuntu, vim]
tags: [ubuntu, vim, linux, vundle]
---

今天来介绍一下用怎么用Vundle来管理vim的各种插件

### 安装Vundle:
```
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
```
<!-- more -->

### 配置bundles：
```
set nocompatible               " be iMproved
filetype off                   " required!

set rtp+=~/.vim/bundle/vundle/
call vundle#rc()

" let Vundle manage Vundle
" required! 
Bundle 'gmarik/vundle'

" My Bundles here:
"
" original repos on github
Bundle 'tpope/vim-fugitive'
Bundle 'Lokaltog/vim-easymotion'
Bundle 'rstacruz/sparkup', {'rtp': 'vim/'}
Bundle 'tpope/vim-rails.git'
" vim-scripts repos
Bundle 'L9'
Bundle 'FuzzyFinder'
" non github repos
Bundle 'git://git.wincent.com/command-t.git'
" git repos on your local machine (ie. when working on your own plugin)
Bundle 'file:///Users/gmarik/path/to/plugin'
" ...

filetype plugin indent on     " required!
"
" Brief help
" :BundleList          - list configured bundles
" :BundleInstall(!)    - install(update) bundles
" :BundleSearch(!) foo - search(or refresh cache first) for foo
" :BundleClean(!)      - confirm(or auto-approve) removal of unused bundles
"
" see :h vundle for more details or wiki for FAQ
" NOTE: comments after Bundle command are not allowed..
```

可以到[这里](http://vim-scripts.org/vim/scripts.html)查找你需要的插件

### 安装配置的bundles：
启动`vim`，执行`:BundleInstall`

##### 参考

1. [https://github.com/gmarik/vundle](https://github.com/gmarik/vundle)