基于docker环境建立的node开发环境

自定义镜像：

* [Docker hub => php-node-image](https://hub.docker.com/r/ahaschool/docker-node-dev/php-node-image/)

里面已经包含了开发所需的php,nodejs,nginx,yarn,npm。

### 前置条件
* 安装好docker和docker-compose

### 目录简介
|目录|简介|
| ----- | ----- |
|app|存放代码的目录|
|php-node-image|自定义镜像Dockerfile文件|
|run|docker-compose 的运行目录|
|run/etc|容器运行的配置文件|

		
### 安装

* 获取代码，运行docker-compose：

```bash
git clone git@github.com:ahaschool/docker-node-dev.git
```

* 构建自定义镜像

```bash
cd php-node-image
docker build -t mktnode . # mktnode可以修改为自己喜欢的名字（修改后记得修改docker-compose.yml）
```

* 启动容器环境

```bash
cd run/
docker-compose up -d
```

* 添加网卡别名(区分本地回环地址127.0.0.1，可选)

```bash
sudo -S ifconfig en0 alias 10.254.254.254 255.255.255.0
```

* 并且虚拟域名绑定

```bash
sudo vi /etc/hosts
"127.0.0.1   <dir>.dist.local"
# <dir> 表示自己定义的域名
```

### 域名规则

* 我们在nginx的配置文件中使用的正则匹配 ```~^(?<sub>.+)\.dist.local```
* 所有已dist.local访问到本容器服务的时候，自动匹配到```/app/web/```下面对应的目录
