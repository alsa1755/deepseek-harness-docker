[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md)

# DeepSeek Harness Docker Images

## About this project

This project provides Alpine, Debian, and Distroless Docker images for running the DeepSeek Harness Web UI. Use it to launch a browser-based coding agent and work with a local project mounted into the container.

The container work folder is `/work`. Mount the project you want to use at this path.

## 1. Build an image

Alpine:

```bash
docker build -f dockerfiles/Dockerfile.alpine -t dsh-web:alpine .
```

Debian:

```bash
docker build -f dockerfiles/Dockerfile.debian -t dsh-web:debian .
```

Distroless:

```bash
docker build -f dockerfiles/Dockerfile.distroless -t dsh-web:distroless .
```

## 2. Run the container

Alpine:

```bash
docker run --rm \
  --name dsh-web-alpine \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-alpine-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:alpine
```

Debian:

```bash
docker run --rm \
  --name dsh-web-debian \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:debian
```

Distroless:

```bash
docker run --rm \
  --name dsh-web-distroless \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-distroless-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:distroless
```

## 3. Open the Web UI

Open `http://127.0.0.1:3080` in a browser.

## 4. Use another port

```bash
docker run --rm \
  -p 127.0.0.1:8080:8080 \
  -e PORT=8080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:debian
```

Open `http://127.0.0.1:8080`.

## Image differences

| Image | Characteristics | Recommended use |
|---|---|---|
| Alpine | Small image with Bash and Git | Lightweight deployments |
| Debian | Full Bash and Git support with easier troubleshooting | Daily use and coding-agent workloads |
| Distroless | Smallest runtime without a shell, Git, or package manager | Restricted Web-only workloads |

**Debian is recommended for most users.**
