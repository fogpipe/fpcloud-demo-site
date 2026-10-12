# fpcloud-demo-site

A static page that deploys to FP Cloud on every push to `main`, using GitHub
OIDC. Nothing is stored in GitHub: no keys, no secrets.

## One-time setup

```bash
fpcloud --project demo sa create deployer

fpcloud --project demo iam set \
  --member serviceAccount:deployer@demo.yqw.cloud.fogpipe.com --role editor

fpcloud --project demo federation github add \
  --repo fogpipe/fpcloud-demo-site --ref refs/heads/main \
  --service-account deployer@demo.yqw.cloud.fogpipe.com
```

## Demo

Edit `public/index.html` and push to `main`. The run's summary links the
live site, which shows the commit it was deployed from. Only `main` can
deploy: pull request runs are not trusted.

```bash
fpcloud --project demo website versions site   # every deploy
fpcloud --project demo website rollback site   # back to the previous one
```
