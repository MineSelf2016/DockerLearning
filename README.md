# DockerLearning
Mr. Qi 的docker 学习记录

# Docker 简介
Docker 是基于Go 语言实现的云开源项目，诞生于2013年初，最初发起者是dotCloud（后更名为Docker Inc） 公司。<br>

Docker 的主要目标是 "Build, Ship and Run Any App, Anywhere."，即通过对应用组件的封装（Packaging）、分发（Distribution）、部署（Deployment）、运行（Runtime）等生命周期的管理，达到应用组件级别的“一次封装，到处运行”。这里的应用组件，既可以是一个Web 应用，也可以是一套数据库服务，甚至是一个操作系统或编译器。<br>

Docker 基于Linux 的多项开源技术提供了高效、敏捷和轻量级的容器方案，并且支持在多种主流云平台PaaS（Aliyun、AWS、Azure）和本地系统上部署。可以说Docker 为应用的开发和部署提供了“一站式”的解决方案。

## Docker 的作用
高效地构建应用。
举个简单的应用场景的例子。假设用户试图基于最常见的LAMP（Linux + Apache + MySQL + php）组合来运维一个网站。按照传统的做法，首先，需要安装Apache、MySQL和PHP 以及它们各自运行所依赖的环境；之后分别对它们进行配置（包括创建合适的用户、配置参数等）；经过大量的操作后，还需要进行功能测试，看是否工作正常；如果不正常，则以为这更多的事件代价和不可控的风险。可以想象，如果再加上更多的应用，事情会变得更加难以处理。<br>
更为可怕的是，一旦需要服务器迁移（例如从阿里云迁移到腾讯云），往往需要重新部署和调试。这些琐碎而无趣的“体力活”，极大地降低了工作效率。<br>
而Docker 提供了一种更为聪明的方式，通过容器来打包应用，意味着迁移只需要在新的服务器上启动需要的容器就可以了。这无疑将节约大量的宝贵时间，并降低部署过程出现问题的风险。

## Docker 的优势
Docker在开发和运维（DevOpvs）过程中，具有如下几个方面的优势。
<ul>
    <li>1. 更快速的交付和部署。
    <li>2. 更高效的资源利用。
    <li>3. 更轻松的迁移和扩展。
    <li>4. 更简单的更新管理。
</ul>

## Docker VS VM
作为一种轻量级的虚拟化方式，Docker 在运行应用上跟传统的虚拟机方式相比具有显著的优势：
<ul>
    <li>Docker 容器很快，启动和停止可以在秒级实现，这相比传统的虚拟机方式要快得多。
    <li>Docker 容器对系统资源需求很少，一台主机上可以同时运行数千个Docker 容器。
    <li>Docker 通过类似Git 的操作来方便用户获取、分发和更新应用镜像，指令简明，学习成本较低。
    <li>Docker 通过Dockerfile 配置文件来支持灵活的自动化创建和部署机制，提高工作效率。
</ul>


# Docker 的核心概念
介绍Docker 的三大核心概念：
<ul>
    <li>镜像（Image）
    <li>容器（Container）
    <li>仓库（Repository）
</ul>
理解这三个核心概念，就能顺利地理解Docker 的整个声明周期。

## Docker 镜像
Docker 镜像（Image）类似于虚拟机镜像，可以将它理解为一个面向Docker 引擎的只读模板，包含了文件系统！<br>
镜像是创建Docker 容器的基础。通过版本管理和增量的文件系统，Docker 提供了一套十分简单的机制来创建和更新现有的镜像，用户甚至可以从网上下载一个已经做好的应用镜像，并通过简单的命令就可以直接使用。

## Docker 容器
Docker 容器（Container）类似于一个轻量级的沙箱，Docker 利用容器来运行和隔离应用。<br>
容器是从镜像创建的应用运行实例，可以将其启动、开始、停止、删除，而这些容器都是相互隔离、互不可见的。<br>
读者可以把容器看作一个简易版的Linux 系统环境（这包括<font color="blue">root 用户权限</font>、<font color="blue">进程空间</font>、<font color="blue">用户空间</font>和<font color="blue">网络空间</font>等！），以及运行在其中的应用程序打包而成的应用盒子。

## Docker 仓库
Docker 仓库（Repository）类似于代码仓库，是Docker 集中存放镜像文件的场所。<br>
当用户创建了自己的镜像之后就可以使用push 命令将它上传到制定的公有或私有仓库。这样用户下次在另外一台机器上使用该镜像时，只需将其从仓库上pull 下来就可以了。


# Docker 镜像
Dokcer 运行容器前需要本地存在对应的镜像，如果镜像不存在本地，Docker 会尝试先从默认镜像仓库下载（默认使用Docker Hub公共注册服务器中的仓库），用户也可以通过配置，使用自定义的镜像仓库。


