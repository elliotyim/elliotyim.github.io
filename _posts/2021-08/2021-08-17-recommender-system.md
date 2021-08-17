---
layout: post
title: Recommender System
description: Recommender System
image: /assets/images/recommender-system/Content-based-filtering-vs-Collaborative-filtering-Source.png
date: 2021-08-17 9:30
---

# Recommender System

![recommender-system](/assets/images/recommender-system/Content-based-filtering-vs-Collaborative-filtering-Source.png)

- 추천 시스템에는 크게 Content-based filtering과 Collaborative filtering의 두 가지가 있고 이를 적절하게 혼합한 것이 Hybrid recommender system이라고 한다

## Content-based filtering

- Content-based filtering은 유저의 활동으로부터 유저가 무엇을 좋아할지 예상해보는 추천 시스템이다
  - 유저의 활동과 구입목록, rating(좋아요와 싫어요 등), 검색한 아이템, 장바구니의 아이템, 조회한 아이템 등을 토대로 유저 프로파일링을 한다
- Content-based filtering은 제품이나 서비스, 콘텐츠등에 의존하고 있다
- 유저 피드백이 중요하게 작용한다
- 추천에 있어 다른 유저의 데이터를 필요로 하지 않는다
- 추천 시스템이 충분한 정보가 수집된 상태가 아니라면 유저들에게 적절한 제품을 추천해주지 못하는 Cold Start 문제가 생길 수 있다
- 기존의 유저의 기호에 따라 판단하기 때문에 예상하지 못한 유저의 기호를 파악하여 추천해줄 수 없다

# References

- https://www.researchgate.net/figure/Content-based-filtering-vs-Collaborative-filtering-Source_fig5_323726564
- https://www.upwork.com/resources/what-is-content-based-filtering
- https://techblog-history-younghunjo1.tistory.com/166
