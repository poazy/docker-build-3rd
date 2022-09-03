
* 运行命令
```shell
docker run -d --restart=always --name sentinel-dashboard-185 \
  -p 8858:8080 \
  -e JAVA_OPTS="-Duser.timezone=Asia/Shanghai -Dfile.encoding=UTF-8" \
  poazy/sentinel-dashboard:1.8.5
```

* 打包命令
```shell
sudo docker buildx build -t poazy/sentinel-dashboard:1.8.5 \
  --build-arg FROM_IMAGE="openjdk:8-jre-alpine" \
  --build-arg APP_NAME="sentinel-dashboard-1.8.5" \
  --no-cache --platform linux/arm64,linux/amd64 . --push
```

* 前戏命令
```shell
sudo docker buildx create --use --name m1_builder

sudo docker buildx inspect --bootstrap

sudo docker login -u poazy -p xxx
```