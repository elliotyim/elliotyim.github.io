---
title: (프로그래머스) Summer/Winter Coding(~2018) - 영어 끝말잇기
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-11 15:51:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "서머/윈터코딩"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/12981](https://programmers.co.kr/learn/courses/30/lessons/12981)

## Introduction

간단한 구현 문제이다. 코드로 끝말잇기 게임이 진행되는 상황을 묘사하면 된다.

## Note

- 현재 order가 n과 같아졌을 때 다시 처음으로 맞춰주는 작업을 해야 한다

## Solution

```python
def solution(n, words):
    spoken_words = dict()
    order, phase = 1, 1
    prev_word = words[0][0]

    for word in words:
        if word in spoken_words or prev_word[-1] != word[0]:
            return [order, phase]
        elif order == n:
            phase += 1

        spoken_words[word] = True
        prev_word = word
        order = (order % n) + 1

    return [0, 0]
```

### 1. 혼자서 게임을 진행

우선 혼자서 게임을 진행해 보자. 단어를 말하고 나면 이미 말한 단어 사전에 집어넣고 이전 단어만 기억한다.

```python
def solution(n, words):
    spoken_words = dict()
    for word in words: # 1
        spoken_word[word] = True
        prev_word = word
```

### 2. 여럿이서 게임을 진행

여럿이서 연습게임을 할건데 틀리는건 상관없이 진행을 해보자

마지막 사람까지 차례가 돌아왔을 때(order == n) phase를 하나씩 올려준다.

order는 1씩 추가해 나가는데, 만약 order == n인 상황인 경우 값을 0으로 맞춰주고(order % n) 1을 더해준다.

```
order = 3, n = 3일 때

3 % 3 = 0 이므로,
order = (3 % 3) + 1 = 1 이 된다.

나머지의 경우 원래 숫자에 1만 더해진다.
```

```python
def solution(n, words):
    spoken_words = dict()
    order, phase = 1, 1

    for word in words:
        if order == n: # 2-2
            phase += 1

        spoken_word[word] = True
        prev_word = word
        order = (order % n) + 1 # 2-1
```

### 3. 틀리는 경우를 걸러낸다.

끝말잇기에서 틀리는 경우는 두 가지이다.

1. 이미 말한 단어를 또 말할 때
2. 이전 단어의 끝 단어와 다른 알파벳으로 시작하는 단어를 말할 때

아무도 틀린 사람이 없을 때는 [0, 0]을 반환한다.

```python
def solution(n, words):
    spoken_words = dict()
    order, phase = 1, 1
    prev_word = words[0][0]

    for word in words:
        if word in spoken_words or prev_word[-1] != word[0]: # 3
            return [order, phase]
        elif order == n:
            phase += 1

        spoken_words[word] = True
        prev_word = word
        order = (order % n) + 1

    return [0, 0]
```

## Comment

이번 문제와 같이 고려할 것이 적은 문제들을 해결할 때의 코드는 굉장히 명확한 경우가 많다.

하지만 다루는 데이터의 양이 커지면 커질수록 순진한 방법으로는 문제를 해결할 수 없고, 어떻게든 다양한 아이디어를 적용하여 최적화를 해야한다.

보통 그런 것들을 해결해 줄 알고리즘들은 머리좋은 사람들이 이미 만들어 놓은 경우가 대부분이라서 열심히 배우고 익혀나가고 있다. 세상엔 정말 대단한 사람들이 많은 것 같다.
