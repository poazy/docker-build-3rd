* 01下载源代码切换版本Tag
```shell
#下载源代码
git clone https://github.com/infiniflow/ragflow.git
#切换到下载的ragflow目录
cd ragflow/
#切换版本Tag
git switch --detach v0.13.0
```

* 02安装与下载依赖
```shell
#要提前安装好python哦!
pip3 install huggingface-hub nltk
#需要科学上网，下载依赖文件这个过程会很久
python3 download_deps.py
```

* 03构建镜像（构建arm64架构，与【构建多平台架构】二选一方式构建）
```shell
#需要在daemon.json中配置"registry-mirrors": ["https://docker.m.daocloud.io"] 或 科学上网
sudo docker build --build-arg ARCH="arm64" -f Dockerfile -t local.dsmm.com:1667/dsmm/ragflow:0.13.0 .
```

* 03构建镜像（构建多平台架构，与【构建arm64架构】二选一方式构建）
```shell
#构建多平台架构
#需要在daemon.json中配置"registry-mirrors": ["https://docker.m.daocloud.io"] 或 科学上网
sudo docker buildx create --use --name m1_builder
sudo docker buildx inspect --bootstrap
#
#先Dockerfile修改下dpkg -i /root/libssl1.1_1.1.1f-1ubuntu2_amd64.deb; \的fi后添加 || true避免在arm64时执行异常！
sudo DOCKER_BUILDKIT=1 docker build -f Dockerfile \
  -t local.dsmm.com:1667/dsmm/ragflow:0.13.0 \
  --no-cache --platform linux/arm64,linux/amd64 .
```

* 04运行镜像
```shell
#运行镜像后检查所有容器状态是否正常
#sed -i "" 's/RAGFLOW_IMAGE=infiniflow\/ragflow:dev-slim/RAGFLOW_IMAGE=local.dsmm.com:1667\/dsmm\/ragflow:0.13.0/g' docker/.env
docker compose -f docker/docker-compose.yml up -d
```

* 05推送镜像到Harbor私服
```shell
# 需要在daemon.json中配置"insecure-registries": ["http://local.dsmm.com:1667"]
sudo docker login http://local.dsmm.com:1667 -u admin
docker push local.dsmm.com:1667/dsmm/sentinel-dashboard:1.8.7
```

* 06其他说明
1.修改daemon.json和config.json文件都要重新启动docker
2.MacOS的daemon.json和config.json在~/.docker目录下
3.用docker buildx build命令就算科学上网了pull和push都会timeout失败，改用DOCKER_BUILDKIT=1 docker build就可以 + 再 docker push 