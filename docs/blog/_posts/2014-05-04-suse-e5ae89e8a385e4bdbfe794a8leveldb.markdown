---
author: ebankie
comments: true
date: 2014-05-04 07:13:14+00:00
layout: post
link: http://www.ebankie.com/blog/?p=135
slug: suse-%e5%ae%89%e8%a3%85%e4%bd%bf%e7%94%a8leveldb
title: SUSE 安装使用levelDB
wordpress_id: 135
categories:
- 技术文章
---

一，安装步骤

1,官网下载leveldb 我的是 leveldb-1.13.0  安装省。

2,PHP下下载源码生成leveldb.so 配置php.ini 重起apache/nginx

/ php-m 查看是否安装成功

二，使用配置例子如下：

[well]Well

$options = array(
'create_if_missing' => true, // if the specified database didn't exist will create a new one
'error_if_exists' => false, // if the opened database exsits will throw exception
'paranoid_checks' => false,
'block_cache_size' => 8 * (2 << 20),
'write_buffer_size' => 4<<20,
'block_size' => 4096,
'max_open_files' => 1000,
'block_restart_interval' => 16,
'compression' => LEVELDB_SNAPPY_COMPRESSION,
'comparator' => NULL, // any callable parameter which returns 0, -1, 1
);
/* default readoptions */
$readoptions = array(
'verify_check_sum' => false,
'fill_cache' => true,
'snapshot' => null
);

/* default write options */
$writeoptions = array(
'sync' => false
);
/*
* Basic operate methods: set(), get(), delete()
*/
//$db->put("Key", "Value is vale");
$db->set("Key2", "Value2"); // set() is an alias of put()
var_dump($db->get("Key"));
$batch = new LevelDBWriteBatch();
$batch->put("key2", "batch value");
$batch->put("key3", "batch value");
$batch->set("key4", "a bounch of values"); // set() is an alias of put()
$batch->delete("some key");

// Write once
$db->write($batch);
echo '<br/>';

[/well]

官方说每秒40W/S 吞吐量，稍后补充使用性能情况
