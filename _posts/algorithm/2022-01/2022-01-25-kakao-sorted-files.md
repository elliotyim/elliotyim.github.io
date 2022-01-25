---
title: 2018 KAKAO BLIND RECRUITMENT [3차] 파일명 정렬
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-25 18:32:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags:
  [
    "알고리즘",
    "프로그래머스",
    "2018",
    "카카오",
    "블라인드",
    "정규표현식",
    "정렬",
  ]
---

## Link

- https://programmers.co.kr/learn/courses/30/lessons/17686

## Introduction

세상엔 머리 좋은 사람들이 많은 것 같다. 해결해야 하는 문제를 보고 발상 자체를 바꿔서 다른 방식으로 해석하고 더 인텔리전트한 방법으로 풀어나가는 걸 보면 정말 경이롭기까지 하다.

간단한 구현 문제였던 이 문제를 가져온 이유는 평소 약했던 정규표현식을 제대로 다시 공부하여 짧고 깔끔한 풀이를 하고 이를 기억하기 위함이었다.

꽤 명료하고 짧은 코드를 작성한 뒤 자신에 차 다른 사람의 풀이를 확인하던 내가 발견한건 단 2줄짜리 코드...심지어 수행시간도 큰 차이가 없다.

나도 언젠간 저런 발상을 역전한 코드를 짜야지

## Note

- 대소문자를 구별하지 않는 점에 주의
- head에 공백(" ")과 빼기 부호("-")가 포함될 수 있다는 조건에 주의.
  - head가 `F-`가 될 수도 있고 `F- `가 될 수도 있다.

## Solution

1. 파일명에서 `HEAD`와 `NUMBER`를 나눠주고 정렬 기준에 따라 힙에 넣어 자동으로 정렬되며 차곡차곡 쌓이도록 한다.
2. heap에 쌓인 순서대로 파일명만 꺼내어 정답리스트에 넣고 반환한다.

```python
import re, heapq

def solution(files):
    answer, heap = [], []

    for i, file in enumerate(files):
        split_file = file.split('.')[0]
        head, no = re.findall('^([A-z\-\s]+)(\d+)', split_file)[0]
        head, no = head.lower(), int(no)
        heapq.heappush(heap, [head, no, i, file])

    while heap:
        answer.append(heapq.heappop(heap)[3])

    return answer
```

그리고 아래가 바로 그 문제의 풀이다..

```python
import re

def solution(files):
    a = sorted(files, key=lambda file : int(re.findall('\d+', file)[0]))
    b = sorted(a, key=lambda file : re.split('\d+', file.lower())[0])
    return b
```
