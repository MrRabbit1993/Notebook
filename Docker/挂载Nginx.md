# docker 部署Nginx并且做宿主机挂载

## 1、宿主机创建挂载路径

```sh
mkdir /data/nginx/conf
mkdir /data/nginx/html
mkdir /data/nginx/logs
```

## 2、启动一个临时nginx镜像,便于复制Nginx配置文件
```sh
docker run --name nginx_test -d -p 80:80 nginx
```
## 3、复制nginx的配置文件到宿主机
```sh
docker cp nginx_test:/etc/nginx/conf.d /data/nginx/conf/
docker cp nginx_test:/etc/nginx/nginx.conf /data/nginx/
docker cp nginx_test:/usr/share/nginx/html/ /data/nginx/
```
## 4、移除容器
```sh
docker rm -f nginx_test
```
## 5、创建真实想要的容器并挂载
```sh
docker run --name nginx -p 80:80 -p 9000:9000 -v /data/nginx/conf/conf.d:/etc/nginx/conf.d -v /data/nginx/nginx.conf:/etc/nginx/nginx.conf -v /data/nginx/html:/usr/share/nginx/html -v /data/nginx/logs/:/var/log/nginx/ -d  nginx
```
## 6、后续在宿主机增加端口，需要重启容器
```sh
docker restart xxxx
```