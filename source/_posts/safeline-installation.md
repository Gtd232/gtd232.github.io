---
title: 安装雷池（Safeline）在 Alpine Linux
date: 2021-01-07 18:00:00
categories:
  - Category 1
tags:
  - Tag 1
---

参考了<https://waf-ce.chaitin.cn/docs/guide/install#%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85>

# 配置环境

1.`apk add docker docker-compose wget`
2.`rc-update add docker boot`
3.`rc-service docker start`

# 安装雷池

1. `cd ~`
2. `wget https://demo.waf-ce.chaitin.cn/image.tar.gz`(海外服务器可以现在国内下载，然后上传到海外服务器)
3. `cat image.tar.gz | gzip -d | docker load` 稍等片刻
4. `mkdir -p safeline  &&  cd safeline    # 创建 safeline 目录并且进入`
5. `wget https://waf-ce.chaitin.cn/release/latest/compose.yaml`
6. 直接执行:
```
cat >> .env <<EOF
SAFELINE_DIR=$(pwd)
IMAGE_TAG=latest
MGT_PORT=9443
POSTGRES_PASSWORD=$(LC_ALL=C tr -dc A-Za-z0-9 </dev/urandom | head -c 32)
SUBNET_PREFIX=172.22.222
IMAGE_PREFIX=chaitin
EOF
```
7. `docker-compose up -d` 启动
