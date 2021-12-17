# Docker 安装与使用 <!-- {docsify-ignore} --> 
## 1. 介绍

### 1.1. 容器技术的介绍
?> _Note_ 注意我们这里所说的容器container是指的一种技术，而Docker只是一个容器技术的实现，或者说让容器技术普及开来的最成功的实现

### 1.2. 容器正在引领基础架构的一场新的革命
- 90年代的PC
- 00年代的虚拟化
- 10年代的cloud
- 11年代的container

### 1.3. 什么是container(容器）？
容器是一种快速的打包技术

Package Software into Standardized Units for Development, Shipment and Deployment

- 标准化
- 轻量级
- 易移植

### 1.4. 为什么容器技术会出现？
#### 1.4.1. 容器技术出现之前
![img](docker-img/1.4.1.png ':size=500')

#### 1.4.2. 容器技术出现之后
![img](docker-img/1.4.2.png ':size=500')

#### 1.4.3. 容器 vs 虚拟机
![img](docker-img/1.4.3.png ':size=700')

#### 1.4.4. 简述
Linux Container容器技术的诞生于2008年（Docker诞生于2013年），解决了IT世界里“集装箱运输”的问题。Linux Container（简称LXC）它是一种内核轻量级的操作系统层虚拟化技术。Linux Container主要由Namespace和Cgroups两大机制来保证实现

- Namespace命名空间主要用于资源的隔离（诞生于2002年）
- Cgroups(Control Groups)就负责资源管理控制作用，比如进程组使用CPU/MEM的限制，进程组的优先级控制，进程组的挂起和恢复等等。（由Google贡献，2008年合并到了Linux Kernel）

### 1.5. 容器的标准化
`docker != container`
在2015年，由Google，Docker、红帽等厂商联合发起了OCI（Open Container Initiative）组织，致力于容器技术的标准化

#### 1.5.1. 容器运行时标准 （runtime spec）
简单来讲就是规定了容器的基本操作规范，比如如何下载镜像，创建容器，启动容器等。

#### 1.5.2. 容器镜像标准（image spec）
主要定义镜像的基本格式。

### 1.6. 容器是关乎“速度”
- 容器会加速你的软件开发
- 容器会加速你的程序编译和构建
- 容器会加速你的测试
- 容器会速度你的部署
- 容器会加速你的更新
- 容器会速度你的故障恢复

### 1.7. 容器的快速发展和普及
?> _Note_ 到2020年，全球超过50%的公司将在生产环境中使用container —— Gartner

![img](docker-img/1.7.png ':size=500')

## 2. 架构
![img](docker-img/2.png ':size=800')

## 3. 安装

