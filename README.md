# samuel-registry

The canonical plugin index for Samuel v2. Plugin discovery (`samuel search`,
`samuel install`, `samuel ls --all`) reads `index.toml` from this repo's
default branch via the loader's registry source.

## Adding a plugin

1. Open a PR adding an entry to `index.toml`:

   ```toml
   [[plugins]]
   name        = "my-plugin"
   repo        = "github.com/<owner>/samuel-my-plugin"
   latest      = "1.0.0"
   description = "One-line summary used in samuel search results."
   categories  = ["framework"]    # or "language" | "workflow" | "translator" | "starter"
   tags        = ["typescript"]   # optional, free-form
   ```

2. CI (`.github/workflows/validate.yml`) will:
   - parse `index.toml` against the schema,
   - HEAD-check every `repo` URL is reachable,
   - run `git ls-remote --tags <repo>` and confirm `latest` exists as a tag.

3. Once green, a maintainer merges. The new plugin is installable within
   minutes (the loader caches the index for 60s by default).

## Upstream plugins

For plugins that live in someone else's monorepo (e.g.
`github.com/anthropics/skills/algorithmic-art`), use:

```toml
[[plugins]]
name     = "algorithmic-art"
repo     = "github.com/anthropics/skills"
subpath  = "algorithmic-art"
latest   = "main"
upstream = true
```

The `upstream = true` flag tells the loader and curators not to expect a
SemVer tag scheme — these track a moving target.

## Removing or renaming

Open a PR with the change. Removals require a deprecation period: mark the
entry with `deprecated = true` for two minor releases before deleting.

## Schema

See `index.toml` header. The full schema is documented in RFD 0003.
