* 01多平台架构支持
```shell
sudo docker buildx create --use --name m1_builder
#ERROR: error getting credentials - err: exit status 1, out: ``需要将config.json中的"credsStore": "desktop"删除
#需要在daemon.json中配置"registry-mirrors": ["https://docker.m.daocloud.io"] 或 科学上网
sudo docker buildx inspect --bootstrap
```
* 02构建镜像
```shell
sudo DOCKER_BUILDKIT=1 docker build -t poazy/sentinel-dashboard:1.8.7 \
  --build-arg FROM_IMAGE="openjdk:8-jre-alpine" \
  --build-arg APP_NAME="sentinel-dashboard-1.8.7" \
  --build-arg CURR_TIME="$(date +'%Y%m%d%H%M%S')" \
  --no-cache --platform linux/arm64,linux/amd64 .
```

* 03运行镜像
```shell
docker run -d --restart=always --name sentinel-dashboard-187 \
  -p 8858:8080 \
  -e JAVA_OPTS="-Duser.timezone=Asia/Shanghai -Dfile.encoding=UTF-8" \
  poazy/sentinel-dashboard:1.8.7
```

* 04推送镜像到hub.docker.com
```shell
# 需要科学上网
# 如果登录login报错Error saving credentials需要将config.json中的"credsStore": "desktop"删除
sudo docker login -u poazy
# 需要科学上网
docker push poazy/sentinel-dashboard:1.8.7
```

* 04推送镜像到Harbor私服
```shell
docker tag b61d921bdd0b local.dsxx.com:11667/dsxx/sentinel-dashboard:1.8.7
# Error response from daemon: Get "https://local.dsmm.com:1667/v2/": http: server gave HTTP response to HTTPS client
# 需要在daemon.json中配置"insecure-registries": ["http://local.dsxx.com:11667"]
sudo docker login http://local.dsxx.com:11667 -u admin
docker push local.dsmm.com:1667/dsmm/sentinel-dashboard:1.8.7
```

* 05其他说明
1.修改daemon.json和config.json文件都要重新启动docker
2.MacOS的daemon.json和config.json在~/.docker目录下
3.用docker buildx build命令就算科学上网了pull和push都会timeout失败，改用DOCKER_BUILDKIT=1 docker build就可以 + 再 docker push 