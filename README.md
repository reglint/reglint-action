# reglint-action

GitHub Action for [RegLint](https://github.com/reglint/reglint) — a regex-based linter for source repositories. Runs `reglint analyze` with your YAML-defined rules and reports findings as [GitHub Actions annotations](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions) (`::error` / `::warning` / `::notice`), failing the step when matches meet the `fail-on` severity threshold.

```yaml
name: scan
on: [push]
jobs:
  reglint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: reglint/reglint-action@v1
        with:
          fail-on: error
```

## Inputs

| Input          | Description                                                 | Default              |
| -------------- | ----------------------------------------------------------- | -------------------- |
| `tool-version` | RegLint version to install (e.g. `v0.1.0`)                  | `latest`             |
| `config`       | Path to the YAML rules config file                          | `reglint-rules.yaml` |
| `paths`        | Paths to scan (space-separated)                             | `.`                  |
| `fail-on`      | Fail if matches at or above severity (`error` or `warning`) | *(empty)*            |

## How it works

The action resolves the requested RegLint release, fetches `install.sh` from that exact immutable tag, and installs the checksum-verified binary to `~/.local/bin`. Pinning `tool-version` therefore pins the entire install chain — installer, checksums, and binary all come from the same release.

Annotations require RegLint v0.2.0+. With the default `tool-version: latest` this is automatic; pinning an older RegLint is only supported on action `v1.0.0` (console output).

Generate a starter config with `reglint init`, or see the [RegLint docs](https://github.com/reglint/reglint#readme).

## License

[MIT](LICENSE)
