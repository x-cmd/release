# x-cmd/release

Central repository for all [x-cmd](https://github.com/x-cmd/x-cmd) release artifacts.

**[x-cmd](https://www.x-cmd.com)** is a POSIX shell standard library — a foundation for shell scripting that includes built-in utility tools and a self-hosted package manager module (`x pk`). It runs on bash/dash/zsh across Linux, macOS, and WSL, with no sudo required.

> Main repo: [x-cmd/x-cmd](https://github.com/x-cmd/x-cmd) | Website: [x-cmd.com](https://www.x-cmd.com)

## Why a dedicated release repo?

### Git-based sync

Release artifacts are stored directly in a git repository. This means you can sync the entire release set with standard git operations — `git clone`, `git pull`, `git fetch` — no special tooling required.

### Small packages, git-friendly

Individual x-cmd packages are typically 1–5 MB (around 2–3 MB each). At this size, storing binaries in git is practical and avoids the overhead of a separate artifact registry.

### Beyond GitHub Releases

Relying solely on GitHub Releases would create a single point of failure — subject to GitHub's availability, bandwidth limits, and rate limiting. A git-based approach is platform-agnostic.

### Multi-mirror — tamper-proof by design

The repo is mirrored across three independent platforms. This is not just for availability — it provides **tamper resistance**:

- Every mirror holds an identical copy of all artifacts, including cryptographic checksums (`sum/`, `index.yml`).
- If any single platform is compromised, users can verify integrity by comparing against the other two mirrors.
- No single platform operator can silently alter release artifacts without detection.
- Users can `git clone` from any mirror and cross-check commit hashes and checksums.

| Platform | URL |
|----------|-----|
| GitHub | https://github.com/x-cmd/release |
| Codeberg | https://codeberg.org/x-cmd/release |
| Gitee | https://gitee.com/x-cmd/release |

**How to verify:** Clone from two different mirrors and compare:

```bash
git clone https://github.com/x-cmd/release.git   mirror-a
git clone https://codeberg.org/x-cmd/release.git  mirror-b
diff <(cd mirror-a && git rev-parse HEAD) <(cd mirror-b && git rev-parse HEAD)
```

If the commit hashes match, the artifacts are untampered.

### Portable and extensible

Because everything is just git, the release system is easy to extend and adapt:

- **Add more mirrors** — Need a new region or platform? Just push to another git host. No tooling changes required.
- **Private mirrors** — Organizations can maintain internal mirrors for air-gapped or self-hosted environments. Clone once, serve internally.
- **No vendor lock-in** — The entire release set is a standard git repo. Migrate to any git hosting provider at any time.

## Structure

```
index.yml              Version index with checksums, dates, and changelogs
dist/
  └── v{version}/
      ├── allinone.tgz           Minimal bootstrap package
      ├── full.tgz               Full package bundle
      ├── x-cmd_{v}_all.apk      Alpine Linux package (since v0.9.6)
      ├── x-cmd_{v}_all.deb      Debian/Ubuntu package (since v0.9.6)
      ├── x-cmd_{v}_all.rpm      Fedora/RHEL package (since v0.9.6)
      └── x-cmd_{v}_all.pkg.tar.zst  Arch Linux package (since v0.9.6)
sum/                   Per-file integrity checksums (SHA-512)
```

### Package formats (v0.9.6+)

Starting from v0.9.6, native packages are provided for major Linux distributions:

| Format | Distribution | Install |
|--------|-------------|---------|
| `.apk` | Alpine Linux | `apk add x-cmd_0.9.6_all.apk` |
| `.deb` | Debian, Ubuntu | `dpkg -i x-cmd_0.9.6_all.deb` |
| `.rpm` | Fedora, RHEL, CentOS | `rpm -i x-cmd_0.9.6_all.rpm` |
| `.pkg.tar.zst` | Arch Linux | `pacman -U x-cmd_0.9.6_all.pkg.tar.zst` |

Earlier versions (pre-v0.9.6) ship only `allinone.tgz` and `full.tgz`.

## Usage

```bash
# Clone the release repo
git clone https://github.com/x-cmd/release.git

# Or use a mirror
git clone https://codeberg.org/x-cmd/release.git
git clone https://gitee.com/x-cmd/release.git
```

## License

Apache License 2.0 — Copyright Li Junhao 李均豪 and X-CMD Team. See [LICENSE](LICENSE).
