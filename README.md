# 🍦 Popsicle Registry

The official package index for [Popsicle](https://github.com/popsicle-lab/popsicle) — a spec-driven development orchestration engine.

## What is this?

This repository is a **git-based package index** (similar to [crates.io-index](https://github.com/rust-lang/crates.io-index) or [Homebrew taps](https://docs.brew.sh/Taps)). It contains metadata about published **modules** and **tools** that the `popsicle` CLI can search and install.

Actual package source code lives in their own repositories — this index only stores version metadata with pointers to the source.

## Index Format

Each package has a file at `<prefix>/<name>` containing one JSON line per published version (NDJSON format).

### Path convention (crates.io-style)

| Name length | Path pattern | Example |
|---|---|---|
| 1 char | `1/<name>` | `1/x` |
| 2 chars | `2/<name>` | `2/ab` |
| 3 chars | `3/<first-char>/<name>` | `3/a/abc` |
| 4+ chars | `<first-2>/<next-2>/<name>` | `sp/ec/spec-development` |

### Version entry schema

Each line is a JSON object:

```json
{
  "name": "spec-development",
  "vers": "1.0.0",
  "type": "module",
  "description": "Full SDLC spec-driven development",
  "author": "popsicle-lab",
  "repository": "https://github.com/popsicle-lab/popsicle-spec-development",
  "source": "github:popsicle-lab/popsicle-spec-development#v1.0.0",
  "skills": ["domain-analysis", "product-prd", "tech-rfc"],
  "pipelines": ["full-sdlc", "tech-sdlc"],
  "tools": [],
  "deps": [],
  "keywords": ["sdlc", "spec", "development"],
  "yanked": false,
  "published_at": "2026-03-28T00:00:00Z"
}
```

### Package types

| Type | Description | Contains |
|---|---|---|
| **module** | A development orchestration module | Skills, Pipelines, optionally Tools |
| **tool** | A standalone action skill | Command or AI prompt template |

## Using the registry

### Search

```bash
popsicle registry search "diagram"
popsicle registry search --type module "sdlc"
```

### Install from registry

```bash
# Modules
popsicle module install spec-development
popsicle module install spec-development@1.0.0

# Tools
popsicle tool install draw-diagram
popsicle tool install draw-diagram@2.0.0
```

### Show package info

```bash
popsicle registry info spec-development
```

### Publish

From within a module or tool directory:

```bash
popsicle registry publish \
  --source github:myorg/my-module#v1.0.0 \
  --repository https://github.com/myorg/my-module \
  --keywords sdlc,planning
```

## Publishing guidelines

1. **Use semantic versioning** — `MAJOR.MINOR.PATCH`
2. **Tag your releases** — The `source` field should point to a git tag (e.g. `github:org/repo#v1.0.0`)
3. **Include a README** — Helps users understand your package
4. **One package per index entry** — Each module or tool gets its own index file

## Contributing

To add or update a package, you can either:
- Use `popsicle registry publish` (recommended)
- Submit a pull request adding a JSON line to the appropriate index file

## License

Apache-2.0
