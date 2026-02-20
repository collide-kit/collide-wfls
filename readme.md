# ⚫ @collide-kit/collide-wfls

[![CI](https://github.com/collide-kit/collide-wfls/actions/workflows/ci.yml/badge.svg)](https://github.com/collide-kit/collide-wfls/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

⚙️ GitHub Actions preset for Collide Tech

## 📦 `actions/prepare`

Sets up a complete CI environment: Node.js, Yarn Berry (via Corepack), and two-level dependency caching (`yarn cache` + `node_modules`).

### Inputs

| Name                   | Description                    | Default  |
| ---------------------- | ------------------------------ | -------- |
| `node-version`         | Node.js version                | `latest` |
| `yarn-version`         | Yarn version                   | `latest` |
| `enable-cache`         | Enable dependency caching      | `true`   |
| `install-dependencies` | Run `yarn install --immutable` | `true`   |

### Outputs

`node-version`, `yarn-version`, `cache-hit`, `yarn-cache-dir`, `install-duration`

### Basic Usage

```yaml
steps:
  - uses: actions/checkout@v6
  - uses: collide-kit/collide-workflows/actions/prepare@v1
  - run: yarn build
  - run: yarn test
```

<details>
<summary><strong>More examples</strong></summary>

**Custom versions:**

```yaml
- uses: collide-kit/collide-workflows/actions/prepare@v1
  with:
    node-version: '20.18.1'
    yarn-version: '4.12.0'
```

**Using outputs for conditional logic:**

```yaml
- name: Setup CI
  id: setup
  uses: collide-kit/collide-workflows/actions/prepare@v1

- name: Skip if cached
  if: steps.setup.outputs.cache-hit != 'true'
  run: yarn typecheck
```

**Matrix testing:**

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, macos-latest, windows-latest]
    node-version: ['20.x', '22.x', 'latest']
steps:
  - uses: collide-kit/collide-workflows/actions/prepare@v1
    with:
      node-version: ${{ matrix.node-version }}
```

</details>

## 🔧 How It Works

Two-level cache with key `{os}-node-{version}-yarn-{version}-{hash(yarn.lock, package.json)}`. Cache invalidates on OS, runtime version, or dependency changes.

## 🐛 Troubleshooting

| Problem                    | Solution                                                                                                  |
| -------------------------- | --------------------------------------------------------------------------------------------------------- |
| Cache not restored         | Normal on first run or after dependency/version changes. GitHub caches expire after 7 days of inactivity. |
| "Immutable install failed" | `yarn.lock` out of sync — run `yarn install` locally and commit the lockfile.                             |
| Wrong version installed    | Explicitly specify `node-version` and `yarn-version`.                                                     |

## 📄 License

MIT © 2026 CollideKit
