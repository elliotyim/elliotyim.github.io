---
title: (프로그래머스) 연습 문제 - 입국심사
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-18 15:43:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "Binary Search"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/43238](https://programmers.co.kr/learn/courses/30/lessons/43238)

## Introduction

![Knife](/assets/img/algorithm/programmers/practice/immigration/knife.jpg)

여기에 칼이 있다. 이것을 보고 어떤 사람들은 아래와 같은 일식 요리사를 떠올린다.

![chef](/assets/img/algorithm/programmers/practice/immigration/chef.jpg)

혹은, 다른 무언가를 떠올릴수도 있을 것이다. 아마도 입에 담기에는 조금 거부감이 들 수 있는 무언가.

전자인 요리사가 사용하는 칼과 후자의 상상은 날카로운 물건을 사용하여 무언가를 잘라낸다라는 행위는 같지만, 이것을 어떤 용도로 사용할지, 어떻게 해석할지는 온전히 칼을 쥔 사람의 몫이다.

---

> [알고리즘 문제] 다음의 정렬된 배열에서 25라는 숫자를 찾아보세요 (4점)

```
[1, 4, 10, 13, 25, 28, 37, 48, 50, 51, 60, 71, 88, 90, 100]
```

이것이 내가 알고 있는 이진탐색의 사용법이자. 가장 먼저 떠오르는 예제이다.

나에게 있어서 `이진탐색 = 데이터 안에서 특정 수를 O(logN)으로 빠르게 찾는 방법` 딱 거기 까지의 의미를 가지고 있었다.

```python
def 이진탐색(대충_저런_배열):
    ...
    return 결과값
```

`이진탐색()`이라는 것은 딱 저러한 형태의 배열을 파라미터로 넣어주면 원하는 값을 찾아 주는 그런 정형화된 알고리즘이었다. 이 문제를 만나기 전까진.

## Note

- 이진탐색을 통해 나올 수 있는 최대 결과값을 원하는 값의 방향으로 빠르게 수렴시킨다.

## Solution

```python
def solution(n, times):
    answer, low, high = 0, 1, n * max(times)

    while low <= high:
        total, mid = 0, (low + high) // 2
        for time in times:
            total += mid // time

        if total < n:
            low = mid + 1
        else:
            high = mid - 1
            answer = mid

    return answer
```

### 1. 최대값 책정

이걸 정말 단순하게 한 명 한 명 돌아가면서 심사받고 걸린 시간 체크하는 시뮬레이션 방식으로 구현하면 무조건 시간초과가 난다.

따라서 접근방법을 바꿔서, 몇 명이 심사를 받던지 간에 소요시간 0초 ~ max초 사이에서 이진탐색을 통해 정답 소요시간 ?초를 찾아내는 방법으로 해결한다.

우선 가장 시간이 많이 걸리는 경우를 생각해보자. 입출력 예에서 나오는 7분짜리 심사대와 10분짜리 심사대가 있는 경우, 6명의 사람이 10분짜리 심사대에서만 줄을 서서 심사를 받으면 `6명 * 10분 = 60분`만큼 시간이 걸린다.

```python
def solution(n, times):
    high = n * max(times) # 1-1
```

### 2. 이진탐색

그러면 소요시간 최대값 60분을 기준으로 0~30분과 30분~60분, 이 두 구간 중에 정답이 어디에 있는지를 찾아보기 위해 가운데 값인 30분 안에 심사를 받을 수 있는 최대한 많은 사람 수를 세보자.

```
(소수점 버림)
30분 / 7분 = 4명
30분 / 10분 = 3명

4명 + 3명 = 7명
```

합계 7명으로 6명 보다 많다. 이 경우 정답은 0~30분 구간에 있다. (0~15~30)

위의 과정을 한 번 더 반복한다. 이번엔 절반 값인 15분을 살펴보자

```
15분 / 7분 = 2명
15분 / 10분 = 1명

2명 + 1명 = 3명
```

합계 3명으로 6명 보다 적다. 이 경우 정답은 15~30분 구간에 있다. 이 과정을 반복한다.

### 3. 구현

우선 이진 탐색을 위해 최소값과 최대값을 지정해준다.

```python
def solution(n, times):
    answer, low, high = 0, 0, n * max(times) # 3-1
```

low와 high의 중간 값 mid를 얻고 low나 high를 mid의 위치로 옮겨주는 것을 두 지점이 만날 때까지 반복한다.

우선, 소요시간의 중간 값 mid와 합계를 담을 변수 total을 선언해준다.

```python
def solution(n, times):
    answer, low, high = 0, 0, n * max(times)

    while low <= high:
        total, mid = 0, (low + high) // 2 # 3-2
```

중간 값의 소요시간 안에 최대한 많은 심사를 받을 수 있는 사람 수 합계를 구한다.

```python
def solution(n, times):
    answer, low, high = 0, 0, n * max(times)

    while low <= high:
        total, mid = 0, (low + high) // 2
        for time in times: # 3-3
            total += mid // time
```

1. 이 합계가 n보다 작으면 절반 중 뒤의 구간을 선택
   - (`low` -- **current** -- `mid` -- answer -- `high`)
2. 크거나 같으면 절반 중 앞의 구간을 선택
   - (`low` -- answer -- `mid` -- **current** -- `high`)

```python
def solution(n, times):
    answer, low, high = 0, 0, n * max(times)

    while low <= high:
        total, mid = 0, (low + high) // 2
        for time in times:
            total += mid // time

        if total < n: # 3-4
            low = mid + 1
        else:
            high = mid - 1
```

total과 n이 같은 경우는 정답을 찾은 상태고 else문으로 들어가니 answer에 현재 소요시간을 넣어주고 이 값을 반환한다.

```python
def solution(n, times):
    answer, low, high = 0, 0, n * max(times)

    while low <= high:
        total, mid = 0, (low + high) // 2
        for time in times:
            total += mid // time

        if total < n:
            low = mid + 1
        else:
            high = mid - 1
            answer = mid # 3-5

    return answer
```

## Comment

이진 탐색은 A라는 값을 B라는 값의 방향으로 빠르게 수렴시킨다는 특성을 가지고 있다. (배열에서 특정 값 찾기만 해도 처음에 내가 얻게 되는 가장 중간에 있는 값을 B라는 값으로 수렴시킨다는 것을 알 수 있다.)

이름이 탐색이라서, 이 특성에 대해서 눈치채지 못할 수도 있는데 이 문제는 나의 그 고정관념을 부술 수 있었던 좋은 문제였던 것 같다.
