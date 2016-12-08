---
author: ebankie
comments: true
date: 2015-08-20 12:55:28+00:00
layout: post
link: http://www.ebankie.com/blog/?p=231
slug: php-json_decode%e8%bf%94%e5%9b%9enull-%e7%89%b9%e5%88%ab%e6%98%af%e4%bd%bf%e7%94%a8file_get_contents
title: php json_decode返回NULL 特别是使用file_get_contents
wordpress_id: 231
categories:
- 技术文章
---

今天遇到了json_decode无法解析json的情况，很是头痛，查来查去原来 编码服务器可以正常解析，但是解码服务无法解析，等到把整个json字串 放到页面上查看元素突然发现json字串头部多了字串，考虑是BOM问题，然后在编码服务打印字串长度6617到了解码服务打印长度成了6620 还要多一点，因些断定是BOM引起的json_decode无法解析。

if(preg_match('/^\xEF\xBB\xBF/',$receive_part))
{
$receive_part= substr($receive_part,3);
}

去掉BOM 解析正常,原来形成应该是编码服务使用header('Content-Type:application/json; charset=utf-8');转UTF－8时形成的BOM 。
