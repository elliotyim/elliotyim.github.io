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

---

- Content-based filtering은 유저의 활동으로부터 유저가 무엇을 좋아할지 예상해보는 추천 시스템이다
  - 유저의 구입목록, rating(좋아요와 싫어요 등), 검색한 아이템, 장바구니의 아이템, 조회한 아이템 등을 토대로 유저 프로파일링을 한다
- Content-based filtering은 제품이나 서비스, 콘텐츠등에 의존하고 있다
- 유저 피드백이 중요하게 작용하며, 추천에 있어 다른 유저의 데이터를 필요로 하지 않는다
- 추천 시스템이 충분한 정보가 수집된 상태가 아니라면 유저들에게 적절한 제품을 추천해주지 못하는 Cold Start 문제가 생길 수 있다
- 기존의 유저의 기호에 따라 판단하기 때문에 예상하지 못한 유저의 기호를 파악하여 추천해줄 수 없다

## Collaborative Filtering

---

- Collaborative filtering은 다른 유사한 유저들의 reaction을 토대로 한 유저가 좋아할만한 아이템들을 추천해주는 기법이다

### Collaborative Filtering 적용

---

- Collaborative Filtering을 적용하기 위해서는 아래의 두 가지 스텝을 밟는다
  - 다른 유저들의 취향이 반영된 아이템들을 토대로 추천을 하기에 앞서 유사한 취향의 유저의 그룹을 찾는다
  - 한 유저가 아직 평가를 하지 않은 아이템들에 대한 평가를 예상한다
- 이를 위한 고려사항은 아래와 같다
  - 어떻게 유사한 취향의 유저들을 묶을 것인가?
  - 유저 그룹에 속한 한 유저가 어떻게 평가를 내릴 것인가?
  - 평가에 대한 정확도를 어떻게 측정할 것인가?
- Collaborative Filtering 만을 사용한 접근은 유저의 나이나 영화의 장르, 또는 유저나 아이템에 대한 어떤 데이터가 유사도 측정의 요인이 될 수는 없다
  - 오직 유저의 아이템에 대한 평가(explicit 하거나 implicit한 것)로만 판단이 된다
  - ex) 나이 차이가 많이 나는 두 유저가 같은 영화를 10점으로 평가했다면 이 유저들은 유사한 그룹으로 고려되어진다

### The Dataset

---

![](/assets/images/recommender-system/rating-matrix.jpg)

- reaction은 명시적인 부분(1~5까지의 평점 또는 좋아요나 싫어요 등)과 암시적인 부분(어떤 아이템 조회, 위시리스트에 추가, 아이템을 보는데 들인 시간 등)으로 나뉜다
- dataset을 다루는데에 보통 각 아이템에 대한 유저 그룹의 reaction을 표시한 matrix를 사용한다

### Memory-based

---

- Memory-based 방식은 한 유저가 아직 평가하지 않은 아이템을 다른 유저들이 평가한 점수로 예측하는 `User-based` 방식과 한 유저가 내린 아이템들을 가지고 평가를 예측하는 `Item-based` 방식으로 나뉜다
- 간단한 예시로, 위의 그림의 경우 u4와 u5가 i2에 대해 똑같이 4점으로 평점을 줬으므로 u4의 i4에 대한 평가는 u5와 비슷한 5점일 것이라고 추정하는 것이다 (User-based)

**User-based**

![](/assets/images/recommender-system/user-based.png)

- user 간의 유사도(같은 성별, 연령대 등)를 측정한 뒤, 유사도가 높은 user들이 내린 평가를 종합하여 평가를 예측한다
- 자세한 내용은 https://www.slideshare.net/xamat/recommender-systems-machine-learning-summer-school-2014-cmu 의 슬라이드 35p~38p를 참조

**Item-based**

![](/assets/images/recommender-system/item-based.png)

- user-based와 같은데 target만 다르다. Item-based에서는 영화 간의 유사도(같은 장르나 시리즈, 감독 등)를 측정한 뒤, 비슷한 영화에 대해 한 평가를 토대로 추측한다
- 자세한 내용은 https://www.slideshare.net/xamat/recommender-systems-machine-learning-summer-school-2014-cmu 의 슬라이드 47p~52p를 참조

### Model-based

---

- user-item matrix를 통해 모델을 학습하는 방법이다. matrix로부터 모델이 학습하고 나면 추천을 할 때는 모델만 있으면 된다
- 데이터셋 전체를 사용해서 추천 하는 것이 아니라 모델만을 사용하기 때문에 추천이 정확하지 않을 수 있다
- 모델의 종류는 Baysian Network, Decision Tree, Latent Factor Models, MDP등이 있다

**Matrix Factorization**

![Latent Factor Approach](/assets/images/recommender-system/LatentFactorApproach.png)

- MF는 latent factor 모델의 일종으로, item과 user를 20-100 정도의 vector로 표현한다
- item을 제목이나 장르, 가수 등으로 표현한 content-based와 달리 latent factor 모델에서는 item을 interaction pattern으로 학습된, 기계가 이해하는 어떤 특징으로 표현한다
- interaction 정보를 포함하기 때문에 풍부하게 item이나 user를 표현할 수 있다
-

### Cosine Similarity

---

[https://needjarvis.tistory.com/665](https://needjarvis.tistory.com/665)

# References

- https://www.researchgate.net/figure/Content-based-filtering-vs-Collaborative-filtering-Source_fig5_323726564
- https://www.upwork.com/resources/what-is-content-based-filtering
- https://techblog-history-younghunjo1.tistory.com/166
- https://realpython.com/build-recommendation-engine-collaborative-filtering
- https://needjarvis.tistory.com/665
- https://data-matzip.tistory.com/entry/Collaborative-FilteringCF-협업필터링1-특징-개념-수정필요
- https://www.slideshare.net/xamat/recommender-systems-machine-learning-summer-school-2014-cmu
- https://blossominkyung.com/archives/collaborative-filtering
- https://dnddnjs.github.io/recomm/2019/08/15/recommendation_system_basics/
