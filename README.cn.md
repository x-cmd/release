# x-cmd/release

x-cmd 全部发布包的集中仓库。

## 为什么使用独立仓库？

### 基于 Git 的同步机制

发布包直接存储在 git 仓库中。你可以用标准的 git 操作 —— `git clone`、`git pull`、`git fetch` —— 来同步整个发布集，无需额外工具。

### 包体积小，适合 Git 存储

单个 x-cmd 包通常在 1–5 MB（约 2–3 MB）。在这个量级下，将二进制文件存入 git 仓库是可行的，无需单独的制品仓库。

### 超越 GitHub Releases

如果仅依赖 GitHub Releases，会存在单点故障风险 —— 受制于 GitHub 的可用性、带宽限制和频率限制。基于 git 的方案是平台无关的。

### 多镜像架构

仓库在多个平台之间镜像，确保全球可用性和公平性：

| 平台 | 地址 |
|------|------|
| GitHub | https://github.com/x-cmd/release |
| Codeberg | https://codeberg.org/x-cmd/release |
| Gitee | https://gitee.com/x-cmd/release |

任何人都可以从任意镜像通过 `git clone` 获取完整的发布集。

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
