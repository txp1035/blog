---
title: '关于umi的半自动化脚本'
category: 技术
tags: [笔记, 前端]
date: 2019-5-19
---

方便快速生成 servers/mock/models...

<!-- more -->

## 背景

因为使用使用 umi 和 dva 所以会大量写 servers/mock/models 文件，上百个接口复制起来还是很麻烦的，所以写个脚本自动生成 servers/mock/models 文件。

## 思路

通过 node 读取 swagger 数据，获取接口信息，再使用模板字符串拼接，然后通过 node 写入到文件中。

## 具体实现

1. 为了方便二次使用，考虑设置配置文件，配置文件主要用来设置输入和输出的配置，输入是 swagger 的 url，输出是生成的文件名、文件里包含的模块和文件类型（servers/mock/models）。
2. 同样是为了方便二次使用，需要设置 servers/mock/models 的模板字符串生成方法。
3. 准备工作完毕（其实是写完了才分离出来的），开始写主体。首先需要请求到数据，通过 axios 请求配置文件中的 url 获取到 swagger 的数据。
4. 接下来需要创建一个存放我们文件的文件夹，我把它创建在根目录，并且为了防止二次执行失败，需要先检查更目录是否有我们需要的文件夹。
5. 有了存放文件的文件夹，就需要往里面写入我们需要的文件了，通过遍历配置项文件里包含的模块，可以生成我们需要的文件。
6. 生成文件前，需要把 swagger 里的数据取出来传入之前设置的模板字符串生成方法中得到文件内容。

整个流程大概就是这样，其中又遇到接口名相同的问题，因为我的接口名取的`/`最后一个名字，然后做了了重名处理，在名字后面加`_copy`，有点不优雅，哈哈。其实也可以用接口，把`/`换成`_`不过可能会太长。还有就是我想可以做成可视化的话自己可以通过界面配置接口名。大概像 easy-mock 那样，不过不仅仅生成 mock 文件。这个项目 mock 和 models 处理的不够好还需要优化。

## 项目地址

[txp-swagger](https://github.com/ShawDanon/txp-swagger)