?> _Note_ 本文以CentOS7安装为例，其他系统版本请参考[官方文档](https://docs.docker.com/engine/install/)

### 3.1. 卸载旧版本
```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 3.2. 使用存储库安装
#### 3.2.1. 设置存储库
安装 yum-utils包（提供 yum-config-manager 实用程序）并设置 **稳定的** 存储库
```shell
$ sudo yum install -y yum-utils

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

#### 3.2.2. 安装 Docker 引擎
1）安装 最新版本 的 Docker Engine 和 containerd，或者进入下一步安装特定版本
```shell
$ sudo yum install docker-ce docker-ce-cli containerd.io
```
2）启动 Docker
```shell
$ sudo systemctl start docker
```
3）通过运行以下命令验证 Docker Engine 是否已正确安装 hello-world 镜像
```shell
$ sudo docker run hello-world
```
此命令下载测试映像并在容器中运行。当容器运行时，它打印一条消息并退出。

## 4. 命令
### docker命令行的基本使用
docker + 管理的对象（比如容器，镜像） + 具体操作（比如创建，启动，停止，删除）

> 例子：
> - `docker image pull nginx`
> - `docker container stop web`

### 简化命令	
| **命令**          | **介绍**                                                               |
|-------------------|-------------------------------------------------------------------------------|
| [docker attach](/docker-cli-docs/reference/commandline/attach.md)     | 将本地标准输入、输出和错误流附加到正在运行的容器 <br>*Attach local standard input, output, and error streams to a running container* |
| [docker build](/docker-cli-docs/reference/commandline/build.md)      | 从Dockerfile生成映像 <br>*Build an image from a Dockerfile*                                            |
| [docker builder](/docker-cli-docs/reference/commandline/builder.md)    | 管理构建 <br>*Manage builds*                                                                 |
| [docker checkpoint](/docker-cli-docs/reference/commandline/checkpoint.md) | 管理检查点 <br>*Manage checkpoints*                                                            |
| [docker commit](/docker-cli-docs/reference/commandline/commit.md)     | 根据容器的更改创建新镜像 <br>*Create a new image from a container’s changes*                                 |
| [docker config](/docker-cli-docs/reference/commandline/config.md)     | 管理Docker配置 <br>*Manage Docker configs*                                                         |
| [docker container](/docker-cli-docs/reference/commandline/container.md)  | 管理容器 <br>*Manage containers*                                                             |
| [docker context](/docker-cli-docs/reference/commandline/context.md)    | 管理上下文 <br>*Manage contexts*                                                               |
| [docker cp](/docker-cli-docs/reference/commandline/cp.md)         | 在容器和本地文件系统之间复制文件/文件夹 <br>*Copy files/folders between a container and the local filesystem*               |
| [docker create](/docker-cli-docs/reference/commandline/create.md)     | 创建一个新容器 <br>*Create a new container*                                                        |
| [docker diff](/docker-cli-docs/reference/commandline/diff.md)       | 检查对容器文件系统上的文件或目录的更改 <br>*Inspect changes to files or directories on a container’s filesystem*           |
| [docker events](/docker-cli-docs/reference/commandline/events.md)     | 从服务器获取实时事件 <br>*Get real time events from the server*                                          |
| [docker exec](/docker-cli-docs/reference/commandline/exec.md)       | 在正在运行的容器中运行命令 <br>*Run a command in a running container*                                          |
| [docker export](/docker-cli-docs/reference/commandline/export.md)     | 将容器的文件系统导出为tar归档 <br>*Export a container’s filesystem as a tar archive*                              |
| [docker history](/docker-cli-docs/reference/commandline/history.md)    | 显示镜像的历史记录 <br>*Show the history of an image*                                                  |
| [docker image](/docker-cli-docs/reference/commandline/image.md)      | 管理镜像 <br>*Manage images*                                                                 |
| [docker images](/docker-cli-docs/reference/commandline/images.md)     | 列出镜像 <br>*List images*                                                                   |
| [docker import](/docker-cli-docs/reference/commandline/import.md)     | 从tarball导入内容以创建文件系统镜像 <br>*Import the contents from a tarball to create a filesystem image*               |
| [docker info](/docker-cli-docs/reference/commandline/info.md)       | 显示系统范围的信息 <br>*Display system\-wide information*                                              |
| [docker inspect](/docker-cli-docs/reference/commandline/inspect.md)    | 返回Docker对象的低级信息 <br>*Return low\-level information on Docker objects*                               |
| [docker kill](/docker-cli-docs/reference/commandline/kill.md)       | 杀死一个或多个正在运行的容器 <br>*Kill one or more running containers*                                           |
| [docker load](/docker-cli-docs/reference/commandline/load.md)       | 从tar存档或STDIN加载图像 <br>*Load an image from a tar archive or STDIN*                                     |
| [docker login](/docker-cli-docs/reference/commandline/login.md)      | 登录到Docker注册表 <br>*Log in to a Docker registry*                                                   |
| [docker logout](/docker-cli-docs/reference/commandline/logout.md)     | 从Docker注册表注销 <br>*Log out from a Docker registry*                                                |
| [docker logs](/docker-cli-docs/reference/commandline/logs.md)       | 获取容器的日志 <br>*Fetch the logs of a container*                                                 |
| [docker manifest](/docker-cli-docs/reference/commandline/manifest.md)   | 管理Docker映像清单和清单列表 <br>*Manage Docker image manifests and manifest lists*                              |
| [docker network](/docker-cli-docs/reference/commandline/network.md)    | 管理网络 <br>*Manage networks*                                                               |
| [docker node](/docker-cli-docs/reference/commandline/node.md)       | 管理群集节点 <br>*Manage Swarm nodes*                                                            |
| [docker pause](/docker-cli-docs/reference/commandline/pause.md)      | 暂停一个或多个容器中的所有进程 <br>*Pause all processes within one or more containers*                             |
| [docker plugin](/docker-cli-docs/reference/commandline/plugin.md)     | 管理插件 <br>*Manage plugins*                                                                |
| [docker port](/docker-cli-docs/reference/commandline/port.md)       | 列出容器的端口映射或特定映射 <br>*List port mappings or a specific mapping for the container*                    |
| [docker ps](/docker-cli-docs/reference/commandline/ps.md)         | 列出容器 <br>*List containers*                                                               |
| [docker pull](/docker-cli-docs/reference/commandline/pull.md)       | 从注册表中提取图像或存储库 <br>*Pull an image or a repository from a registry*                                 |
| [docker push](/docker-cli-docs/reference/commandline/push.md)       | 将映像或存储库推送到注册表 <br>*Push an image or a repository to a registry*                                   |
| [docker rename](/docker-cli-docs/reference/commandline/rename.md)     | 重命名容器 <br>*Rename a container*                                                            |
| [docker restart](/docker-cli-docs/reference/commandline/restart.md)    | 重新启动一个或多个容器 <br>*Restart one or more containers*                                                |
| [docker rm](/docker-cli-docs/reference/commandline/rm.md)         | 移除一个或多个容器 <br>*Remove one or more containers*                                                 |
| [docker rmi](/docker-cli-docs/reference/commandline/rmi.md)        | 删除一个或多个图像 <br>*Remove one or more images*                                                     |
| [docker run](/docker-cli-docs/reference/commandline/run.md)        | 在新容器中运行命令 <br>*Run a command in a new container*                                              |
| [docker save](/docker-cli-docs/reference/commandline/save.md)       | 将一个或多个图像保存到tar存档（默认情况下流式传输到标准输出） <br>*Save one or more images to a tar archive \(streamed to STDOUT by default\)*    |
| [docker search](/docker-cli-docs/reference/commandline/search.md)     | 在Docker Hub中搜索图像 <br>*Search the Docker Hub for images*                                              |
| [docker secret](/docker-cli-docs/reference/commandline/secret.md)     | 管理Docker机密 <br>*Manage Docker secrets*                                                         |
| [docker service](/docker-cli-docs/reference/commandline/service.md)    | 管理服务 <br>*Manage services*                                                               |
| [docker stack](/docker-cli-docs/reference/commandline/stack.md)      | 管理Docker堆栈 <br>*Manage Docker stacks*                                                          |
| [docker start](/docker-cli-docs/reference/commandline/start.md)      | 启动一个或多个停止的容器 <br>*Start one or more stopped containers*                                          |
| [docker stats](/docker-cli-docs/reference/commandline/stats.md)      | 显示容器资源使用统计信息的实时流 <br>*Display a live stream of container\(s\) resource usage statistics*             |
| [docker stop](/docker-cli-docs/reference/commandline/stop.md)       | 停止一个或多个正在运行的容器 <br>*Stop one or more running containers*                                           |
| [docker swarm](/docker-cli-docs/reference/commandline/swarm.md)      | 管理群集 <br>*Manage Swarm*                                                                  |
| [docker system](/docker-cli-docs/reference/commandline/system.md)     | 管理Docker <br>*Manage Docker*                                                                 |
| [docker tag](/docker-cli-docs/reference/commandline/tag.md)        | 创建一个引用 SOURCE\_IMAGE 的标签 TARGET\_IMAGE <br>*Create a tag TARGET\_IMAGE that refers to SOURCE\_IMAGE*                       |
| [docker top](/docker-cli-docs/reference/commandline/top.md)        | 显示容器的运行进程 <br>*Display the running processes of a container*                                  |
| [docker trust](/docker-cli-docs/reference/commandline/trust.md)      | 管理对Docker映像的信任 <br>*Manage trust on Docker images*                                                 |
| [docker unpause](/docker-cli-docs/reference/commandline/unpause.md)    | 取消暂停一个或多个容器中的所有进程 <br>*Unpause all processes within one or more containers*                           |
| [docker update](/docker-cli-docs/reference/commandline/update.md)     | 更新一个或多个容器的配置 <br>*Update configuration of one or more containers*                                |
| [docker version](/docker-cli-docs/reference/commandline/version.md)    | 显示Docker版本信息 <br>*Show the Docker version information*                                           |
| [docker volume](/docker-cli-docs/reference/commandline/volume.md)     | 管理卷 <br>*Manage volumes*                                                                |
| [docker wait](/docker-cli-docs/reference/commandline/wait.md)       | 阻塞直到一个或多个容器停止，然后打印它们的退出代码 <br>*Block until one or more containers stop, then print their exit codes*          |

### `$ docker version`
Windows (Intel芯片)
```shell
$ docker version
Client: Docker Engine - Community
Cloud integration: 1.0.12
Version:           20.10.5
API version:       1.41
Go version:        go1.13.15
Git commit:        55c4c88
Built:             Tue Mar  2 20:14:53 2021
OS/Arch:           windows/amd64
Context:           default
Experimental:      true

Server: Docker Engine - Community
Engine:
Version:          20.10.5
API version:      1.41 (minimum version 1.12)
Go version:       go1.13.15
Git commit:       363e9a8
Built:            Tue Mar  2 20:15:47 2021
OS/Arch:          linux/amd64
Experimental:     false
containerd:
Version:          1.4.4
GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
runc:
Version:          1.0.0-rc93
GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
docker-init:
Version:          0.19.0
GitCommit:        de40ad0
```
Linux（Intel芯片）
```shell
￥ docker version
Client: Docker Engine - Community
Version:           20.10.0
API version:       1.41
Go version:        go1.13.15
Git commit:        7287ab3
Built:             Tue Dec  8 18:59:40 2020
OS/Arch:           linux/amd64
Context:           default
Experimental:      true

Server: Docker Engine - Community
Engine:
Version:          20.10.0
API version:      1.41 (minimum version 1.12)
Go version:       go1.13.15
Git commit:       eeddea2
Built:            Tue Dec  8 18:57:45 2020
OS/Arch:          linux/amd64
Experimental:     false
containerd:
Version:          1.4.3
GitCommit:        269548fa27e0089a8b8278fc4fc781d7f65a939b
runc:
Version:          1.0.0-rc92
GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19403a7aff
docker-init:
Version:          0.19.0
GitCommit:        de40ad0
```
Mac （Intel芯片）
```shell
$ docker version
Client: Docker Engine - Community
Cloud integration: 1.0.9
Version: 20.10.5
API version: 1.41
Go version: go1.13.15
Git commit: 55c4c88
Built: Tue Mar 2 20:13:00 2021
OS/Arch: darwin/amd64
Context: default
Experimental: true

Server: Docker Engine - Community
Engine:
Version: 20.10.5
API version: 1.41 (minimum version 1.12)
Go version: go1.13.15
Git commit: 363e9a8
Built: Tue Mar 2 20:15:47 2021
OS/Arch: linux/amd64
Experimental: false
containerd:
Version: 1.4.3
GitCommit: 269548fa27e0089a8b8278fc4fc781d7f65a939b
runc:
Version: 1.0.0-rc92
GitCommit: ff819c7e9184c13b7c2607fe6c30ae19403a7aff
docker-init:
Version: 0.19.0
GitCommit: de40ad0
```
## 5. DockerFile
### FROM
基础镜像的选择
```docker
FROM <镜像名>
```
例子：Build一个Nginx镜像
```docker
FROM nginx:1.21.0-alpine
ADD index.html /usr/share/nginx/html/index.html
```

### RUN
`RUN`主要用于在Image里执行指令，比如安装软件，下载文件等（在docker build时执行）
```docker
RUN <执行的命令>
```
例子：需要在docker中执行的如下命令
```shell
$ apt-get update
$ apt-get install wget
$ wget https://github.com/ipinfo/cli/releases/download/ipinfo-2.0.1/ipinfo_2.0.1_linux_amd64.tar.gz
$ tar zxf ipinfo_2.0.1_linux_amd64.tar.gz
$ mv ipinfo_2.0.1_linux_amd64 /usr/bin/ipinfo
$ rm -rf ipinfo_2.0.1_linux_amd64.tar.gz
```
所对应的dockerfile为：
```docker
FROM ubuntu:21.04
RUN apt-get update
RUN apt-get install -y wget
RUN wget https://github.com/ipinfo/cli/releases/download/ipinfo-2.0.1/ipinfo_2.0.1_linux_amd64.tar.gz
RUN tar zxf ipinfo_2.0.1_linux_amd64.tar.gz
RUN mv ipinfo_2.0.1_linux_amd64 /usr/bin/ipinfo
RUN rm -rf ipinfo_2.0.1_linux_amd64.tar.gz
```
改进版DockerFile：
```docker
FROM ubuntu:21.04
RUN apt-get update && \
    apt-get install -y wget && \
    wget https://github.com/ipinfo/cli/releases/download/ipinfo-2.0.1/ipinfo_2.0.1_linux_amd64.tar.gz && \
    tar zxf ipinfo_2.0.1_linux_amd64.tar.gz && \
    mv ipinfo_2.0.1_linux_amd64 /usr/bin/ipinfo && \
    rm -rf ipinfo_2.0.1_linux_amd64.tar.gz
```

### CMD
`CMD`可以用来设置容器启动时默认会执行的命令（在docker run时执行）
```docker
RUN <执行的命令>
```

### ADD
#### 1）复制普通文件
把local的一个文件复制到镜像里，如果目标目录不存在，则会自动创建
```docker
ADD <docker外位置> <docker内位置>
```
例子：将index.html增加到nginx
index.html:
```html
<h1>Hello Docker</h1>
```
DockerFile：
```docker
FROM nginx:1.21.0-alpine
ADD index.html /usr/share/nginx/html/index.html
```
#### 2）复制压缩文件
`ADD`比`COPY`高级一点的地方就是，如果复制的是一个gzip等压缩文件时，ADD会帮助我们自动去解压缩文件。
```docker
ADD <docker外的压缩文件位置> <docker内解压位置>
```
例子：解压hello.tar.gz到/app/
```docker
FROM python:3.9.5-alpine3.13
ADD hello.tar.gz /app/
```

### COPY
把local的一个文件复制到镜像里，如果目标目录不存在，则会自动创建
```docker
COPY <docker外位置> <docker内位置>
```
例子：将index.html增加到nginx
index.html:
```html
<h1>Hello Docker</h1>
```
DockerFile：
```docker
FROM nginx:1.21.0-alpine
COPY index.html /usr/share/nginx/html/index.html
```