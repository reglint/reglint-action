# RegLint Action

[![Release](https://img.shields.io/github/v/release/reglint/reglint-action)](https://github.com/reglint/reglint-action/releases)

Run [RegLint](https://github.com/reglint/reglint) — the regex-based linter with YAML-defined rules — in your GitHub Actions workflow and report findings as native PR annotations. Every match becomes an inline `::error` / `::warning` / `::notice` annotation on the exact file and line, and the step fails when matches meet the `fail-on` severity threshold. No token, no upload step, no extra permissions.

## Usage

```yaml
name: scan
on: [push]
jobs:
  reglint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: reglint/reglint-action@v1.1.0
        with:
          fail-on: error
```

## What inputs does the action accept?

| Input          | Description                                                 | Default              |
| -------------- | ----------------------------------------------------------- | -------------------- |
| `tool-version` | RegLint version to install (e.g. `v0.1.0`)                  | `latest`             |
| `config`       | Path to the YAML rules config file                          | `reglint-rules.yaml` |
| `paths`        | Paths to scan (space-separated)                             | `.`                  |
| `fail-on`      | Fail if matches at or above severity (`error` or `warning`) | *(empty)*            |

## How does the action work?

The action resolves the requested RegLint release, fetches `install.sh` from that exact immutable tag, and installs the checksum-verified binary to `~/.local/bin`. Pinning `tool-version` therefore pins the entire install chain — installer, checksums, and binary all come from the same release.

Annotations require RegLint v0.2.0+. With the default `tool-version: latest` this is automatic; pinning an older RegLint is only supported on action `v1.0.0` (console output).

## FAQ

### Where do the findings show up?

Inline in the PR "Files changed" view and in the workflow run summary, as native GitHub annotations on the exact file and line. GitHub caps annotations at 10 per severity per step; for the complete list, run RegLint with `--format json` or `--format sarif` as shown in the [RegLint CI recipes](https://github.com/reglint/reglint#ci-recipe-github-actions).

### Does the action need a token or extra permissions?

No. Annotations are workflow commands written to the step log, which the runner surfaces natively — default permissions with `contents: read` are enough.

### How do I pin the RegLint version?

Set `tool-version: v0.2.0` (for example). Pinning pins the whole install chain — installer, checksums, and binary all come from that single release. Annotations require RegLint v0.2.0+.

### Which action version should I pin?

Pin a full tag, e.g. `reglint/reglint-action@v1.1.0`. This action publishes full `vX.Y.Z` tags only — there is no moving major tag like `@v1` — so workflows always reference an exact, immutable version.

### How do I define the rules?

In a `reglint-rules.yaml` file committed to your repo (path configurable via the `config` input). Generate a starter with `reglint init`, or see the rule schema and examples in the [RegLint docs](https://github.com/reglint/reglint#readme).

## License

[MIT](LICENSE)
