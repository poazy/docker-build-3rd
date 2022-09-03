
* 运行命令
```shell
# --net=host \
# -p 27848:7848 -p 28848:8848 -p 29848:9848 \
docker run -d --restart=always --name nacos211 \
    --net=host \
    -v /poazy/data/dockerdata/nacos211/logs:/home/nacos/logs \
    -v /poazy/data/dockerdata/nacos211/init.d/custom.properties:/home/nacos/init.d/custom.properties \
    -v /poazy/data/dockerdata/nacos211/plugins/mysql:/home/nacos/plugins/mysql \
    -e "MODE=standalone" \
    -e "PREFER_HOST_MODE=ip" -e "NACOS_SERVER_IP=192.168.0.95" \
    -e "SPRING_DATASOURCE_PLATFORM=mysql" \
    -e "MYSQL_SERVICE_HOST=192.168.0.95" \
    -e "MYSQL_SERVICE_PORT=3306" \
    -e "MYSQL_SERVICE_USER=XXX" \
    -e "MYSQL_SERVICE_PASSWORD=XXX" \
    -e "MYSQL_SERVICE_DB_NAME=XXX" \
    -e "MYSQL_SERVICE_DB_PARAM=characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useSSL=false" \
    -e "TIME_ZONE=Asia/Shanghai" -e "TZ=Asia/Shanghai" \
    poazy/nacos-server:v2.1.1
```

* 打包命令
```shell
sudo docker buildx build -t poazy/nacos-server:v2.1.1 \
  --build-arg NACOS_VERSION="2.1.1" \
  --no-cache --platform linux/arm64,linux/amd64 . --push
```

* 前戏命令
```shell
sudo docker buildx create --use --name m1_builder

sudo docker buildx inspect --bootstrap

sudo docker login -u poazy -p xxx
```