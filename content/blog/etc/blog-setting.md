---
title: 'Gatsby blog Setting'
date: 2021-03-25 20:00:00
category: 'etc'
draft: false
---

gatsby 블로그 생성하면서 겪은 에러 정리하고자 한다.

## template 셋팅

template 셋팅 과정에서 node버전 16.x 버전을 사용중에 있어 npm install과정에서 node-sass 버전을 호환하지 않아 오류가 지속되었다.

pacakge.joson의 depndency 업데이트 해주었다.

```
npm install -g npm-check-updates
ncu -u
npm install
```

## Github Pages로 Deploy

github pages로 호스팅
gh-pages 브랜치를 만들어 준후 deploy해주어야 main에 build된 내용들이 반영되지않는다.

```
install --save-dev gh-pages
"script: {
  "deploy": "gatsby build && gh-pages -d public"
}
```

## Github Actions로 배포자동화하기

이제 gh-pages브랜치로 포스팅하고 main브랜치에 머지 시 배포되게 셋팅해주었다.

- github actions token 생성
- Repository에 Token할당하기

New workflow생성 후 main.yml을 작성한다.
로컬에서 node 버전을 16버전을 사용중에 있어 빌드시 node-version을 14.x로 변경해주었다.

```
name: devlog deploy
on:
  only for the main branch
  push:
    branches: [main]

sequentially or in parallel
jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [14.x]
    as part of the job
    steps:
      your job can access it
      - uses: actions/checkout@v2

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}

      - name: Run a one-line script
        run: echo Hello, world!

      - name: npm install
        run: npm install

      - name: gatsby build
        env:
          GH_API_KEY: ${{ secrets.GITHUB }}
        run: npm run build

      - name: deploy
        uses: maxheld83/ghpages@v0.2.1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GH_PAT: ${{ secrets.GITHUB }}
          BUILD_DIR: 'public/'
```
