---
layout: post
title: Markdown 学习记录一(概述) 
description: 从宗旨、兼容HTML、特殊字符自动转换三方面精简官方文档之内容 
category: markdown 
keywords: markdown
---

####宗旨####


* Markdown的目标是实现**易读易写**
* 既有text-to-Html格式的影响，更大受纯文本电子邮件格式的启发
* 全由一些精挑细选符号组成，一目了然


####兼容HTML####


* Markdown不是为了取代HTML，而是为了成为一种适用于网络的**书写**语言
* HTML是一种**发布**的格式，Markdown是一种**书写**的格式，只涵盖纯文本可以涵盖的范围
* 不在Markdown涵盖范围内的标签，可以直接在文档里可以直接用HTML撰写
* 一些HTML区块元素需要在前后加上空行隔开，区块间Markdown格式语法不会被处理
* HTML区段标签可以随意使用，甚至完全替代Markdown格式，Markdown语法在区段标签中是有效的


####特殊字符自动转换####
* 不同于HTML中，一些字符(<,&)需要手动进行转换，Markdown会根据当前的语境自动对字符进行转换
* 无论是在行内还是区块，code范围内的<和&都会转换成HTML实体
