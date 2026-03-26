---
title: Docker 入门：用容器化思维开发应用
date: 2026-03-10 11:00:00
categories:
  - 后端
tags:
  - Docker
  - 容器化
  - DevOps
cover: https://images.unsplash.com/photo-1605745341112-85968b19335b?w=800&q=80
---

"在我机器上能跑啊"——这句话让多少团队抓狂。Docker 的出现就是为了彻底解决这个问题。

<!-- more -->

## Docker 是什么？

Docker 是一个容器化平台，它将应用及其所有依赖打包成一个**镜像（Image）**，然后在任何地方以**容器（Container）**的形式运行，保证环境完全一致。

```
你的代码 + 运行环境 + 依赖 = Docker 镜像
镜像运行起来 = 容器
```

## 核心概念

| 概念 | 说明 |
|------|------|
| **Image** | 只读模板，包含运行应用所需的一切 |
| **Container** | Image 的运行实例 |
| **Dockerfile** | 构建 Image 的脚本 |
| **Registry** | 存储 Image 的仓库（如 Docker Hub） |

## 安装 Docker

前往 [Docker 官网](https://www.docker.com/) 下载 Docker Desktop，支持 Mac / Windows / Linux。

## 基础命令

```bash
# 拉取镜像
docker pull nginx

# 查看本地镜像
docker images

# 运行容器
docker run -d -p 8080:80 --name my-nginx nginx

# 查看运行中的容器
docker ps

# 停止/删除容器
docker stop my-nginx
docker rm my-nginx
```

## 编写 Dockerfile

以一个 Node.js 应用为例：

```dockerfile
# 基础镜像
FROM node:18-alpine

# 设置工作目录
WORKDIR /app

# 复制依赖文件（利用层缓存）
COPY package*.json ./
RUN npm install --registry https://registry.npmmirror.com

# 复制源代码
COPY . .

# 暴露端口
EXPOSE 3000

# 启动命令
CMD ["node", "server.js"]
```

构建并运行：

```bash
# 构建镜像
docker build -t my-app:1.0 .

# 运行容器
docker run -d -p 3000:3000 my-app:1.0
```

## Docker Compose

多个服务协作时，用 `docker-compose.yml` 管理：

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - DATABASE_URL=mongodb://db:27017/mydb

  db:
    image: mongo:6
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

一键启动所有服务：

```bash
docker-compose up -d
```

## 最佳实践

1. **使用 `.dockerignore`** 排除 `node_modules`、`.git` 等目录
2. **利用层缓存**：把不常变的指令放在前面（如安装依赖）
3. **使用具体版本标签**：`node:18-alpine` 而不是 `node:latest`
4. **多阶段构建**：减小生产镜像体积

---

Docker 学习曲线不陡，但改变开发方式的效果是显著的。从现在开始容器化你的项目吧！
