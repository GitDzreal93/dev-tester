# Docker 使用

### 安装Docker

#### mac下安装

```shell
brew cask install docker
```

启动docker：进入launchpad，双击docker图标即可启动

#### Linux下安装

> https://yeasy.gitbooks.io/docker_practice/install/centos.html （CentOS）
>
> https://yeasy.gitbooks.io/docker_practice/install/ubuntu.html （Ubuntu）

#### Windows下安装

> https://yeasy.gitbooks.io/docker_practice/install/windows.html

### Docker 概念

镜像：一个特殊的文件系统，提供容器运行所需的程序、库、资源等。可以把它看作一个类。

容器：容器的实质是进程，每个进程最好占用一个容器，比如 jenkins，wiki，jira，gitlab单独放在不同容器中。可以把它看作是镜像的一个实例。 

仓库：相当于git的远程仓库，有公开的，也有私有的，表现形式是 <仓库名>:<标签>，每个标签即为一个远程镜像。

网络：Docker 允许通过外部访问容器或容器互联的方式来提供网络服务。有多种网络模式。

数据管理：分为**数据卷**（`volume`）和**挂载主机**两种方式

1. 数据卷，是一个可供一个或多个容器使用的特殊目录，通过`docker volume create vol_name` 创建数据卷
2. 挂载主机，是挂载一个主机目录作为数据卷，docker容器启动时，通过 `--mount` 参数去实现

### Docker 常用命令

#### 启动docker

安装docker的时候已经提到如何启动docker

#### 镜像操作

##### 查找镜像

`docker search <镜像名>`  再官方仓库查找镜像

##### 获得镜像

`docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]` 

举例：

```shell
# docker方式获取jenkins
docker pull jenkins/jenkins:lts
```

##### 运行镜像（启动容器）

`docker run [选项] IMAGE [命令][参数..]`

举例：

```shell
# 启动jenkins
docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts

# -d ：--detach 后台运行容器
# -v ：--volume 绑定挂载目录
# -p : 绑定端口，主机端口:容器端口 8080是web端口，50000是从属服务器使用端口
```

##### 列出镜像

`docker image ls`  列出全部镜像

`docker image ls <镜像名>`  列出部分镜像

```shell
# 列表包含了 仓库名、标签、镜像 ID、创建时间 以及 所占用的空间。
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
b3log/solo          latest              deba8aac9495        4 days ago          150MB
```

##### 镜像体积

`docker system df`

##### 虚悬镜像

**仓库名**和**标签**都为 `<none>`的镜像

`docker image ls -f dangling=true`  查看虚悬镜像

`docker image prune`  删除虚悬镜像

##### 删除本地镜像

`docker image rm [选项] <镜像1> [<镜像2> ...]` 删除本地镜像

`docker image rm $(docker image ls -q image_name)` 成批删除镜像，用在某个镜像可能有不同tag，但想全部删除的情况

##### 保存镜像（慎用）

`docker commit [选项] <容器ID或容器名> [<仓库名>[:<标签>]]`

##### Dockerfile

作用：定制镜像

构建镜像：`docker build [上下文路径/URL/-]`，（`docker build -t <镜像名> -f <DOckerfile路径> .` 是常用命令）

Dockerfile 常用指令：

`FROM <镜像名>`   指定基础镜像

`RUN <命令>`  执行命令

```shell
# 举个例子
# stretch 是空白镜像
FROM debian:stretch

# RUN最多是42层，所以不要滥用RUN，一个RUN一般是一个步骤，比如下方的编译、安装 redis 可执行文件可以放在一个步骤里执行，用 && 可以串联命令
RUN buildDeps='gcc libc6-dev make wget' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps
```

`COPY [--chown=<user>:<group>] <源路径>... <目标路径>` 复制文件

- 源路径可以有多个，甚至可以用通配符
- 源路径是相对路径
- 目标路径可以是绝对路径，也可以是相对于工作目录的相对路径

