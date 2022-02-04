---
title: (프로그래머스) 위클리 챌린지 - 피로도
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-04 17:19:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "위클리 챌린지", "그리디"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/87946](https://programmers.co.kr/learn/courses/30/lessons/87946)

## Introduction

현재 상황에서 가장 최적의 값을 선택해 나가는 그리디 알고리즘을 설명하는데 이 문제는 가장 적합한 문제가 아닐까 싶다.

## Note

- 피로도 효율과 최소 피로 필요도 순으로 정렬하면 앞에서 부터 가장 최적의 던전을 고를 수 있다.

## Solution

```python
def solution(k, dungeons):
    answer = 0
    relocated_dungeons = sorted(dungeons, key=lambda x: (x[1] / x[0], x[1]))
    for dungeon in relocated_dungeons:
        required, consumption = dungeon
        if k < required or k - consumption < 0:
            continue
        k -= consumption
        answer += 1
    return answer
```

**풀이**

### 1. 던전을 재정렬

던전은 당연히 효율이 좋은 순서대로, 그리고 같은 효율이라면 더 적은 피로도를 쓰는게 좋기 때문에 던전을 `피로도 효율(소모 피로도/최소 필요 피로도)`, `소모 피로도` 순으로 정렬해준다.

```python
def solution(k, dungeons):
    relocated_dungeons = sorted(dungeons, key=lambda x: [x[1] / x[0], x[1]]) # 1
```

### 2. 던전을 순회

던전을 돌면서 피로도를 깎아나간다.

```python
def solution(k, dungeons):
    answer = 0
    relocated_dungeons = sorted(dungeons, key=lambda x: (x[1] / x[0], x[1]))
    for dungeon in relocated_dungeons: # 2
        required, consumption = dungeon
        k -= consumption
        answer += 1
    return answer
```

### 3. 입장불가 던전은 패스하기

던전을 돌다보면 다음 던전이 내가 가진 피로도 보다 최소 필요 피로도가 높거나, 피로도가 부족해 입장 할 수 없는 던전이 있다.그런 던전들은 제외한다.

continue로 다시 던전 순회를 계속하는 이유는, 내가 가진 피로도가 30이고 효율이 더 좋은 던전A(소모 피로도 50), 안 좋은 던전B(소모 피로도 20)가 뒤에 더 남아있을 때 A를 넘기고 B라도 입장하기 위해서이다.

```python
def solution(k, dungeons):
    answer = 0
    relocated_dungeons = sorted(dungeons, key=lambda x: (x[1] / x[0], x[1]))
    for dungeon in relocated_dungeons:
        required, consumption = dungeon
        if k < required or k - consumption < 0: # 3
            continue
        k -= consumption
        answer += 1
    return answer
```

## Comment

간단하고 재미있는 문제였다. 어떤 던전을 우선적으로 들어갈지에 대한 정렬만 잘하면 어렵지 않게 풀리는 문제이다.

프로그래머스의 몇몇 알고리즘 문제는 문제를 풀기 위해 추가로 독해능력을 필요로 한다.

현업에서 말이 잘 안통하는 상대의 요구사항을 파악하는 능력을 기르기 위함인건지는 모르겠지만, 알고리즘 고민하는 것보다 문제를 이해하는데 더 많은 노력이 필요한 것 같기도 하고 여러모로 그런 문제들은 좀 피곤해지는 것은 사실이다.

그런 점에서 지난 포스팅인 [프렌즈4 블록](https://elliotyim.github.io/posts/kakao-friends4-block/)도 그렇고 이번 문제와 같이 이해할 수 있는 명확한 조건들이 있는 문제들이 너무 좋다. 일단 풀면서 내가 뭘 해야할지 명확하니까 즐겁다.
