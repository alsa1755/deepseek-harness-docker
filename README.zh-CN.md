[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md)

# DeepSeek Harness Docker 镜像

## 项目介绍

本项目提供用于运行 DeepSeek Harness Web 界面的 Alpine、Debian 和 Distroless Docker 镜像。你可以使用它在浏览器中启动编程 Agent，并处理挂载到容器中的本地项目。

容器工作目录为 `/work`。请将需要处理的项目挂载到该目录。

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
  -v "$PWD:/work" \
  dsh-web:alpine
```

Debian：

```bash
docker run --rm \
  --name dsh-web-debian \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="你的 API Key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:debian
```

Distroless：

```bash
docker run --rm \
  --name dsh-web-distroless \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="你的 API Key" \
  -v dsh-distroless-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:distroless
```

## 3. 打开 Web 界面

在浏览器中访问 `http://127.0.0.1:3080`。

## 4. 修改端口

```bash
docker run --rm \
  -p 127.0.0.1:8080:8080 \
  -e PORT=8080 \
  -e DEEPSEEK_API_KEY="你的 API Key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD:/work" \
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
