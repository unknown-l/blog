---
title: centos 安装 docker
date: 2020-03-06 11:38:22
tags:
---
# centos

## 1. 卸载旧版本

   ```bash
   $ sudo yum remove docker \
                     docker-client \
                     docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

## 2. 安装依赖

   ```bash
   $ sudo yum install -y yum-utils \
     device-mapper-persistent-data \
     lvm2
   ```

## 3. 设置镜像源

   ```bash
   # 官方镜像源
   $ sudo yum-config-manager \
       --add-repo \
       https://download.docker.com/linux/centos/docker-ce.repo
   # 国内镜像源
   $ sudo yum-config-manager \
       --add-repo \
       http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   ```
## 4. 列出docker版本
   ```bash
   $ yum list docker-ce --showduplicates | sort -r
   ```
## 5. 安装
   ```bash
   $ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
   ```
## 6. 启动docker
   ```bash
   # 开机自启
   $ sudo systemctl enable docker
   # 启动
   $ sudo systemctl start docker
   ```
## 7. 非root用户需将当前用户加入docker组
   ```bash
   $ sudo usermod -aG docker $USER
   ```
## 8. 配置镜像加速
   ```bash
   # 编辑配置文件
   $ vim /etc/docker/daemon.json
   # registry-mirrors 镜像加速
   # insecure-registries 私有http仓库
   ```
   ```json
   {
       "insecure-registries": [
            "xxx.xxx.xxx.xxx:xxxx"
        ],
        "registry-mirrors": [
            "https://registry.docker-cn.com"
        ]
   }
   ```
   ```bash
   # 重启服务
   $ sudo systemctl daemon-reload
   $ sudo systemctl restart docker
   ```
## 9. 安装docker compose
   ```bash
   # 安装
   $ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   # 授予执行权限
   $ sudo chmod +x /usr/local/bin/docker-compose
   ```
## 10. swarm mode
   ```bash
   # 初始化swarm
   $ docker swarm init
   # 查看加入命令
   $ docker swarm join-token worker
   # 修改worker节点远程访问
   $ vim /usr/lib/systemd/system/docker.service
   ```
## 11. /usr/lib/systemd/system/docker.service
   ```bash
   # ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://{本机ip || 内网ip || 0.0.0.0}:2375
   # 重启
   ```