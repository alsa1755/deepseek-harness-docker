[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md)

# DeepSeek Harness Docker イメージ

## このプロジェクトについて

このプロジェクトは、DeepSeek Harness Web UI を実行するための Alpine、Debian、Distroless Docker イメージを提供します。ブラウザーでコーディングエージェントを起動し、コンテナにマウントしたローカルプロジェクトを操作できます。

コンテナの作業フォルダーは `/home/node` です。既定ではホストの `work/` フォルダーがここにマウントされます。DeepSeek Harness からアクセスするファイルを `work/` に配置してください。

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
  -v "$PWD/work:/home/node" \
  dsh-web:alpine
```

Debian：

```bash
docker run --rm \
  --name dsh-web-debian \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:debian
```

Distroless：

```bash
docker run --rm \
  --name dsh-web-distroless \
  -p 127.0.0.1:3080:3080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-distroless-data:/data/dsh \
  -v "$PWD/work:/home/node" \
  dsh-web:distroless
```

## 3. Docker Compose で起動する

環境変数ファイルを作成します：

```bash
cp compose/.env.example compose/.env
```

`compose/.env` を開き、`your-api-key` を DeepSeek API キーに置き換えます。既定の `WORK_PATH=../work` はホストの `work/` フォルダーをコンテナの `/home/node` にマウントします。別のプロジェクトを使用する場合は、`WORK_PATH` をその絶対パスに変更してください。`DSH_VERSION` の既定値は `latest` です。特定のリリースを固定する場合のみ具体的なバージョンに変更してください。

使用するイメージを一つ選んで起動します：

```bash
# Alpine
docker compose --env-file compose/.env -f compose/docker-compose.alpine.yml up -d --build

# Debian（推奨）
docker compose --env-file compose/.env -f compose/docker-compose.debian.yml up -d --build

# Distroless
docker compose --env-file compose/.env -f compose/docker-compose.distroless.yml up -d --build
```

コンテナを停止します：

```bash
docker compose --env-file compose/.env -f compose/docker-compose.debian.yml down
```

## 4. Web UI を開く

ブラウザーで `http://127.0.0.1:3080` を開きます。

## 5. ポートを変更する

```bash
docker run --rm \
  -p 127.0.0.1:8080:8080 \
  -e PORT=8080 \
  -e DEEPSEEK_API_KEY="your-api-key" \
  -v dsh-debian-data:/data/dsh \
  -v "$PWD/work:/home/node" \
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
