[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md)

# DeepSeek Harness Docker Images

## About this project

This project provides Alpine, Debian, and Distroless Docker images for running the DeepSeek Harness Web UI. Use it to launch a browser-based coding agent and work with a local project mounted into the container.

The container working directory is `/home/node`. By default, the host `work/` folder is mounted there. Put the files you want DeepSeek Harness to access inside `work/`.

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
  -v "$PWD/work:/home/node" \
  dsh-web:alpine
```

Debian:

```bash
docker run --rm \
  --name dsh-web-debian \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:debian
```

Distroless:

```bash
docker run --rm \
  --name dsh-web-distroless \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-distroless-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:distroless
```

## 3. Run with Docker Compose

Create the environment file:

```bash
cp compose/.env.example compose/.env
```

Open `compose/.env` and replace `your-api-key` with your DeepSeek API key. The default `WORK_PATH=../work` mounts the host `work/` folder into the container at `/home/node`. To use another project, set `WORK_PATH` to its absolute path. `DSH_VERSION` uses `latest` by default; replace it with a specific version only when you need to pin a release.

Start one of the images:

```bash
# Alpine
docker compose --env-file compose/.env -f compose/docker-compose.alpine.yml up -d --build

# Debian (recommended)
docker compose --env-file compose/.env -f compose/docker-compose.debian.yml up -d --build

# Distroless
docker compose --env-file compose/.env -f compose/docker-compose.distroless.yml up -d --build
```

Stop the container:

```bash
docker compose --env-file compose/.env -f compose/docker-compose.debian.yml down
```

## 4. Open the Web UI

Open `http://127.0.0.1:3080` in a browser.

## 5. Use another port

```bash
docker run --rm \
  -p 127.0.0.1:8080:8080 \
  -e PORT=8080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD/work:/home/node" \
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
