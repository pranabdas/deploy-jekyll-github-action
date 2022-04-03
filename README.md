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

- For the first time we will deploy the website via built-in github pages
service. Create a new branch and name it to `gh-pages`.

![create-gh-pages-branch.png](./assets/create-gh-pages-branch.png)

- This will trigger, github-pages to deploy to
<https://pranabdas.github.io/deploy-jekyll-github-action>. You can head over and
check it out.

- Now let's add our GitHub Actions to deploy automatically, every time we push
out code to `main` branch.

- Create our workflow file:
```console
mkdir -p .github/workflows
vi .github/workflows/deploy-gh-pages.yml
```

- Add following content to our workflow:
```yaml
name: Deploy gh-pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}

    steps:
      - uses: actions/checkout@v3

      - name: Prepare Ruby and Jekyll
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: '2.7'
          bundler-cache: true

      - name: Build website
        run: bundle exec jekyll build

      - name: Deploy on gh-pages
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_site
          user_name: 'github-actions[bot]'
          user_email: 'github-actions[bot]@users.noreply.github.com'
```

- Commit and push changes to github.
