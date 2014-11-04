---
layout: post
title: Docker的Jenkins持续集成：使用docker插件
---
#{{ page.title }}#
## 背景 ##

!!! 本文章已发表于[InfoQ Docker栏目]()， 转载请注明InfoQ出处 !!!


基于Jenkins的持续集成已深入人心，它的一个重要的实践是构建的实际任务放在Slave（从属）机器上，Jenkins Master（主机）主要负责调度。这样能确保干净的构建环境和整体性能。

【图片1】

管理Slave机器能从侧面看出一个公司配置管理和自动化程度的专业性。Slave机器是构建的主要场所，涉及到的环境如果产品要求多，那么它就会非常复杂，一般有一下几个问题

1. 构建平台：不同操作系统 （Redhat/Ubuntu/SuSE)、不同语言环境（Ruby/Python/C/Java）和各个版本。有些语言有天生的跨平台，但也涉及到不同版本的切换，保证每个场景都有一个适合的机器用来构建会很繁琐。云和虚拟机是一个很好的方向。
2. 平衡干净系统和启动速度：为了保证构建的准确性，每次系统重启是个最有效的办法 （travis-ci就是这么做的），只是启动速度会受一些影响，这两者需要平衡。
3. 保留构建现场：构建经常会有出错的情况，开发人员总是希望能登陆到构建机器上重现，特别是复杂的情况。但是由于机器的限制，一般都只是保留Log（日志）来诊断，会耽误不少时间。如果是虚拟机，有些情况可以Snapshot（快照）构建机器，但是由于硬盘大小和Snapshot的开销，采用的也不常见。

Docker的出现可以从技术上一定程度解决这个问题，这篇文章就是介绍如何使用[Jenkins Docker Plugin]()来了解docker在持续集成中的作用。

## 演示环境 ##

废话少说，直接运行上演示环境，如果没有docker的运行环境，请自己安装。Windows/Mac用户推荐[boot2docker](http://boot2docker.com)

    $ docker run -v /var/lib/docker.sock:/docker.sock larrycai/jenkins-docker-demo1

它会自动下载相关的docker container（容器）并运行，主要是一个Jenkins container作为Jenkins Master、一个Ubuntu还有一个CentOS的container作为Slave机器。

[图2. 架构]

Jenkins服务的端口是8180，如果是boot2docker，那就是 http://192.168.59.103:8180

[图3：界面]

你可以直接运行里面的Job（任务）：docker-demo，它会启动两个子任务在Ubuntu和CentOS系统上编译并打包，一个任务会出错。

出错的任务的现场会保留，你可以启动并登陆检验错误在哪里

    $ docker run -it docker-demo-222 bash

下面我们来讲解docker起的作用和怎么配置的。

## Jenkins配置 ##

### docker plugin ###

docker插件需要Jenkins 。。。版本支持，幸好最新的LTS版本已经支持了。

为了作为docker container在Jenkins Slave中使用，Java环境和SSH服务需要配置好，已经有好多文章了，这里就不啰嗦了。

### 配置slave ###

配置界面

[配置界面图]

### Job中配置Slave ###

[Job界面图]

## 总结 ##

通过。。。

本文中所用的所有演示源代码都在github上 http://github.com/larrycai/jenkins-docker-demo1 。有些技巧你可以借用 

## 结尾 ##

这只是我一些粗浅的使用Docker经验，Docker是一个新技术，我也在不断的学习进步中，欢迎一起切磋。

如果喜欢这篇文章，请给个赞 （微博 @larrycaiyu），鼓励我再写几篇docker的文章。

忘了说，如果你要学习Docker，可以看我的两篇90分钟培训教材。

技术无极限，只怕没追求。

## 参考 ##
1. [Travis CI会替代Jenkins吗？](http://larrycaiyu.com/2012/03/06/travis-ci-is-evolution.html)
2. [jenkins docker plugin](https://wiki.jenkins-ci.org/display/JENKINS/Docker+Plugin)
3. [The Docker book](http://www.dockerbook.com/) 
4. Docker CodingWithMe 90分钟教材：[Learn Docker](http://www.slideshare.net/larrycai/learn-docker-in-90-minutes),[Build Service with Docker](http://www.slideshare.net/larrycai/build-service-withdockerin90mins)
    
 
 