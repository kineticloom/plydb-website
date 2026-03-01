# Public website for PlyDB

This repo contains the marketing and documentation site for the
[PlyDB](https://github.com/kineticloom/plydb) open source project.

It is built with [Hugo](https://gohugo.io/), a static site generator.

## Develop

Pre-reqs:

- [Hugo](https://gohugo.io/)

Run dev server

```sh
hugo server --buildDrafts --disableFastRender --source src --destination $PWD/dist
```

## Updating the theme

We use [Hextra](https://github.com/imfing/hextra) as our Hugo theme. We fully
vendor the theme in the repo instead of using git submodules.

```sh
rm -rf src/themes/hextra

git clone https://github.com/imfing/hextra.git src/themes/hextra

# save hextra commit sha for reference
git -C src/themes/hextra rev-parse HEAD > hextra-version

# remove hextra git repo
rm -rf src/themes/hextra/.git
```
