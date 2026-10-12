# fpcloud-demo-site

A static page that deploys to FP Cloud on every push to `main`, using GitHub
OIDC. No keys are stored in GitHub.

## One-time setup

Assumes you're logged in (`fpcloud login`) and the `demo` project exists.

```bash
# 1. A service account for CI, allowed to deploy to the project
fpcloud --project demo sa create deployer
SA=$(fpcloud --project demo sa list -o json | jq -r '.[] | select(.name=="deployer") | .id')
fpcloud --project demo iam set --member serviceAccount:$SA --role editor

# 2. Trust this repo's GitHub Actions to act as it
fpcloud --project demo federation github add --repo fogpipe/fpcloud-demo-site --service-account $SA

# 3. Tell the workflow which account to use (an id, not a secret)
gh variable set FPCLOUD_DEPLOY_SA --repo fogpipe/fpcloud-demo-site --body $SA
```

## Demo

Edit `public/index.html`, push to `main`, then open
https://site-demo-yqw-web.fogpipe.cloud. The page shows the commit it was
deployed from.

```bash
fpcloud --project demo website versions site   # every deploy
fpcloud --project demo website rollback site   # back to the previous one
```
