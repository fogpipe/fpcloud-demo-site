# fpcloud-demo-site

A static page that deploys to FP Cloud on every push to `main`, using GitHub
OIDC. Nothing is stored in GitHub: no keys, no secrets.

## One-time setup

```bash
fpcloud --project demo sa create deployer

fpcloud --project demo iam set \
  --member serviceAccount:deployer@demo.yqw.cloud.fogpipe.com --role editor

fpcloud --project demo federation github add \
  --repo fogpipe/fpcloud-demo-site \
  --service-account deployer@demo.yqw.cloud.fogpipe.com
```

## Demo

Edit `public/index.html`, push to `main`, and open
https://site-demo-yqw-web.fogpipe.cloud. The page shows the commit it was
deployed from.

```bash
fpcloud --project demo website versions site   # every deploy
fpcloud --project demo website rollback site   # back to the previous one
```
