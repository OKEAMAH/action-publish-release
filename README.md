# MetaMask/action-publish-release

This Action can be used on its own, but was designed to be combined with [MetaMask/action-create-release-pr](https://github.com/MetaMask/action-create-release-pr).
Using the following the workflow file, this Action will run whenever a PR created by `action-create-release-pr` is merged.

`.github/workflows/publish-release.yml`

```yaml
name: Publish Release

on:
  pull_request:
    types: [closed]

jobs:
  release_merge:
    if: |
      github.event.pull_request.merged == true &&
      startsWith(github.event.pull_request.head.ref, 'release-v')
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Get Node.js version
        id: nvm
        run: echo ::set-output name=NODE_VERSION::$(cat .nvmrc)
      - uses: actions/setup-node@v2
        with:
          node-version: ${{ steps.nvm.outputs.NODE_VERSION }}
      - uses: MetaMask/action-publish-release@v0.0.3
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