## 搜寻镜像
docker search 命令可以搜索远端仓库中共享的镜像，默认搜索Docker Hub 官方仓库中的镜像。用法为docker search TERM，支持的参数包括：
<ul>
    <li>--automated=false 是否显示自动创建的镜像。
    <li>--no-trunc=false 输出信息不截断显示。
    <li>-s, --stars=0 指定仅显示评价为指定星级以上的镜像。
</ul>
默认的输出结果将按照星级评价进行排序。官方的镜像说明是官方项目组创建和维护的，automated 资源则允许用户验证镜像的来源和内容。

## 获取镜像
docker pull NAME[:TAG]命令从网络上下载镜像。
例，从Docker Hub 的Ubuntu 仓库下载一个最新的Ubuntu 操作系统的镜像。
```bash
sudo docker pull ubuntu
```
<br>
通过指定标签来下载特定版本的某一个镜像，例如14.04 标签的镜像：
```bash
sudo docker pull ubuntu:14.04
```
<br>
通过指定完整仓库注册服务器地址从特定仓库实现下载：
```bash
sudo docker pull dl.dockerpool.com:5000/ubuntu
```
<br>

## 查看镜像信息
docker images 命令可以列出本地host 上已有的镜像。
```bash
sudo docker images
```
在列出的信息中，可以看到几个字段信息：
<ul>
    <li>REPOSITORY: 来自于哪个仓库，比如ubuntu 仓库。
    <li>TAG：镜像的标签信息，比如14.04。
    <li>IMAGE ID：镜像的ID 号（唯一）。
    <li>CREATED：创建时间。
    <li>VIRTUAL SIZE：镜像大小。
</ul>
其中镜像的ID 信息十分重要，它唯一标识了镜像。
<br>
docker inspect IMAGE ID 命令可以获取该镜像的详细信息：
```bash
sudo docker inspect a2a15febcdf3
```
docker inspect 命令返回的是一个JSON 格式的消息，如果只需要获取其中的一项内容，可以使用-f 参数来指定。例如，获取镜像的 "Config" 信息：
```bash
sudo docker inspect -f {{".Config}} a2a15febcdf3
```

## tag 的增加与移除
docker tag IMAGE ID REPOSITORY:TAG 命令为镜像增加新的仓库与标签：
```bash
sudo docker tag a2a15febcdf3 centos:14.04
```
docker rmi -f REPOSITORY:TAG 命令删除某个镜像的tag
```bash
sudo docker rmi [-f] centos:14.04
```

## 删除镜像
docker rmi IMAGE ID 命令可以删除镜像。
```bash
sudo docker rmi a2a15febcdf3
```
<br>
删除镜像前需确保：只有一个标签而没有额外的标签指向它且没有该镜像创建的容器。

## 创建镜像
docker commit [OPTIONS] CONTAINER ID [REPOSITORY[:TAG]] 命令用于从本地创建一个新镜像，类似git commit 命令提交一个新版本。
OPTIONS 选项主要包括：
<ul>
    <li>-a, --author="作者信息"
    <li>-m, --message="提交消息"
    <li>-p, --pause=true 提交时是否暂停容器运行。
</ul>
例，运行ubuntu 镜像，在其中进行修改操作，之后退出，使用commit 命令提交一个新的镜像。
```bash
docker run -t -i a2a15febcdf3 /bin/bash
touch testDockerCommit
exit
```
返回CONTAINER ID fe7c37f14f8d

```bash
sudo docker commit -a "Qi Cong" -m "Added a new file." fe7c37f14f8d ubuntu:test
```

## 导出/导入镜像
docker save -o PATH (IMAGE ID | REPOSITORY:TAG) 命令将指定镜像导出到本地文件
```bash
sudo docker save -o dockerfiles/ubuntu_latest.tar ubuntu:latest
```
当使用IMAGE ID 导出时，不会保存REPOSITORY:TAG 该元数据。

docker load --input PATH 或 docker load < PATH 导入本地镜像文件：
```bash
sudo docker load < dockerfiles/ubuntu_latest.tar
```

## 上传镜像
docker push REPOSITORY:TAG 命令用于将镜像推送到远端服务器，第一次使用时，会提示输入登录信息或进行注册。
```bash
sudo docker push ubuntu/latest
```

## 从镜像启动容器
<br>
使用以下命令从镜像中创建ubuntu 容器，并在其中运行bash 应用：
```bash
sudo docker run -t -i ubuntu /bin/bash
```
<br>
docker ps -a 命令查看运行的容器：
```bash
sudo docker ps -a
```
<br>
docker rm CONTAINER ID 命令删除指定容器。
