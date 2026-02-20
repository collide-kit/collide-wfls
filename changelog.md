# ⚫ @collide-kit/collide-wfls

## 1.0.0 🚀

### Major Release

🎉 **Initial release of `@collide-kit/collide-wfls`**

The first stable release of **@collide-kit/collide-wfls**

### ✨ Features

- **`actions/prepare`** — CI setup: Node.js + Yarn (Corepack), dependency installation with two-level caching (`yarn cache` + `node_modules`), immutable installs, environment metadata outputs, and auto-summary.
- **Modular actions** — `setup-node`, `setup-yarn`, `cache-deps` as standalone reusable building blocks.
- **Release Workflow** — automatic version management via changesets (PR + GitHub Releases, no npm publishing).
- **Dependency Review** — vulnerability scanning on PRs (moderate+) with automatic comments.
- **Actions Up** — weekly GitHub Actions updates with SHA pinning and automatic PR creation.

### 🧪 Testing

Matrix testing: Ubuntu / macOS / Windows × Node.js 20.x / 22.x / latest, including edge cases (no cache, no install, custom versions).

### 🔐 Security

All actions pinned by SHA. Automatic vulnerability scanning and dependency updates.

### 📚 Documentation

README with usage examples, troubleshooting, architecture overview, and caching strategy explanation.
