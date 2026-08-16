[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md)

# DeepSeek Harness Docker イメージ

## このプロジェクトについて

このプロジェクトは、DeepSeek Harness Web UI を実行するための Alpine、Debian、Distroless Docker イメージを提供します。ブラウザーでコーディングエージェントを起動し、コンテナにマウントしたローカルプロジェクトを操作できます。

コンテナの作業フォルダーは `/work` です。使用するプロジェクトをこのパスにマウントしてください。

## 1. イメージをビルドする

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

## 2. コンテナを起動する

Alpine：

```bash
docker run --rm \
  --name dsh-web-alpine \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-alpine-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:alpine
```

Debian：

```bash
docker run --rm \
  --name dsh-web-debian \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:debian
```

Distroless：

```bash
docker run --rm \
  --name dsh-web-distroless \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-distroless-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:distroless
```

## 3. Web UI を開く

ブラウザーで `http://127.0.0.1:3080` を開きます。

## 4. ポートを変更する

```bash
docker run --rm \
  -p 127.0.0.1:8080:8080 \
  -e PORT=8080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD:/work" \
  dsh-web:debian
```

ブラウザーで `http://127.0.0.1:8080` を開きます。

## イメージの違い

| イメージ | 特徴 | 推奨用途 |
|---|---|---|
| Alpine | 小型で Bash と Git を含む | 軽量なデプロイ |
| Debian | Bash と Git を完全に利用でき、トラブルシューティングが容易 | 通常利用とコーディングエージェントのワークロード |
| Distroless | Shell、Git、パッケージマネージャーを含まない最小構成 | 制限された Web 専用ワークロード |

**ほとんどのユーザーには Debian 版を推奨します。**
