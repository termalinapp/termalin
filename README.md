# Termalin

A fast SSH client for you and your AI agents. Windows · macOS · Linux.

- Tabbed terminal, SFTP browser with a built-in editor, dual-pane file manager
- Database client for 13 engines (PostgreSQL, MySQL, MongoDB, Redis, ClickHouse, SQL Server and more) - browse, query and edit over SSH or a Connector, no exposed ports
- End-to-end encrypted sync — the server only ever stores ciphertext
- Built-in MCP server: your agent can SSH too, while you watch every session live — keys never touch disk

**[Download](https://termal.in/download/)** · [Docs](https://termal.in/docs/) · [Blog](https://termal.in/blog/) · [Telegram](https://t.me/termalin)

## Install

```sh
# macOS (Homebrew)
brew install --cask termalinapp/termalin/termalin

# Windows (Scoop)
scoop bucket add termalin https://github.com/termalinapp/scoop-termalin
scoop install termalin
```

Installers for every OS: [termal.in/download](https://termal.in/download/).

## Connector in Docker

The Termalin Connector lets Termalin (and AI apps through its hosted MCP) reach an SSH server, an internal site or a desktop on a private network without opening a port. The official image runs it on anything with Docker: a NAS, a home server, a MikroTik router with containers (arm64/x86), Kubernetes or ECS.

```sh
docker run -d --name termalin-connector --restart unless-stopped   -v termalin:/data   -e TERMALIN_TOKEN=<token from termal.in/account, Connectors -> Add>   -e TERMALIN_TARGET=192.168.1.10:22 -e TERMALIN_USER=admin   ghcr.io/termalinapp/termalin-connector
```

On first start it enrols with the token and prints one line to add to `~/.ssh/authorized_keys` of the login user on the target server (`docker logs termalin-connector`, or `docker exec termalin-connector termalin-agent check`). Later starts just connect. The image is about 4 MB, runs as a non-root user and is built here ([workflow](.github/workflows/connector-image.yml)) only from the signed release binaries on termal.in, each checked against its signature.

## About this repository

This repository is the public home for Termalin: release notes and the issue tracker.
Found a bug or want a feature? [Open an issue](../../issues).
