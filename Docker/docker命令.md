## 搜素镜像

docker search nginx

## 拉取镜像

docker pull xxx

docker pull nginx

## 查看镜像

docker images

## 启动容器实例

docker run --rm --name nginx -p 8080:80 -d nginx

 --rm：容器终止运行后，自动删除容器文件。

--name nginx：容器的名字叫做nginx,名字自己定义.

-p: 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口

-d：容器启动后，在后台运行

## 查看启动的docker容器

 docker ps

## 查看docker容器

 docker ps -a

## host 主机和container之间拷贝

docker cp

docker cp 358354f206fd:/etc/nginx/nginx.conf /home/nginx/conf/

## host主机与container映射

docker run --rm -d -p 8888:80 --name nginx -v D:/dockerVirtual/nginx/html:/usr/share/nginx/html -v D:/dockerVirtual/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v D:/dockerVirtual/nginx/logs:/var/log/nginx nginx

## 删除镜像/实例

docker rm 比如 docker rm nginx 、docker rm 20a

docker rmi 123 //通过镜像Id删除镜像

## 重启容器

docker start

docker start 12d

## 停止容器

docker stop

docker stop 12d

## 进入容器

docker exec

-d :分离模式: 在后台运行

-i :即使没有附加也保持STDIN 打开

-t :分配一个伪终端

--user root 用root进入

docker exec -it 12d /bin/bash

## 构建镜像
docker build -t vuedocker .

-t 是给镜像命名，. 是基于当前目录的 Dockerfile 来构建镜像。 镜像名称小写

## 快速查找某一个镜像
docker image ls | grep vuedocker