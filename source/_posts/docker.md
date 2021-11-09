---
title: Docker
date: 2021-11-09 22:30:11
categories: 
- 测试开发
tags:
- docker
---
---

## 1.docker 历史:
- 2013 ~ 开源应用容器引擎，go语言开发，可以让开发者打包他们的应用及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的系统

## 2.docker 优点:
- 快速交付应用，打包快，加快测试，加快发布，缩短交付周期
- 复杂环境管理，应用隔离，开发环境/测试环境/线上环境
- 轻量级：对系统来说，一个docker只是一个进程，一个系统可以运行上千个容器

## 3. docker 架构:

```
- client:
	docker build
	docker pull
	docker run
	
- docker_host:
	docker_dameon
	
- registry:
	下载。。。
```

## 4.docker与虚拟机的区别:
```
	容器与容器只是进程的隔离，虚拟机则是完全的资源隔离
	虚拟机启动慢，可能要几分钟，docker秒启动
	容器使用宿主操作系统的内核，虚拟机是完全独立的内核
```

## 5.基本概念:
```
	镜像： docker images,每个镜像都可能依赖一个或多个下层的镜像组成另一个镜像，AUFS文件系统
	仓库：Docker registry 集中放镜像的地方
	容器：docker containers，镜像运行后的进程
```

## 6.docker安装与配置:
	