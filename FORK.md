# About this fork

`519lab/n8n-sandbox-service` is a fork of `n8n-io/n8n-sandbox-service` (`main`,
service version 1.11.0). It exists so that 519lab/n8n's CI and its
`get-n8n.sh` installer pull sandbox images built here, never from upstream.

## What was removed

- **Every `.ee` path, from all of history.** The Firecracker runner
  (`cmd/runner-firecracker.ee`, `internal/runner/runtime/firecracker.ee`,
  `scripts/firecracker.ee`, `Dockerfile.ee.runner-firecracker`) is under the n8n
  Enterprise License (`LICENSE_EE.md`). It was stripped with
  `git filter-repo --invert-paths --path-regex '(^|/)[^/]*\.ee(\.|/|$)'`, so no
  commit here contains it. Never reintroduce a `.ee` file, or recover one from
  upstream or its history.
- **Firecracker leftovers** that only work with that runner: its e2e scripts,
  golden-build scripts, quickstart, `BUNDLE.md`, and Makefile targets.
- **Upstream's CI workflows.** They release to n8n-io's registries, npm and Helm
  repo, and run on n8n-io's infrastructure. They're replaced by `ci.yml` (format,
  vet, build, test) and `publish.yml`.

Some docs (`docs/security-model.md`, `docs/configuration.md`) still describe the
Firecracker runner; that runner is not part of this fork.

## What's here

Everything else is upstream's code under the Sustainable Use License
(`LICENSE.md`, unchanged). The Go module path is still
`github.com/n8n-io/sandbox-service`: it only names the module's own packages and
fetches nothing.

## Images

`publish.yml` runs on every push to `main` (and by hand) and pushes
`linux/amd64` images to GHCR, tagged `latest` and `sha-<commit>`:

| Image | Dockerfile |
| --- | --- |
| `ghcr.io/519lab/n8n-sandbox-service-api` | `Dockerfile.api` |
| `ghcr.io/519lab/n8n-sandbox-service-runner-dind` | `Dockerfile.runner-dind` |
| `ghcr.io/519lab/n8n-sandbox-service-sandbox` | `Dockerfile.sandbox` |

## Branches

`develop` is the default and integration branch; every change lands there by PR.
`main` is promoted from `develop` by PR (merge commit, never squash), and a
push to `main` publishes images.

## Pulling upstream changes

Fetch upstream `main`, and strip `.ee` paths before merging, using the same
filter as above on a scratch clone. Don't merge upstream directly.

Forked from upstream commit `9ceb238d56c790b9801f3a1cda6cd30d13ff7bb9`.
