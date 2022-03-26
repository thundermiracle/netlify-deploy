# Netlify Deploy

Github action to deploy to Netlify.

## Usage

Add one step to your jobs.

※. This action uses `yarn` as default dependency manager. If you want to use another dependency manager, you need to override each input.

```yaml
jobs:
  deploy:
     steps:
       - name: deploy to preview mode
         uses: thundermiracle/netlify-deploy@v1
         with:
           NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN}}
           NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID}}
           deploy_dir: "./public"
           production: false
           node: ${{ matrix.node }}
```

## Options

| input | required | default | description |
| :--- | :--: | :-: | :--- |
| NETLIFY_AUTH_TOKEN | ○ |  | Auth token of your Netlify, usually passed by [`secrets`](https://docs.github.com/en/actions/security-guides/encrypted-secrets) |
| NETLIFY_SITE_ID | ○ |  | Your website id in Netlify, usually passed by [`secrets`](https://docs.github.com/en/actions/security-guides/encrypted-secrets) |
| deploy_dir |  | ./dist | Directory to be uploaded to Netlify |
| production |  | false | Deploy to production mode flag |
| node |  | 14 | Node version to run deployment |
| build_command | △ | yarn build | |
| install_command | △ | yarn --check-files --frozen-lockfile --non-interactive | ※ You should override it if you're not using yarn as your dependency manager |
| cache_path | | node_modules | |
| cache_strategy | △ | yarn | `yarn`, `npm`, or `pnpm` |

## License

This project is licensed under the terms of the [MIT license](/LICENSE).