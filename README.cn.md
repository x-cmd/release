# x-cmd/release

[x-cmd](https://github.com/x-cmd/x-cmd) 全部发布包的集中仓库。

**[x-cmd](https://www.x-cmd.com)** 是一个 POSIX Shell 标准库——为 Shell 脚本提供基础库，内置实用工具集，并包含自托管的包管理模块（`x pk`）。支持 bash/dash/zsh，运行在 Linux、macOS 和 WSL 上，无需 sudo。

> 主仓库：[x-cmd/x-cmd](https://github.com/x-cmd/x-cmd) | 官网：[x-cmd.com](https://www.x-cmd.com)

## 为什么使用独立仓库？

### 基于 Git 的同步机制

发布包直接存储在 git 仓库中。你可以用标准的 git 操作 —— `git clone`、`git pull`、`git fetch` —— 来同步整个发布集，无需额外工具。

### 包体积小，适合 Git 存储

单个 x-cmd 包通常在 1–5 MB（约 2–3 MB）。在这个量级下，将二进制文件存入 git 仓库是可行的，无需单独的制品仓库。

### 超越 GitHub Releases

如果仅依赖 GitHub Releases，会存在单点故障风险 —— 受制于 GitHub 的可用性、带宽限制和频率限制。基于 git 的方案是平台无关的。

### 多镜像 —— 天然防篡改

仓库在三个独立平台之间镜像。这不仅是为了可用性，更关键的是提供**防篡改保障**：

- 每个镜像持有完全相同的制品副本，包括加密校验和（`sum/`、`index.yml`）
- 如果任一平台被篡改，用户可以通过对比另外两个镜像发现异常
- 任何单一平台运营商都无法在不被发现的情况下静默修改发布包
- 用户可以从任意镜像 `git clone`，然后交叉比对提交哈希和校验和

| 平台 | 地址 |
|------|------|
| GitHub | https://github.com/x-cmd/release |
| Codeberg | https://codeberg.org/x-cmd/release |
| Gitee | https://gitee.com/x-cmd/release |

**验证方法：** 从两个不同镜像克隆并对比：

```bash
git clone https://github.com/x-cmd/release.git   mirror-a
git clone https://codeberg.org/x-cmd/release.git  mirror-b
diff <(cd mirror-a && git rev-parse HEAD) <(cd mirror-b && git rev-parse HEAD)
```

提交哈希一致，说明制品未被篡改。

### 可迁移、可扩展

因为一切都是 git，发布系统天然易于扩展和适配：

- **扩展镜像** — 需要覆盖新区域或新平台？直接 push 到另一个 git 主机即可，无需改动任何工具链
- **私有化部署** — 企业可以在内网维护私有镜像，支持离线/气隙环境。一次克隆，内部分发
- **无供应商锁定** — 整个发布集就是一个标准 git 仓库，随时可以迁移到任意 git 托管平台

## 目录结构

```
index.yml              版本索引（含校验和、日期、更新日志）
dist/
  └── v{version}/
      ├── allinone.tgz           最小引导包
      ├── full.tgz               完整安装包
      ├── x-cmd_{v}_all.apk      Alpine Linux 包（v0.9.6 起）
      ├── x-cmd_{v}_all.deb      Debian/Ubuntu 包（v0.9.6 起）
      ├── x-cmd_{v}_all.rpm      Fedora/RHEL 包（v0.9.6 起）
      └── x-cmd_{v}_all.pkg.tar.zst  Arch Linux 包（v0.9.6 起）
sum/                   每个文件的完整性校验（SHA-512）
```

### 包格式（v0.9.6 起）

从 v0.9.6 开始，为主流 Linux 发行版提供原生包：

| 格式 | 发行版 | 安装命令 |
|------|--------|----------|
| `.apk` | Alpine Linux | `apk add x-cmd_0.9.6_all.apk` |
| `.deb` | Debian、Ubuntu | `dpkg -i x-cmd_0.9.6_all.deb` |
| `.rpm` | Fedora、RHEL、CentOS | `rpm -i x-cmd_0.9.6_all.rpm` |
| `.pkg.tar.zst` | Arch Linux | `pacman -U x-cmd_0.9.6_all.pkg.tar.zst` |

v0.9.6 之前的版本仅提供 `allinone.tgz` 和 `full.tgz`。

## 使用方法

```bash
# 克隆发布仓库
git clone https://github.com/x-cmd/release.git

# 或使用镜像
git clone https://codeberg.org/x-cmd/release.git
git clone https://gitee.com/x-cmd/release.git
```

## 许可证

Apache License 2.0 — 版权所有 Li Junhao 李均豪 及 X-CMD 团队。详见 [LICENSE](LICENSE)。
