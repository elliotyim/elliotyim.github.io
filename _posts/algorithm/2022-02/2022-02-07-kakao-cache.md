---
title: 2018 KAKAO BLIND RECRUITMENT [1차] 캐시
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-07 18:28:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "카카오", "블라인드", "큐"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/17680](https://programmers.co.kr/learn/courses/30/lessons/17680)

## Introduction

큐를 사용하면 정말 간단하게 풀 수 있는 문제이다.

Level 2로 분류될 문제는 아닌듯

## Note

- 큐를 사용하여 캐시의 사용빈도를 관리한다.

## Solution

```python
from collections import deque

def solution(cacheSize, cities):
    if cacheSize == 0:
        return 5 * len(cities)

    answer, cache = 0, deque()
    for c in cities:
        city = c.lower()
        if city in cache:
            cache.remove(city)
            cache.append(city)
            answer += 1
            continue
        elif len(cache) == cacheSize:
            cache.popleft()
        cache.append(city)
        answer += 5

    return answer
```

**풀이**

간단한 문제이다. cities 리스트를 순회하면서 city가 캐쉬에 있으면 갱신하고 없으면 캐쉬 사이즈를 체크한 다음에 넘으면 가장 오래된걸 버리고 캐쉬 큐에 집어넣는다.

처음에는 cache_set을 만들어 해당 city가 cache 안에 있는지를 O(1)로 체크하려고 했는데, n이 30으로 굉장히 작아서 그냥 더 짧은 코드로 변경했다.

cache를 갱신하는 부분(line 8, 9)을 처음에는 직접 구현했다가 저렇게 두 줄로 쓰는거나 큰 차이가 없을 것 같아서 더 짧은 쪽으로 선택했다. (지금 다시 보면 그 구현한 부분 너무 심각했다;;)

이 문제는 파이썬 deque에 maxLen 기능을 알고 있다면 더 짧게 풀 수 있는 문제였다.

아래는 maxLen 기능을 사용하여 더 짧게 푼 다른 사람의 풀이이다.

```python
def solution(cacheSize, cities):
    import collections
    cache = collections.deque(maxlen=cacheSize)
    time = 0
    for i in cities:
        s = i.lower()
        if s in cache:
            cache.remove(s)
            cache.append(s)
            time += 1
        else:
            cache.append(s)
            time += 5
    return time
```

## Comment

좀 더 사용하고 있는 기본 라이브러리에 대해 자세하게 공부하고 어떤 기능들이 있는지를 배워야겠다.
