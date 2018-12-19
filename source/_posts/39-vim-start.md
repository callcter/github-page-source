---
title: Vim开始篇
tags:
  - vim
url: 138.html
id: 138
categories:
  - 开发工具
date: 2016-07-23 10:11:55
---

关于Vim，就不做介绍了，太多，我选择用Vim原因，个人网站的后台只有我一个人做，所以打算直接服务器上写，Vim是Linux上一个不错的选择，于是乎~ 直接贴个人配置单：

  1 set encoding=utf-8 " 文件编码                                                                                                                                                                                                                                             
  2 set nocompatible " 不要支持vi                                                                                                      
  3 set backspace=indent,eol,start " 配置backspace工作方式                                                                             
  4 set nu " 开启行号
  5 set ruler " 显示光标行列信息                                                                                                       
  6 set nowrap " 取消换行
  7 set showcmd " 状态栏显示正在输入的命令                                                                                             
  8 
  9 syntax enable
 10 syntax on " 语法高亮
 11 
 12 set t_Co=256 " 配色方案256色                                                                                                       
 13 set showmatch " 设置匹配模式，括号匹配                                                                                             
 14 set ignorecase " 搜索时忽略大小写                                                                                                  
 15 
 16 set cursorline " 突出显示当前行
 17 set tabstop=2 " 设置tab长度=2
 18 colorscheme Tomorrow-Night " 配色
 19 
 20 " 设置取消备份 禁止临时文件生成
 21 set nobackup
 22 set noswapfile
 23 
 24 filetype off
 25 
 26 set rtp+=~/.vim/bundle/Vundle.vim
 27 call vundle#begin()
 28 
 29 Plugin 'VundleVim/Vundle.vim'
 30 Plugin 'mattn/emmet-vim'
 31 Plugin 'vim-scripts/L9'
 32 Plugin 'majutsushi/tagbar'
 33 Plugin 'scrooloose/nerdtree'
 34 Plugin 'ivalkeen/nerdtree-execute'
 35 Plugin 'tpope/vim-fugitive'
 36 Plugin 'vim-scripts/taboo.vim'
 37 Plugin 'vim-scripts/neocomplcache'
 38 Plugin 'terryma/vim-multiple-cursors'
 39 Plugin 'gregsexton/MatchTag'
 40 Plugin 'vim-scripts/Syntastic'
 41 Plugin 'Valloric/YouCompleteMe'
 42 
 43 call vundle#end()       
 44 filetype plugin indent on " 加载插件和支持缩进