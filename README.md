# Netlify Deploy with Comments

Github action to deploy to Netlify with Comments of Preview and Log URL.

## Table of contents

* [Usage](#usage)
* [Options](#options)
* [What's `NEW` in this Github action?](#whats-new-in-this-github-action)
* [Examples](#examples)
  * [GatsbyJS with yarn](#gatsbyjs-with-yarn)
  * [GatsbyJS with npm](#gatsbyjs-with-npm)
* [License](#license)

## Usage

1. Add your Netlify `Personal Access Token` & Web site's `Site ID` to your repository's secrets.

    [https://docs.netlify.com/cli/get-started/#obtain-a-token-in-the-netlify-ui](https://docs.netlify.com/cli/get-started/#obtain-a-token-in-the-netlify-ui)


2. Add one step to your Github's jobs.

    ※. This action uses `yarn` as default dependency manager. If you want to use another dependency manager, you need to override each input.

    Following settings will deploy your site to preview mode in Netlify and add a comment to the pull request with the URL of the preview and the log.

    ![comment in pull request](./assets/comment_in_pr.png)

    ```yaml
    jobs:
      deploy:
        steps:
          - name: deploy to preview mode
            uses: thundermiracle/netlify-deploy@v3.0.0
            with:
              NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN}}
              NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID}}
              deploy-dir: "./public"
              production: false
              node: 14
    ```

    ※. For production mode, you need to add `production: true` to the `with` section.

## Options

| input | required | default | description |
| :--- | :--: | :-: | :--- |
| NETLIFY_AUTH_TOKEN | ○ |  | Auth token of your Netlify, usually passed by [`secrets`](https://docs.github.com/en/actions/security-guides/encrypted-secrets) |
| NETLIFY_SITE_ID | ○ |  | Your website id in Netlify, usually passed by [`secrets`](https://docs.github.com/en/actions/security-guides/encrypted-secrets) |
| deploy-dir |  | ./dist | Directory to be uploaded to Netlify |
| production | △ | false | Deploy to production mode flag |
| node |  | 14 | Node version to run deployment |
| build-command | △ | yarn build | |
| install-command | △ | yarn --check-files --frozen-lockfile --non-interactive | ※ You should override it if you're not using yarn as your dependency manager |
| cache-strategy | △ | yarn | `yarn`, `npm`, or `pnpm` |
| extra-cache-path | | | extra cache paths(eg: `.cache` in GatsbyJS) |
| working-directory | | | Working directory of your project |

## What's `NEW` in this Github action?

1. Automatically cache dependencies of your project.

    You can override `cache-strategy` to customize the cache.
2. Cache extra paths for specific projects.

    You can override `extra-cache-path` to enable extra cache policy(eg: `.cache` & `public` folders for `GatsbyJS`).
3. Add comment with Preview URL and Log URL to your pull request automatically.

    ![comment in pull request](./assets/comment_in_pr.png)
4. `working-directory` is supported.

    To deploy your project in sub directory easily, just set `working-directory` like `./packages/my-package`.

## Examples

### GatsbyJS with yarn

1. For Pull Request, create a `.github/workflows/preview.yml` file with the following content.

    ```yaml
    name: 'Netlify Preview Deploy'

    on:
      pull_request:
        types: ['opened', 'edited', 'synchronize']

    jobs:
      deploy-preview:
        name: 'Deploy Preview'
        runs-on: ubuntu-latest

        steps:
          - name: deploy to preview mode
            uses: thundermiracle/netlify-deploy@v3.0.0
            with:
              NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN}}
              NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID}}
              deploy-dir: "./public"
              extra-cache-path: |
                .cache
                public
    ```

2. For production build, create a `.github/workflows/production.yml` file with the following content.

    ```yaml
    name: 'Netlify Production Deploy'

    on:
      push:
        branches:
          - main

    jobs:
      deploy:
        name: 'Deploy Production'
        runs-on: ubuntu-latest

        steps:
          - name: deploy to production mode
            uses: thundermiracle/netlify-deploy@v3.0.0
            with:
              NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN}}
              NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID}}
              deploy-dir: "./public"
              extra-cache-path: |
                .cache
                public
              production: true
    ```
### GatsbyJS with npm

1. For Pull Request, create a `.github/workflows/preview.yml` file with the following content.

    ```yaml
    name: 'Netlify Preview Deploy'

    on:
      pull_request:
        types: ['opened', 'edited', 'synchronize']

    jobs:
      deploy-preview:
        name: 'Deploy Preview'
        runs-on: ubuntu-latest

        steps:
          - name: deploy to preview mode
            uses: thundermiracle/netlify-deploy@v3.0.0
            with:
              NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN}}
              NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID}}
              deploy-dir: "./public"
              extra-cache-path: |
                .cache
                public
              cache-strategy: npm
              install-command: npm install --production
              build-command: npm run build
    ```

2. For production build, create a `.github/workflows/production.yml` file with the following content.

    ```yaml
    name: 'Netlify Production Deploy'

    on:
      push:
        branches:
          - main

    jobs:
      deploy:
        name: 'Deploy Production'
        runs-on: ubuntu-latest

        steps:
          - name: deploy to production mode
            uses: thundermiracle/netlify-deploy@v3.0.0
            with:
              NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN}}
              NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID}}
              deploy-dir: "./public"
              extra-cache-path: |
                .cache
                public
              production: true
              cache-strategy: npm
              install-command: npm install --production
              build-command: npm run build
    ```

## License

This project is licensed under the terms of the [MIT license](/LICENSE).
