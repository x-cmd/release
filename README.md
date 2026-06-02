# x-cmd/release

Central repository for all x-cmd release artifacts.

## Why a dedicated release repo?

### Git-based sync

Release artifacts are stored directly in a git repository. This means you can sync the entire release set with standard git operations — `git clone`, `git pull`, `git fetch` — no special tooling required.

### Small packages, git-friendly

Individual x-cmd packages are typically 1–5 MB (around 2–3 MB each). At this size, storing binaries in git is practical and avoids the overhead of a separate artifact registry.

### Beyond GitHub Releases

Relying solely on GitHub Releases would create a single point of failure — subject to GitHub's availability, bandwidth limits, and rate limiting. A git-based approach is platform-agnostic.

### Multi-mirror architecture

The repo is mirrored across multiple platforms, ensuring global availability and fairness:

| Platform | URL |
|----------|-----|
| GitHub | https://github.com/x-cmd/release |
| Codeberg | https://codeberg.org/x-cmd/release |
| Gitee | https://gitee.com/x-cmd/release |

Anyone can replicate the full release set with a simple `git clone` from any mirror.

## Structure

```
index.yml     Version index with checksums
dist/         Release artifacts organized by module
sum/          Integrity checksums
```

## Usage

```bash
# Clone the release repo
git clone https://github.com/x-cmd/release.git

# Or use a mirror
git clone https://codeberg.org/x-cmd/release.git
git clone https://gitee.com/x-cmd/release.git
```

## License

See [LICENSE](LICENSE).
