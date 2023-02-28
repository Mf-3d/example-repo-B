自動コミットの実装するにはこのようなGitHub Actionsを実行します。

`release.yml`
```yaml 
name: Release Workflow

on:
  release:
    types: [released]

jobs:
  build_docs:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout A code
        uses: actions/checkout@v2
        with:
          ref: ${{ github.event.release.tag_name }}

      - name: Copy A code to B
        run: |
          mkdir ../B/A
          cp -r * ../B/A

      - name: Checkout B code
        uses: actions/checkout@v2
        with:
          repository: owner/B
          ref: main

      - name: Install dependencies and build
        uses: octopus-app/action-docusaurus@v1
        with:
          install-command: yarn install
          build-command: yarn build
          docs-dir: A/docs

      - name: Commit and push docs changes
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit-message: "📝 update docs for release ${{ github.event.release.tag_name }}!"
          commit-user-name: "thinkerAI bot"
          commit-user-email: "actions@github.com"
```

# Website

This website is built using [Docusaurus 2](https://docusaurus.io/), a modern static website generator.

### Installation

```
$ yarn
```

### Local Development

```
$ yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Build

```
$ yarn build
```

This command generates static content into the `build` directory and can be served using any static contents hosting service.

### Deployment

Using SSH:

```
$ USE_SSH=true yarn deploy
```

Not using SSH:

```
$ GIT_USER=<Your GitHub username> yarn deploy
```

If you are using GitHub pages for hosting, this command is a convenient way to build the website and push to the `gh-pages` branch.
