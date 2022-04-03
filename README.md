# Deploy Jekyll 4.x.x via GitHub Actions

- Create a brand new Jekyll site:
```console
jekyll new deploy-jekyll-github-action
```

- We need to take care of few things in the `_config.yml` to deploy the site in
GitHub pages. Change the `url` and `baseurl`. In my case:
```yaml
baseurl: "/deploy-jekyll-github-action"
url: "https://pranabdas.github.io"
```

- Let's commit and push the code to github:
```console
git init
git branch -m main
git remote add origin https://github.com/pranabdas/deploy-jekyll-github-action.git
git add --all
git commit -m "first commit"
git push -u origin main
```
