# Quickstart

本文介绍如何快速入门计算巢命令行工具。

## 要求

- Python >= 3.7

## 安装

```shell
# 安装 computenest-cli
pip install computenest-cli
```

## 采用官方模板创建服务

```shell
# 1. 查看官方维护的项目
computenest-cli list-projects

# 2. 初始化项目,以'SpringBoot单机版-软件包部署'为例
cd /path/to/project  # 指定项目目录
computenest-cli init-project --project_name=springboot-ecs-package-demo 
cd springboot-ecs-package-demo

# 3. 用个人AccessKey登录计算巢
computenest-cli login --access_key_id=xxxxxx --access_key_secret=xxxxxx

# 4. 创建服务
computenest-cli import --service_name=springboot-ecs-package-demo-test
```

创建服务后，可以登录[计算巢控制台](https://computenest.console.aliyun.com/service/cn-hangzhou)查看已创建的服务详细信息。

如何获取 AccessKey 对： https://help.aliyun.com/zh/id-verification/cloudauth/obtain-an-accesskey-pair

## 卸载
```shell
pip uninstall computenest-cli -y
```