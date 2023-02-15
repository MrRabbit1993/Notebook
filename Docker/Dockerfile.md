
```sh
# 设置镜像源

FROM nginx

# 设置作者

# MAINTAINER MrRabbit 老版本

LABEL maintainer="MrRabbit"

# 将文件复制到指定目录下

COPY dist/ /usr/share/nginx/project/vue

```