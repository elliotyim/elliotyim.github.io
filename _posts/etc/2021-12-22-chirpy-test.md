---
title: Chirpy 테마 삽질기
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2021-12-22 16:25:00 +0900
categories: [잡담]
tags: [jekyll, chirpy]
---

Chirpy 테마 자체는 정말 깔끔하고 좋은데, 내가 난독증이 있는건지 README.md를 읽어도 이해가 안되서 일단 그냥 막 해보다가 삽질을 엄청 많이 했다.

- 배포하는 브랜치를 gh-pages에서 master로 바꿨더니 아래와 같이 빌드가 제대로 안된 화면이 나왔다.
  ![t](/assets/img/etc/001.png)
  - jekyll 블로그가 빌드가 제대로 안되었거나, 무언가 충돌이 일어난 것 같다. 이래서 빌드하는 파일을 다른 브랜치로 하고 그 브랜치로 서빙하는 것이 안전하다고 readme에 적혀 있나보다. (처음에는 이게 무슨말인지 이해를 못했음)
- 카테고리랑 태그 써놓은 부분이 갱신이 안되길래 검색을 하다보니 이런 글을 발견했다.
  - [https://github.com/cotes2020/jekyll-theme-chirpy/issues/70](https://github.com/cotes2020/jekyll-theme-chirpy/issues/70)
  - 처음에 init.sh를 실행해준 것 같은데 아마 위에서 master 브랜치로 뭔가 잘못하다가 다시 날리고 만들 때 init.sh 실행하는 걸 깜빡해서 일어난 일인 것 같다. 위의 이슈 댓글처럼 글을 작성할 때마다 init.sh를 실행할 필요는 없고, 처음 블로그를 만들 때 한 번만 실행하면 되는 것 같다.

아직은 좀 익숙하지 않은 블로깅이지만, 조금씩 나아지겠지...........?
