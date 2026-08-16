[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md)

# DeepSeek Harness Docker 镜像

## 项目介绍

本项目提供用于运行 DeepSeek Harness Web 界面的 Alpine、Debian 和 Distroless Docker 镜像。你可以使用它在浏览器中启动编程 Agent，并处理挂载到容器中的本地项目。

容器工作目录为 `/home/node`。默认会把宿主机的 `work/` 文件夹挂载到这里，请将需要让 DeepSeek Harness 访问的文件放入 `work/`。

## 1. 构建镜像

Alpine：

```bash
docker build -f dockerfiles/Dockerfile.alpine -t dsh-web:alpine .
```

Debian：

```bash
docker build -f dockerfiles/Dockerfile.debian -t dsh-web:debian .
```

Distroless：

```bash
docker build -f dockerfiles/Dockerfile.distroless -t dsh-web:distroless .
```

## 2. 启动容器

Alpine：

```bash
docker run --rm \
  --name dsh-web-alpine \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="你的 API Key" \
  -v dsh-alpine-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:alpine
```

Debian：

```bash
docker run --rm \
  --name dsh-web-debian \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="你的 API Key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:debian
```

Distroless：

```bash
docker run --rm \
  --name dsh-web-distroless \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="你的 API Key" \
  -v dsh-distroless-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:distroless
```

## 3. 使用 Docker Compose 启动

创建环境变量文件：

```bash
cp compose/.env.example compose/.env
```

打开 `compose/.env`，将 `your-api-key` 替换为你的 DeepSeek API Key。默认的 `WORK_PATH=../work` 会把宿主机的 `work/` 文件夹挂载到容器 `/home/node`。如需使用其他项目，请将 `WORK_PATH` 改为该项目的绝对路径。`DSH_VERSION` 默认使用 `latest`；只有需要固定版本时才改为具体版本号。

选择一个镜像启动：

```bash
# Alpine
docker compose --env-file compose/.env -f compose/docker-compose.alpine.yml up -d --build

# Debian（推荐）
docker compose --env-file compose/.env -f compose/docker-compose.debian.yml up -d --build

# Distroless
docker compose --env-file compose/.env -f compose/docker-compose.distroless.yml up -d --build
```

停止容器：

```bash
docker compose --env-file compose/.env -f compose/docker-compose.debian.yml down
```

## 4. 打开 Web 界面

在浏览器中访问 `http://127.0.0.1:3080`。

## 5. 修改端口

```bash
docker run --rm \
  -p 127.0.0.1:8080:8080 \
  -e PORT=8080 \
  -e DEEPSEEK_API_KEY="你的 API Key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:debian
```

在浏览器中访问 `http://127.0.0.1:8080`。

## 镜像区别

| 镜像 | 特点 | 适用场景 |
|---|---|---|
| Alpine | 镜像较小，包含 Bash 和 Git | 轻量化部署 |
| Debian | 完整支持 Bash 和 Git，排查问题更方便 | 日常使用和编程 Agent 工作负载 |
| Distroless | 运行环境最精简，不包含 Shell、Git 和包管理器 | 受限的纯 Web 工作负载 |

**推荐大多数用户使用 Debian 版本。**