`ADD [--chown=<user>:<group>] <源路径>... <目标路径>` 比COPY更高级的复制文件

- 源路径可以是URL、压缩文件（会自动解压到目标路径）

`CMD` 容器启动命令

- `shell` 格式: `CMD <命令>`
- `exec` 格式: `CMD ["可执行文件", "参数1", "参数2"...]`
- 参数列表格式：`CMD ["参数1", "参数2"...]`。指定`ENTRYPOINT`指令后，用`CMD`指定具体参数
- 注意：`CMD` 的命令**不能**是后台运行的命令！例如，不能是 `CMD service nginx start`
- 注意：`CMD` 推荐使用`exec`格式编写
- 注意：一个Dockerfile只能有一个`CMD`指令

`ENTRYPOINT` 入口点

> 详细参考：https://yeasy.gitbooks.io/docker_practice/image/dockerfile/entrypoint.html

- `ENTRYPOINT  "CMD"`  或 `ENTRYPOINT ["可执行文件", "参数1", "参数2"...]`
- 注意：一个Dockerfile只能有一个`ENTRYPOINT`指令

- 应用场景：
  - 运行docker容器（`docker run`）时，可以把镜像当命令一样使用
  - 应用在运行前的准备工作

`ENV` 设置环境变量

- `ENV <key> <value>`
- `ENV <key1>=<value1> <key2>=<value2>...`

使用环境变量 `$<key>`  

`ARG` 构建参数

- 格式：`ARG <参数名>[=<默认值>]` 
- 作用也是设置环境变量，但是在容器运行时不会存在这些环境变量

`VOLUME` 定义匿名数据卷

- `VOLUME ["<路径1>", "<路径2>"...]`
- `VOLUME <路径>`

`EXPOSE` 声明端口

- `EXPOSE <端口1> [<端口2>...]` 只是声明打算使用什么端口，但实际不会自动在宿主进行端口映射

`WORKDIR` 指定工作目录

- 用法：`WORKDIR <工作目录路径>`

`USER` 指定当前用户

- `USER <用户名> [:<用户组>]`
- 用户必须是事先简历好的，否则无法切换
- 不要用`su`或`sudo`，应该用`gosu`

`HEALTHCHECK` 健康检查

- `HEALTHCHECK [选项] CMD <命令>` 设置检查容器健康状况的命令
- `HEALTHCHECK NONE` 如果基础镜像有健康检查指令，使用这个命令屏蔽掉健康检查指令
- 注意：一个Dockerfile只能有一个`HEALTHCHECK`指令
- 参数可选
  - `--interval=<间隔>`：两次健康检查的间隔，默认为30秒；
  - `--timeout=<时长>`: 健康检查命令运行超时时间，运行超时则本次健康检查视为失败
  - `--retries=<次数>`：健康检查次数，默认为3次

#### 容器操作

##### 启停容器

`docker run -d` 后台启动容器

`docker container start <容器名/ID>` 启动一个已终止的容器

`docker container stop <容器名/ID>` 停止一个正运行的容器

`docker container restart <容器名/ID>` 重启一个正运行的容器

##### 查看容器的具体信息

`docker inspect <容器名>` 查看容器的具体信息

##### 查看容器运行状态

`docker container ls` 查看**正运行**的容器运行状态

`docker ps` 列出正在运行的容器

`docker container ls -a`  查看所有的容器的运行状态

`docker ps -a`  列出所有容器

`docker logs -f 容器名称` 查看容器应用程序运行日志

##### 进入容器

`docker attach <容器名/ID>`  进入容器，但退出容器时，会导致容器停止（不建议用这种方式）

`docker exec -it <容器名/ID> /bin/sh` 进入容器，退出容器时，不会导致容器停止（推荐）

##### 复制文件

`docker cp 主机源文件路径 <容器名>:容器目的文件路径` 往容器里导入文件

