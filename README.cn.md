# x-cmd/release

x-cmd 全部发布包的集中仓库。

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

## 目录结构

```
index.yml     版本索引（含校验和）
dist/         按模块组织的发布包
sum/          完整性校验文件
```

## 使用方法

```bash
# 克隆发布仓库
git clone https://github.com/x-cmd/release.git

# 或使用镜像
git clone https://codeberg.org/x-cmd/release.git
git clone https://gitee.com/x-cmd/release.git
```

## 许可证

参见 [LICENSE](LICENSE)。
