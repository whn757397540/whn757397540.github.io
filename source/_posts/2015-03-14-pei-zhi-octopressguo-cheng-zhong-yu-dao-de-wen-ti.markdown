---
layout: post
title: "配置Octopress过程中遇到的问题"
date: 2014-12-19 18:53:31 +0800
comments: true
categories: 博客配置
---

配置Octopress过程中遇到的问题
我是在ubuntu14.04下配置的octopress，第一次和第二次配置遇到了同样的问题。

<!--more-->

首先，配置过程可以去octopress的文档中看,虽然是全英文的，但是也不难看懂（推荐使用RVM安装）。

然后在rake generate 的时候出现了问题，仔细看了一下结果，是因为javascript没有安装，于是百度安装js，这个百度的大部分都是可以解决的。

接着在rake deploy的时候又出现了问题，推送不上去，报错如下

```ruby
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/yeesterbunny/yeesterbunny.github.com.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
这个问题，我是找到这篇[博客](http://allenyee.me/blog/2013/08/21/what-i-learned-from-hosting-octopress-on-github/)解决的.

其实也就是两个命令：

```ruby
cd octopress/_deploy
```

```ruby
git pull origin master
```
执行完之后再进行

```ruby
rake deplpy
```

就没有问题了。 如果有人遇到了跟我一样的问题，希望这些能够给你一些帮助！

之前的博客忘记了推送source，结果现在源文件找不到了，所以只能重开一个了2333333T_T!!

[<--Back](/blog/archives)
