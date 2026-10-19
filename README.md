# paolobussacchini.it

Personal CV site (Jekyll + Minima).

## Local preview

```powershell
docker compose up
```

Open http://127.0.0.1:4000

`bundle install` runs inside the container (uses gems from `jekyll/jekyll:4`), then
`bundle exec jekyll serve` with live reload.

GitHub Pages deploy uses `.github/workflows/pages.yml` (`actions/jekyll-build-pages`).
