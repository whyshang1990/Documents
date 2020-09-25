# Docker

## Image文件

Docker 把应用程序及其依赖打包在一个image文件里面，可以理解为一个container的说明书。
通过image文件能够生成container容器的实例，同一个image文件可以同时生成多个container实例。
image文件制作完成后，可以共用，一般初识冲仓库pull image文件使用

常用命令

```bash
# 同时列出本机所有image文件
docker images
# 搜索images
docker search [image-name]
# 从默认仓库拉取image文件到本地
# 拉取最新版本的ubuntu image文件，默认使用TAG为latest
docker pull ubuntu
```

## container
image文件生成的实例本身也是一个文件,称为container文件.

```bash
# 从一个image文件中生成一个container实例，并且运行它
docker container run ubuntu
# 当容器不自动终止时,使用命令终止
docker container kill [container-id]
# 查看当前运行container
docker container ls
# 运行ubuntu容器并进入后台交互  
# --name percy 指定名称
# -p 80:8080 参数将 Docker 主机的端口(80)映射到容器的端口(8080)。
# -d 后台运行容器，并返回容器id
# -v 设置容器目录，将本地目录映射到容器  [local_dir]:[container_dir]
# 使用2次快捷键 ctrl+p ctrl+q 退出交互界面
# windows git bash 需要管理员权限，命令前加winpty
docker container run -ti [container-id or container-name] bash 

# 进入一个已经运行的容器
docker container exec -it [container-id or container-name] bash
# 删除容器(使用name)，先stop，再rm
docker container stop [container-id or container-name]
docker container rm [container-id or container-name]
```

## inspect 命令

用于查看image，container详细信息

```bash
# 查看指定image详细信息
docker inspect [image_name or container-id or container-name] | grep [property_name]
```

## network 命令

```bash
# 展示network
docker network ls
# 创建一个网络
docker network create --driver bridge --subnet=172.18.12.0/16 --gateway=172.18.1.1 mynet

```

## 文件操作命令

### cp

```bash
# 复制容器内文件到宿主机
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
# 复制宿主机内文件到容器
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
```

## 导出镜像文件

```bash
# 将当前container创建为一个image
# -m，--message 备注， REPOSITORY: ubuntu-java, TAG:v1.0.0
docker commit -m "ubuntu安装java" [container-id] ubuntu-java:v1.0.0
docker commit -m "ubuntu安装java" [container-id] [REPOSITORY]:[TAG]
# 将image，container 导出为文件
# -o，--output 导出文件路径
docker save --output /e/docker/ubuntu_java.tar ubuntu-java:v1.0.0
```

## 安装mariadb

1 docker search mariadb 搜索mariadb镜像（非必须）

2 docker pull mariadb 下载docker镜像

3 docker images 查看本地已有的所有镜像

4 mkdir -p /data/mariadb/data 建一个目录作为和容器的映射目录

5 docker run --name mariadb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=输入数据库root用户的密码 -v /data/mariadb/data:/var/lib/mysql -d mariadb

　　--name启动容器设置容器名称为mariadb

　　-p设置容器的3306端口映射到主机3306端口

　　-e MYSQL_ROOT_PASSWORD设置环境变量数据库root用户密码为输入数据库root用户的密码

　　-v设置容器目录/var/lib/mysql映射到本地目录/data/mariadb/data

　　-d后台运行容器mariadb并返回容器id

6 docker ps -a 查看容器是否运行

7 docker container update --restart=always 容器id   修改容器为自启动

8 进入容器docker exec -it 容器Id bash

9 在容器内登录数据库 mysql -uroot -proot密码

其他常用命令：

docker start 容器id　　启动容器

docker stop 容器id　　停止容器

10 使用navicat连接数据库
查看本机docker主机ip， 使用ipconfig命令，端口设置为本机映射端口

## docker创建container语句备份

```bash
# 创建网桥
docker network create --driver bridge --subnet=172.18.12.0/16 --gateway=172.18.1.1 mynet

# 创建容器
docker container run --name customapp --network mynet --ip 172.18.12.20 -p 11106:11106 -ti ubuntu-java11:1.0.0 bash

# 创建ubuntu容器
docker run --name customapp -p 11106:11106 -ti ubuntu bash

# 创建mariadb数据库容器
docker run --name maria_database -p 13306:3306 -e MYSQL_ROOT_PASSWORD=wuhaiyun -v e:/docker/maria_database -d mariadb
```
docker run --name mariadb --network mynet --ip 172.18.12.30 -p 13306:3306 -e MYSQL_ROOT_PASSWORD=wuhaiyun -v /Users/why/code/docker -d mariadb