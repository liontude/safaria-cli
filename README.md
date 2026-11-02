# safaria

Find the AI models in your code and when they retire. Part of [Safaria](https://safaria.com).

```sh
brew install liontude/tap/safaria                  # macOS and Linux (Homebrew)
curl -fsSL https://safaria.com/install.sh | sh     # macOS and Linux
irm https://safaria.com/install.ps1 | iex          # Windows (PowerShell)
```

```sh
safaria login           # once: approve it with your Safaria account
safaria scan            # the current folder
safaria scan --fail-on 30d --format sarif   # in CI
```

**Your code never leaves your machine.** safaria downloads the public list of
model ids, matches it on your machine, and asks Safaria only about the ids it
found: their status, retirement date and replacement.

This repository holds the releases (binaries for macOS, Linux and Windows, with
their `SHA256SUMS`). The source code will be published here later. Using the
binaries is subject to [Safaria's terms](https://safaria.com/terms).
