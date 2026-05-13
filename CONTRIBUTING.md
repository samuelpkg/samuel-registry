# Contributing to samuel-registry

## Quick contribution flow

1. Fork this repo.
2. Add a `[[plugins]]` block to `index.toml`. Keep entries alphabetical by `name`.
3. Open a PR. Wait for `validate` CI to pass (it checks schema + repo reachability + tag existence).
4. A maintainer reviews and merges.

## Entry schema (TOML)

| Field          | Required | Notes                                                                |
|----------------|----------|----------------------------------------------------------------------|
| `name`         | yes      | Matches `samuel install <name>`. Must be `[a-z0-9][a-z0-9-]*`, 2-64 chars. |
| `repo`         | yes      | Bare `github.com/<owner>/<repo>` (no protocol).                      |
| `subpath`      | no       | When the plugin lives in a subdirectory of the repo.                 |
| `latest`       | yes      | SemVer tag (`1.2.3`) or `main` for `upstream = true`.                |
| `description`  | no       | One sentence; shown in `samuel search`.                              |
| `categories`   | no       | `language` \| `framework` \| `workflow` \| `translator` \| `starter`. |
| `tags`         | no       | Free-form; e.g. `typescript`, `iac`, `mobile`.                       |
| `upstream`     | no       | `true` if the plugin is mirrored from an external maintainer.        |
| `deprecated`   | no       | `true` to hide from `samuel search` while keeping `samuel install` working. |

## What CI checks

- `index.toml` parses cleanly against the schema documented in RFD 0003.
- Every `repo` returns a non-404 from `HEAD https://<repo>`.
- For non-upstream entries, `git ls-remote --tags <repo>` confirms `latest` is a real tag.

## Code of conduct

This is a curated index. We accept plugins that have a clear scope, a usable
README, a passing release workflow, and a maintainer who responds to issues.
Plugins that are abandoned for >12 months get marked `deprecated = true`.
