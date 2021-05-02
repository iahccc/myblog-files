---
title: linux笔记
cover: 'https://cdn.jsdelivr.net/gh/iahccc/PicBed/img/archive_img.jpeg'
date: 2020-11-15 13:07:45
tags: linux apt vim 
categories: linux
---

## apt
### apt软件卸载
    apt-get purge / apt-get –purge remove 
删除已安装包（不保留配置文件)。 
如软件包a，依赖软件包b，则执行该命令会删除a，而且不保留配置文件

    apt-get autoremove 
删除为了满足依赖而安装的，但现在不再需要的软件包（包括已安装包），保留配置文件。

    apt-get remove 
删除已安装的软件包（保留配置文件），不会删除依赖软件包，且保留配置文件。

    apt-get autoclean 
APT的底层包是dpkg, 而dpkg 安装Package时, 会将 *.deb 放在 /var/cache/apt/archives/中，apt-get autoclean 只会删除 /var/cache/apt/archives/ 已经过期的deb。

    apt-get clean 
使用 apt-get clean 会将 /var/cache/apt/archives/ 的 所有 deb 删掉，可以理解为 rm /var/cache/apt/archives/*.deb。

## vim
### vim配置
    set number
	set ts=4
	set expandtab
	set autoindent
	set softtabstop=4
	set shiftwidth=4
	inoremap jk <esc>

	set fileformats=unix
    set enc=utf-8
    set fenc=utf-8

### gvim配置
    set smartindent   "设置智能缩进
    set shortmess=atI "去掉欢迎界面
    colorscheme slate   "sublime的配色方案
    set guifont=Consolas:h12    "字体与字号
    set langmenu=zh_CN.UTF-8    "设置菜单语言
    source $VIMRUNTIME/delmenu.vim  "导入删除菜单脚本，删除乱码的菜单
    source $VIMRUNTIME/menu.vim "导入正常的菜单脚本
    language messages zh_CN.utf-8   "设置提示信息语言

    "set guioptions-=m   "去除菜单栏 
    set guioptions-=T   "去除工具栏

### 常用操作
#### 多行编辑
1. 在命令行模式按Ctrl+V进入块模式
2. 利用上下左右键选择需要编辑多少行、从哪开始
3. 输入大写字母“I”进入插入模式
4. 编辑行
5. 再次按Esc键回到命令行模式即可完成多行编辑

### 替换
全局替换a为b
    :%s/a/b/g

## 常用操作命令
杀死包含某关键字的所有进程
    ps -ef | grep param | grep -v grep | cut -c 9-15 | xargs kill -9
    或
    ps x | grep param | grep -v grep | awk ‘{print $1}’ | xargs kill -9
