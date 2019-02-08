# blog.prskavec.net

[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fabtris%2Fblog.prskavec.net-hugo.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fabtris%2Fblog.prskavec.net-hugo?ref=badge_shield)

[![Netlify Status](https://api.netlify.com/api/v1/badges/68b9a7d5-adb6-438d-aab1-f5796a874798/deploy-status)](https://app.netlify.com/sites/reporter-wax-30177/deploys)

## Requirements

- Hugo

```
brew install hugo
```


## Create new post

```
hugo new post/`date +%Y-%m-%d`-{{name}}.markdown
```


## Local preview

```
hugo server
```

You can look at preview on [https://localhost:1313](https://localhost:1313).

## Publish

1. Create Git branch with new content
2. Open PR in Github
3. Netlify publish PR as new url for review.
4. Make review and merge

## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fabtris%2Fblog.prskavec.net-hugo.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fabtris%2Fblog.prskavec.net-hugo?ref=badge_large)