`docker cp <容器名>:容器的源文件路径 主机目的文件路径` 把文件从容器里导出

##### 导入/导出容器快照

`docker export <容器名/ID> > <xxx.tar>` 导出容器快照

`cat xxx.tar | docker import  - <镜像:tag>` 从快照文件中再导入为镜像

##### 删除容器

`docker container rm <终止的容器>` 删除终止状态的容器

`docker container rm -f <运行的容器>` 删除运行状态的容器

`docker container prune` 删除所有处于终止状态的容器

### 数据卷操作

##### 创建数据卷

`docker volume create <数据卷名>`

##### 查看数据卷

`docker volume ls`

##### 挂载数据卷

```
docker run -d -P \
    --name web \
    # -v my-vol:/webapp \
    --mount source=my-vol, target=/webapp \
    training/webapp \
    python app.py
```

##### 删除数据卷

`docker volume rm my-vol`

`docker volume prune` 删除无主的数据卷

### 网络

##### 指定端口运行容器

```shell
docker run --name 镜像 \
   -p 8090:8080 \  # 宿主机端口8090，容器端口8080
```

##### 查看端口映射

`docker port <容器名>`

### 其他

`docker-compose` ：负责快速部署分布式应用

> https://yeasy.gitbooks.io/docker_practice/compose/

`docker machine`：快速安装 Docker 环境

> https://yeasy.gitbooks.io/docker_practice/machine/

### 实际案例

利用solo搭建属于自己的博客网站

> solo github：https://github.com/b3log/solo

环境：linux CentOS7

**step1**

安装 docker ，移步本文开头安装 docker

**step2**

手动创建数据库（mysql）

我是用宝塔Linux面板创建的，十分方便，创建数据库非常简单，大家也可以用其他方式创建，比如用sql创建

```mysql
mysql> create database solo;
```

**step3**

docker拉取solo镜像

```shell
docker pull b3log/solo
```

**step4**

在终端执行以下命令

```shell
# qa.tencentgg.cn 端口号9090
docker run --detach --name solo --network=host \
    --env RUNTIME_DB="MYSQL" \
    --env JDBC_USERNAME="user" \
    --env JDBC_PASSWORD="password" \
    --env JDBC_DRIVER="com.mysql.cj.jdbc.Driver" \
    --env JDBC_URL="jdbc:mysql://127.0.0.1:3306/solo?useUnicode=yes&characterEncoding=UTF-8&useSSL=false&serverTimezone=UTC" \
    b3log/solo --listen_port=9090 --server_scheme=http --server_host=qa.tencentgg.cn
```

注意的点：

`--name` : 容器名（随意填，默认solo也行）

`--env`：配置环境变量

- JDBC_USERNAME  填你创建数据库的用户名

- JDBC_PASSWORD  填你创建数据库的密码

`--network=host`：网络采用和主机互通的网络

最后一行是 solo 启动命令，具体参数可以查看 solo/src/main/java/org/b3log/solo/Starter.java

注意的是：

`--server_host` 配置你浏览器访问该网站的域名，假如你用域名访问，就配置域名；假如你用IP访问，就配置IP

**step5**

nginx配置

**首先确保你的域名是可解析的**！我的服务器是腾讯云的服务器，域名也是腾讯云买的，腾讯云有专门的域名解析服务

 ```nginx
upstream backend {
    server localhost:9090; # Solo 监听端口
}

server {
    listen       80;
    server_name  qa.tencentgg.cn; # 博客域名

    access_log off;

    location / {
        proxy_pass http://backend$request_uri;
        proxy_set_header  Host $host:$server_port;
        proxy_set_header  X-Real-IP  $remote_addr;
        client_max_body_size  10m;
    }
}
 ```

nginx 配置检测

`nginx -t` 假如显示 syntax is ok 及 test is successful，说明nginx配置没问题

nginx 重启

`nginx -s reload` 重启nginx

 **step6**

浏览器地址栏访问：http://qa.tencentgg.cn

