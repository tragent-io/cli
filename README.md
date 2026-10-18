# tragent CLI

Test whether AI shopping agents can find, price and buy from your store, from
your terminal or your CI pipeline. Learn more at [tragent.io](https://tragent.io).

This repository hosts the CLI's **release binaries**, the **GitHub Action**
and the **GitLab CI template**. The source is maintained privately.

## Install

```bash
# Run without installing
npx @tragent/cli scan https://your-store.com

# Or install it
npm i -g @tragent/cli
tragent version
```

You can also download an archive for your platform from
[Releases](https://github.com/tragent-io/cli/releases). Check it against
`checksums.txt` before use. Builds are published for Linux, macOS and Windows,
on amd64 and arm64.

`tragent scan` runs entirely on your machine and needs no account. Every other
command talks to the tragent API and needs an API key (`tk_live_…`) from your
tragent dashboard, set as `TRAGENT_API_KEY`.

## GitHub Actions

```yaml
name: agent readiness
on: [pull_request]

jobs:
  tragent:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write   # only needed for comment: true
    steps:
      - uses: actions/checkout@v4
      - uses: tragent-io/cli@v1
        with:
          api-key: ${{ secrets.TRAGENT_API_KEY }}
          target: https://staging.your-store.example
          suite: suites/retail-core.yaml
          gate: true
          comment: true
```

The Action downloads the matching release binary, checks it against
`checksums.txt`, runs your test plan and fails the build when the store gets
worse for agents. All inputs are listed in [`action.yml`](action.yml).

## GitLab CI

```yaml
include:
  - remote: 'https://raw.githubusercontent.com/tragent-io/cli/v1/gitlab/.tragent.yml'
```

Set `TRAGENT_API_KEY` as a masked CI/CD variable. The template is in
[`gitlab/.tragent.yml`](gitlab/.tragent.yml).

## Support

Open an [issue](https://github.com/tragent-io/cli/issues) or email
[hellotragent@gmail.com](mailto:hellotragent@gmail.com).
