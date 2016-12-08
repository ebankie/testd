---
author: ebankie
comments: true
date: 2014-01-18 02:51:34+00:00
layout: post
link: http://www.ebankie.com/blog/?p=101
slug: gvim-vim-windows-%e4%b8%8b%e9%85%8d%e7%bd%ae
title: gvim /vim windows 下配置
wordpress_id: 101
categories:
- 技术文章
tags:
- vim
- 总结，方法
---

"设置UTF－8

set encoding=utf-8
set termencoding=utf-8
set formatoptions+=mM
set fencs=utf-8,gbk

set fileencodings=utf-8,chinese,latin-1

"解决consle输出乱码
language messages zh_CN.utf-8

"菜单乱码
source $VIMRUNTIME/delmenu.vim
source $VIMRUNTIME/menu.vim
