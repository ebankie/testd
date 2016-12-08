---
author: ebankie
comments: true
date: 2014-05-29 07:07:13+00:00
layout: post
link: http://www.ebankie.com/blog/?p=143
slug: nodejs-%e5%ba%94%e7%94%a8%e5%bc%80%e5%8f%91socketio%ef%bc%8cmemcacheredis-foreverno%e4%bd%bf%e7%94%a8
title: nodejs 应用开发socketio，memcache,redis ,forever使用
wordpress_id: 143
categories:
- 技术文章
tags:
- forever
- nodejs
- socketio
---

[well]一、安装

全局安装使用 forever

#:npm install forever -g

在应用目录安装memcahce ,redis ,socket.io

#:npm install memcache

#:npm install redis

#:npm install socket.io

安装完具体使用 请查看  node_modules 下 相应 example.js  或者 simple ,test 亦可

├── memcache@0.3.0
├─┬ memcached@0.2.8
│ ├─┬ hashring@0.0.8
│ │ ├── bisection@0.0.3
│ │ └── simple-lru-cache@0.0.1
│ └─┬ jackpot@0.0.6
│ └── retry@0.6.0
├── redis@0.10.1
├─┬ socket.io@0.9.17
│ ├── base64id@0.1.0
│ ├── policyfile@0.0.4
│ ├── redis@0.7.3
│ └─┬ socket.io-client@0.9.16
│ ├── active-x-obfuscator@0.0.1
│ ├── uglify-js@1.2.5
│ ├─┬ ws@0.4.31
│ │ ├── commander@0.6.1
│ │ ├── nan@0.3.2
│ │ ├── options@0.0.5
│ │ └── tinycolor@0.0.1
│ └── xmlhttprequest@1.4.2
└── zeparser@0.0.7 invali

[/well]

二、使用forever

node应用遇到出错时会退出服务，这对于上线的服务是致命的，伤害用户，流失用户。

使用forever 进程管理应用

起动：

#：forever start app.js

查看node服务器状态

#:forever list

    
    <span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;">查看 进程是否起动，</span>


#:netstat -npl | grep 1234

或

#: lsof -i:1234

找出node app.js 的pid

#:ps -aux  | grep node

kill -9 进程号，看看是否重新起动，注 pid 重新起动后改变

/usr/local/bin/forever -p  -l /data/nodelogs/access.log -e /data/nodelogs/error.log -a -w start /data/node/server.js   查看日志

访问日志在 查看 forever list 可以列出

三、socketio 遇到的问题

Access-Control-Allow-Origin　跨域问题在socket1.04版本几乎是没有办法解决

/npm -install socketio@0.9.16

回复到0.9.16问题解决



四、监控脚本


