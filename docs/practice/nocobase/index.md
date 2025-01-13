# NocoBase 使用指南

本文档介绍了如何使用 ComputeNest CLI 创建 NocoBase 服务的具体步骤。

## 案例简介

NocoBase 是一个极易扩展的开源无代码开发平台。可以在 GitHub 仓库中找到其源码：[NocoBase GitHub 仓库](https://github.com/nocobase/nocobase)。ComputeNest CLI 支持快速生成基于 Docker Compose 或 Dockerfile 的计算巢服务配置，并基于该配置快速创建服务。

下面将以 NocoBase 的 Docker Compose 安装模式为例，介绍如何使用 ComputeNest CLI 快速创建 NocoBase 服务。

## 操作步骤

### 1. 安装 ComputeNest CLI

首先，使用 pip 安装 ComputeNest CLI：

```bash
pip install computenest-cli
```

### 2. 克隆仓库到本地

接下来，将 NocoBase 源码仓库克隆到本地：

```bash
git clone https://github.com/nocobase/nocobase.git
cd nocobase
```

### 3. 生成计算巢配置文件

使用 `generate nocobase-docker-compose` 命令自动生成计算巢服务配置文件。生成的文件可以在 `.computenest` 目录中查看：

```bash
computenest-cli generate nocobase-docker-compose
```

### 4. 创建计算巢服务

最后，使用生成的服务配置文件创建计算巢服务：

```bash
computenest-cli import --service_name=nocobase-docker-compose-test
```

---

通过上述步骤，即可成功创建并运行 NocoBase 服务。如果在操作过程中遇到问题，请参考 ComputeNest CLI 或 NocoBase 的相关文档获取更多信息。

### 注意事项

- 确保你的环境已正确安装并配置了 Docker 和其他相关依赖。
- 在执行命令时，请确保路径和文件名的准确性。
- 若要更改配置，修改 `.computenest` 目录中的文件后重新导入服务。
