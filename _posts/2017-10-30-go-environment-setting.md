---
layout: post
title:  "go"
---

# Ubuntu下安装go环境变量的问题

一般开发建议不要用root帐号

go安装目录：/usr/local/go/bin
工作源码目录放到 $HOME/work

然后在你的~/.zshrc中加:

```bash
 export PATH=$PATH:/usr/local/go/bin
 export GOPATH=$HOME/work

```
