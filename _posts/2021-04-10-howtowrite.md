---
title: "블로그 포스팅 방법"
excerpt: ""

categories:
    - howto
tags:
    - howto
    - blog
use_math: false
---

로컬에서 github 블로그 실행시키기
---
MAC OS , terminal

```
$ cd ~\로컬파일이 있는 폴더 경로
$ bundle exec jekyll serve
```

접속 후 localhost:4000 주소로 들어가 확인한다.

github에 업로드하기
---
MAC OS, terminal

```
$ git add .
$ git commit -m "커밋저장내용"
$ git push -u origin master
```
