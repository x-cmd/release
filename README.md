# x-cmd/release

Central repository for all x-cmd release artifacts.

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
