# paolobussacchini.it

Sito personale e blog (Jekyll + Minima), pubblicato su **GitHub Pages** con dominio custom `paolobussacchini.it`.

- Repo: `paolobussacchini/paolobussacchini.github.io`
- URL: https://paolobussacchini.it

## Sviluppo locale (opzionale)

Serve Ruby + Bundler (WSL o RubyInstaller):

```bash
bundle install
bundle exec jekyll serve
```

Apri http://127.0.0.1:4000

In assenza di Ruby: modifica i Markdown e fai push; la build avviene in GitHub Actions (`.github/workflows/pages.yml`).

## Nuovo post

Crea `_posts/YYYY-MM-DD-titolo-con-trattini.md` con front matter:

```yaml
---
layout: post
title: "Titolo"
date: YYYY-MM-DD
categories: [homelab]
tags: []
---
```

Poi `git commit` + `git push` su `main`.

## Dominio / DNS

File `CNAME` → `paolobussacchini.it`.

Sul DNS del registrar:

| Tipo | Nome | Valore |
|------|------|--------|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |
| CNAME | `www` | `paolobussacchini.github.io` |

Poi: GitHub → Settings → Pages → Custom domain `paolobussacchini.it` → Enforce HTTPS.

Verifica sempre gli IP aggiornati sulla [documentazione GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).

## Cosa non pubblicare

- Secret, token, chiavi private, kubeconfig
- Materiale interno aziendale / clienti
- Path e URL non pubblici del lab di lavoro

I post sono versioni **sanitizzate** di appunti personali.
