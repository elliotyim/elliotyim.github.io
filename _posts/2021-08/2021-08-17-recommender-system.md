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

## Content-based Filtering

- Content-based filtering은 유저의 활동으로부터 유저가 무엇을 좋아할지 예상해보는 추천 시스템이다
  - 유저의 활동과 구입목록, rating(좋아요와 싫어요 등), 검색한 아이템, 장바구니의 아이템, 조회한 아이템 등을 토대로 유저 프로파일링을 한다
- Content-based filtering은 제품이나 서비스, 콘텐츠등에 의존하고 있다
- 유저 피드백이 중요하게 작용한다
- 추천에 있어 다른 유저의 데이터를 필요로 하지 않는다
- 추천 시스템이 충분한 정보가 수집된 상태가 아니라면 유저들에게 적절한 제품을 추천해주지 못하는 Cold Start 문제가 생길 수 있다
- 기존의 유저의 기호에 따라 판단하기 때문에 예상하지 못한 유저의 기호를 파악하여 추천해줄 수 없다

## Collaborative Filtering

- Collaborative filtering은 다른 유사한 유저들의 reaction을 토대로 한 유저가 좋아할만한 아이템들을 추천해주는 기법이다

### The Dataset

- reaction은 explicit(1~5까지의 평점 또는 좋아요나 싫어요 등)과 implicit(어떤 아이템 조회, 위시리스트에 추가, 아이템을 보는데 들인 시간 등)한 부분으로 나뉜다
- dataset을 다루는데에 보통 각 아이템에 대한 유저 그룹의 reaction을 표시한 matrix를 사용한다

![](/assets/images/recommender-system/rating-matrix.jpg)

- 모든 유저가 모든 아이템에 대해 평점을 매기는 것은 흔치 않기 때문에 비슷한 그룹의 유저의 평가들을 토대로 추측을 해야한다

# References

- https://www.researchgate.net/figure/Content-based-filtering-vs-Collaborative-filtering-Source_fig5_323726564
- https://www.upwork.com/resources/what-is-content-based-filtering
- https://techblog-history-younghunjo1.tistory.com/166
