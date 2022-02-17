---
title: (프로그래머스) Summer/Winter Coding(~2018) - 점프와 순간 이동
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-17 12:52:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "서머/윈터코딩", "그리디"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/12980](https://programmers.co.kr/learn/courses/30/lessons/12980)

## Introduction

프로그래머는 인류가 쌓아 온 경험과 설계 방식, 알고리즘 그리고 수학 지식 등 다양한 무기를 활용하여 문제를 해결하는 해결사들이다.

그들은 문제 해결에 대한 아이디어를 얻을 때 이러한 무기들을 사용하는데, 여기서 활용할 수 있는 방법들은 무궁무진하기 때문에 사람마다 다른 방법을 생각해내고 다양한 시각으로 문제를 분석하여 해결해 나간다.

경험이 적었던 나는 문제 해결에 대한 아이디어를 얻는 능력이 약했고, 왜인지 문제 해결에 대한 상상력을 발휘하는 것에는 제동이 걸려져 있는 상태였다.

그렇기 때문에 이 문제도 쉽게 풀리지 않았었고, 다른 사람의 풀이를 보고 놀라게 되었다.

![Hassou wo gyakuten suru no](/assets/img/algorithm/programmers/practice/jump-and-teleport/gyakuten.jpg)

처음에 이 문제를 풀 때 나는 어떻게든 시작점에서 목적지까지 도달하는 것만 신경 쓰고 있었지, 목적지에서부터 출발할 생각을 하지 못했었다.

## Note

- 최대한 많이 순간이동 하는 것이 건전지를 적게 쓰는 방법

## Solution

```python
def solution(n):
    steps = 0
    while n > 0:
        if n % 2 == 1:
            steps += 1
        n //= 2
    return steps
```

**풀이**

![tenet](/assets/img/algorithm/programmers/practice/jump-and-teleport/tenet.jfif)
_인버전_

우선 목적지인 n번째 칸에 서서 1번째 칸을 바라본다. 저곳으로 최대한 건전지를 적게 써서 가야 한다.

여기서 가장 많이 움직일 수 있는 방법은 한 칸 걷는 것보다는 순간이동이다. 순간 이동을 하면 현재 칸의 절반 숫자의 칸으로 건전지를 쓰지 않고 이동할 수 있다.

가능하면 걷는 것보단 순간이동만 해서 가는게 좋다. 심지어 한 칸을 순간이동 하더라도 건전지를 사용하는 걷기보다는 낫다.

```python
def solution(n):
    n //= 2 # 1
```

어? 그런데 밟고 있는 칸을 보니 101번째 칸이고, 50.5번째 칸이란건 안 보인다. 아무래도 순간이동을 할 수 있는 건 짝수칸 일 때 뿐인 것 같다.

![platform](/assets/img/algorithm/programmers/practice/jump-and-teleport/platform.png)
_뭐라고?_

하는 수 없이 한 칸 앞으로 이동 후 순간이동을 한다.

```python
def solution(n):
    steps = 0
    if n % 2 == 1: # 2
        steps += 1
    n //= 2
```

이걸 1번째 칸에 도착할 때까지 반복한다.

```python
def solution(n):
    steps = 0
    while n > 0:
        if n % 2 == 1:
            steps += 1
        n //= 2
    return steps
```

## Comment

발상의 전환이 중요하다는 말은 많이 듣지만 머리가 많이 굳어져 있는 탓일까 쉽지 않은 것 같다.
