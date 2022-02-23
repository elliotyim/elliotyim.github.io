---
title: (프로그래머스) 월간 코드 챌린지 시즌1 - 삼각 달팽이
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-23 12:31:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "월간 코드 챌린지", "시즌1"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/68645](https://programmers.co.kr/learn/courses/30/lessons/68645)

## Introduction

수학에 대해 이해를 잘못하고 있는 걸지도 모르겠지만, 내가 생각하기에 수학은 어떤 규칙들과 논리들이 서로 맞물려 있고 이를 토대로 절대적인 답이 정해져 있다.

세상을 살면서 벌어지는 모든 일들에는 절대적인 게 없는데, 유독 수학의 세계는 변함이 없는 엄격함이 적용되어서 신기하기도 했지만 어릴 적에는 그게 이해가 잘 안 됐었다. 어떻게 그런 신기한 일이 벌어질 수 있는 거지? 수학에는 예외라는 게 없나?

수학을 사용하는 입장에서는 예외가 없이 언제나 동일하게 적용되는 것만큼 안정적인 것도 없다. 그저 규칙을 찾고, 그 규칙을 가지고 문제를 해결하기만 하면 되니까

## Note

- 숫자를 채워 나가는 부분을 3파트로 나눈다.
- 파트별로 채워 나가는 숫자가 1씩 줄어듬에 유의

## Solution

```python
from functools import reduce

def solution(n):
    answer = [[0 for _ in range(i+1)] for i in range(n)]

    x, y, no = 0, -1, 0
    for i in range(n, 0, -3):
        for j in range(i):
            y += 1
            no += 1
            answer[y][x] = no
        for j in range(i-1):
            x += 1
            no += 1
            answer[y][x] = no
        for j in range(i-2):
            x -= 1
            y -= 1
            no += 1
            answer[y][x] = no

    return reduce(lambda a, b: a+b, answer)
```

### 1. 규칙을 파악

삼각 달팽이는 2차원 리스트를 통해 표현할 수 있다. 그리고 반시계 방향으로 회전하면서 숫자가 채워진다.

그리고 아래와 같은 규칙이 있다.

1. 세로로 한 줄, 가로로 한 줄, 대각선 한 줄로 숫자가 차는 것이 반복 된다.
2. n개부터 시작해서 세로는 n개, 가로는 n-1개, 대각선은 n-2개 만큼 숫자를 채우고 다음 layer에서는 n에서 3만큼(이전 layer의 세로, 가로, 대각선 줄의 갯수만큼) 빠진 숫자로 반복한다.

![snail](/assets/img/algorithm/programmers/practice/triangle-snail/snail001.png)

![snail2](/assets/img/algorithm/programmers/practice/triangle-snail/snail002.png)

### 2. 구현

우선 속이 0으로 비어있는 2차원 리스트를 만들어 준다.

```python
def solution(n):
    answer = [[0 for _ in range(i+1)] for i in range(n)] # 2-1
```

한 줄 한 줄 연산을 할 수 있도록 for문을 써준다. n부터 시작해서 0보다 크면 계속 반복하고, layer가 증가 될 때마다 3씩 빼준다.

다음 for문 3개는 첫 번째부터 세로줄, 가로줄, 대각선 순이다.

```python
def solution(n):
    answer = [[0 for _ in range(i+1)] for i in range(n)]

    for i in range(n, 0, -3): # 2-2
        for j in range(i):
            pass
        for j in range(i-1):
            pass
        for j in range(i-2):
            pass
```

아래의 조건 생각하며 구현해 준다.

1. 세로줄: y가 1씩 커짐
2. 가로줄: x가 1씩 커짐
3. 대각선: x와 y가 1씩 작아짐

초기 값은 처음에 더할 때 x=0, y=0, no=1로 적용되도록 맞춰준다.

```python
def solution(n):
    answer = [[0 for _ in range(i+1)] for i in range(n)]

    x, y, no = 0, -1, 0 # 2-3
    for i in range(n, 0, -3):
        for j in range(i):
            y += 1
            no += 1
            answer[y][x] = no
        for j in range(i-1):
            x += 1
            no += 1
            answer[y][x] = no
        for j in range(i-2):
            x -= 1
            y -= 1
            no += 1
            answer[y][x] = no
```

이렇게 준비된 2차원 리스트([[1], [2, 15], [3, 16, 14], ...]를 1차원 리스트([1, 2, 15, 3, 16, 14, ...])로 변환해주면 된다.

파이썬의 경우 리스트를 더하는 연산으로 각 리스트를 쉽게 합칠 수 있으므로 더 짧게 표기가 가능했다.

ex) [1] + [2] = [1, 2]

```python
from functools import reduce

def solution(n):
    answer = [[0 for _ in range(i+1)] for i in range(n)]

    x, y, no = 0, -1, 0
    for i in range(n, 0, -3):
        for j in range(i):
            y += 1
            no += 1
            answer[y][x] = no
        for j in range(i-1):
            x += 1
            no += 1
            answer[y][x] = no
        for j in range(i-2):
            x -= 1
            y -= 1
            no += 1
            answer[y][x] = no

    return reduce(lambda a, b: a+b, answer) # 2-4
```

## Comment

알고 보면 정말 쉬운데 규칙을 발견하기가 까다로웠던 문제였다.

문제를 보면 긴장부터 하는 습관을 고쳐야 할듯...
