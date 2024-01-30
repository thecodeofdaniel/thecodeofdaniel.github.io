# How to get Started

## Download Congo Theme

- Make sure golang is installed

  ```bash
  # If you're managing your project on GitHub
  hugo mod init github.com/<username>/<repo-name>

  # If you're managing your project locally
  hugo mod init my-project
  ```

- Declare theme in `config/_default/module.toml`

  ```bash
  [[imports]]
  path = "github.com/jpanther/congo/v2"
  ```

  - Update the theme

    ```bash
    hugo mod get -u
    ```
  
  - You may need to run this to clear local cache

    ```
    hugo mod clean
    ```

- Setup theme config files (download [here](https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/jpanther/congo/tree/stable/config/_default))

  ```bash
  config/_default/
  ├─ config.toml
  ├─ markup.toml
  ├─ menus.toml
  ├─ module.toml  # if you installed using Hugo Modules
  └─ params.toml
  ```

## Using GitHub Pages

- Create this file in `github/workflows/gh-pages.yml`

  ```yml
  # .github/workflows/gh-pages.yml

  name: GitHub Pages

  on:
    push:
      branches:
        - main

  jobs:
    build-deploy:
      runs-on: ubuntu-latest
      concurrency:
        group: ${{ github.workflow }}-${{ github.ref }}
      steps:
        - name: Checkout
          uses: actions/checkout@v3
          with:
            submodules: true
            fetch-depth: 0

        - name: Setup Hugo
          uses: peaceiris/actions-hugo@v2
          with:
            hugo-version: "latest"
            extended: true

        - name: Build
          run: hugo --minify

        - name: Deploy
          uses: peaceiris/actions-gh-pages@v3
          if: ${{ github.ref == 'refs/heads/main' }}
          with:
            github_token: ${{ secrets.GITHUB_TOKEN }}
            publish_branch: gh-pages
            publish_dir: ./public
  ```

- Make sure base URL follows the one on github in `hugo.toml`

- Change version of golang to go from `x.x.x` to `x.x`

- Change workflow permissions to `Read and write permissions` and check `Allow GitHub Actions to create and approve pull requests`

- Turn off then on `Deploy from branch`. It WILL FAIL the first time. However, this will create a branch named `gh-pages`

- Switch from `main` to `gh-pages` branch (It should be successful)
