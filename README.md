# safaria

Find the AI models in your code and when they retire. Part of [Safaria](https://safaria.com).

```sh
brew install liontude/tap/safaria                  # macOS and Linux (Homebrew)
curl -fsSL https://safaria.com/install.sh | sh     # macOS and Linux
irm https://safaria.com/install.ps1 | iex          # Windows (PowerShell)

safaria login           # once: approve it with your Safaria account
safaria scan            # the current folder
```

**Your code never leaves your machine.** safaria downloads the public list of
model ids, matches it on your machine, and asks Safaria only about the ids it
found: their status, retirement date and replacement.

## GitHub Action

Create an API key in Safaria (Settings › API keys), save it as the
`SAFARIA_TOKEN` secret, and add a step:

```yaml
- uses: actions/checkout@v4
- uses: liontude/safaria-cli@v0
  with:
    token: ${{ secrets.SAFARIA_TOKEN }}
    fail-on: 30d # fail if a model retires within 30 days
    sync: true # track what it finds, to get alerts
```

Each repository that runs it takes one of your plan's places: Free covers 1,
Pro 10 and Team all of them. Safaria on your own computer doesn't count.

With GitHub code scanning, upload the SARIF report too:

```yaml
- uses: liontude/safaria-cli@v0
  with:
    token: ${{ secrets.SAFARIA_TOKEN }}
    sarif-file: safaria.sarif
- uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: safaria.sarif
```

| Input        |                                                                        |
| ------------ | ---------------------------------------------------------------------- |
| `token`      | A Safaria API key (required)                                           |
| `path`       | Folder to scan (`.`)                                                   |
| `fail-on`    | `retired`, `deprecated` or a number of days (`30d`); empty never fails |
| `sync`       | `true` to track what it finds in your space                            |
| `project`    | With `sync`, also put it in this project                               |
| `sarif-file` | Also write a SARIF report here                                         |
| `version`    | A safaria version (`0.4.0`) or `latest`                                |

Linux and macOS runners.

This repository holds the releases (binaries for macOS, Linux and Windows, with
their `SHA256SUMS`) and the GitHub Action. The source code will be published
here later. Using the binaries is subject to [Safaria's terms](https://safaria.com/terms).

## Verify a download

Each release's `SHA256SUMS` is signed by Safaria (`SHA256SUMS.sig`). The
installers and `safaria update` check it for you; to check it by hand:

```sh
base=https://github.com/liontude/safaria-cli/releases/latest/download
curl -fsSLO "$base/SHA256SUMS" -fsSLO "$base/SHA256SUMS.sig"
curl -fsSL https://safaria.com/safaria-release.pem -o safaria-release.pem
openssl dgst -sha256 -verify safaria-release.pem -signature SHA256SUMS.sig SHA256SUMS
sha256sum -c SHA256SUMS --ignore-missing
```

## License

Not open source. You can read the code and use the official binaries and this
action with Safaria; you can't copy, modify or redistribute them. See
[LICENSE](LICENSE).
