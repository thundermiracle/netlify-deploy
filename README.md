# Netlify Deploy

Github action to deploy to Netlify.

## Usage

Add one step to your jobs.

```yaml
jobs:
  deploy:
     steps:
       - name: deploy to preview mode
         uses: thundermiracle/netlify-deploy@v0.1.0
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

## License

This project is licensed under the terms of the [MIT license](/LICENSE).