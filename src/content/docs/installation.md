---
title: Installation
weight: 1
---

## Quick install (macOS / Linux)

```sh
curl -fsSL https://raw.githubusercontent.com/kineticloom/plydb/main/install.sh | sh
```

## Quick install (Windows — PowerShell)

```powershell
irm https://raw.githubusercontent.com/kineticloom/plydb/main/install.ps1 | iex
```

## Options

The install script accepts the following environment variables:

| Variable            | Description                            | Default        |
| :------------------ | :------------------------------------- | :------------- |
| `PLYDB_INSTALL_DIR` | Where to place the binary              | `~/.local/bin` |
| `PLYDB_VERSION`     | Version tag to install (e.g. `v0.1.0`) | latest         |

For example, to install a specific version to a custom directory:

```sh
PLYDB_INSTALL_DIR=/usr/local/bin PLYDB_VERSION=v0.1.0 \
  curl -fsSL https://raw.githubusercontent.com/kineticloom/plydb/main/install.sh | sh
```

## Manual download

Pre-built binaries for all platforms are available on the [Releases](https://github.com/kineticloom/plydb/releases) page. Download the archive for your OS and architecture, extract it, and place the `plydb` binary somewhere on your `PATH`.

## Build from source

If you have the source repository cloned, you can build from source:

```sh
make build
```
