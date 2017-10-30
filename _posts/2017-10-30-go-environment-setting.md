---
layout: post
title:  "go"
---

# Ubuntu下安装go环境变量的问题

一般开发建议不要用root帐号

可以这样搞
把你的go放到你 $HOME/opt/go
工作源码目录放到 $HOME/src/goworkspace

然后在你的~/.bashrc中加:

if [ -d $HOME/opt/go ];then
    GOROOT=$HOME/opt/go
    export GOROOT
    export PATH=$PATH:$GOROOT/bin
fi

# new dev
GO_WORKSPACE=$HOME/src/goworkspace
if [ -d $GO_WORKSPACE ];then
    export GOPATH=$GO_WORKSPACE
    export PATH=$PATH:$GOPATH/bin
fi
unset GO_WORKSPACE
如果你用其它shell,比如zsh
同理把这些丢到~/.zshrc 就行了


参考：https://segmentfault.com/q/1010000004646589