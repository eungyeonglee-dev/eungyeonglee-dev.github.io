---
title: "jekyll blog pseudocode 작성 방법"
excerpt: ""

categories:
    - howto
tags:
    - blog
    - tip
    - dairy
---

# PSEUDOCODE JEKYLL BLOG에 작성하기

알고리즘 공부하면서 정리한 내용을 jekyll 기반 블로그에 업로드 하려는데 pseudocode도 함께 작성하여 올리고 싶었다.

`을 세번쓰는 방식은 괜찮긴 하지만 정식 방법은 아니기에 제대로 된 pseudocode format을 올릴 수 있는 방법을 찾아보았다.

역시나 이미 해놓은 사람은 있었고 readme를 따라 적용했다.

[pseudocode-b](https://github.com/tobiasbu/jekyll-pseudocode-b)

위 pseudocode-b의 원형이 되는 라이브러리(라고 해야하나)도 있었다.

[pseudocode](https://github.com/baites/jekyll-pseudocode)

일단 나의 local github에서 jekyll-pseudocode-b를 설치한다.

```
gem install jekyll-pseudocode-b
```

그리고 최상위 폴더에서 Gemfile을 찾아 다음 코드를 붙여준다.

```
gem 'jekyll-pseudocode-b'
```

_config.yml 파일의 plusins: 를 찾아 아래 항목을 추가해주고

```
plugins:
  - jekyll-pseudocode-b
```

```
bundle install
bundle exec jekyll serve
```

하니 적용은 된다. css를 변경하고 싶은데 아직 더 알아봐야할 것 같고,
결과를 보니 띄어쓰기가 적용이 안된다. 이유가 뭘까.. 
