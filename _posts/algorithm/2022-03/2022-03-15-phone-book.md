---
title: (프로그래머스) 연습 문제 - 전화번호 목록
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-15 15:23:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "해시"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/42577](https://programmers.co.kr/learn/courses/30/lessons/42577)

## Introduction

원래 예전에 풀었었던 문제였는데 테스트 케이스가 변경되고 이전 풀이로는 못 넘어간다길래 다시 풀어봤다.

다시 풀어놓고 보니 절대 좋은 풀이는 아니다. (n이 백만이라 그런지 수행시간이 미쳐날뛴다.)

하지만, 다른 사람이 Dictionary(Hash Map)를 사용해 문제를 푼 것을 보고 `해시`라는 키워드를 보고 각자 떠올리는게 다르구나 하는 생각을 했다. (나는 Hash Set을 떠올렸다.)

그리고 이걸 기록으로 남겨두고 싶었다.

내가 작성하는 어떠한 형태의 글도 읽는 사람에 따라서는 다르게 해석할 수 있다는 것을 되새기고 싶었고, 그렇다면 나의 생각과 의도를 더욱 정확하게 전달하기 위해서 어떤 식으로 글을 쓰고(코딩을 하고) 단어 선택(변수 지정)을 해야 할 것인가에 대해 고민하게 됐다.

## Note

- 해시는 중복을 허용하지 않는다는 점을 이용한다.

## Solution

```python
def solution(phone_book):
    no_set = set()
    sorted_phone_book = sorted(phone_book, key=len, reverse=True)

    for no in sorted_phone_book:
        added_nums = 0
        for i in range(len(no)):
            if no[:i+1] not in no_set:
                no_set.add(no[:i+1])
                added_nums += 1
        if added_nums == 0:
            return False
    return True
```

### 1. 사전 작업

우선 사용할 해시 셋을 하나 준비한다.

```python
def solution(phone_book):
    no_set = set() # 1-1
```

phone_book을 길이의 역순으로 정리한다.

```python
def solution(phone_book):
    no_set = set()
    sorted_phone_book = sorted(phone_book, key=len, reverse=True) # 1-2
```

### 2. sorted_phone_book 순회

역순으로 정렬된 sorted_phone_book을 번호들을 순회한다.

```python
def solution(phone_book):
    no_set = set()
    sorted_phone_book = sorted(phone_book, key=len, reverse=True)

    for no in sorted_phone_book: # 1-3
        pass
```

no를 앞에서 부터 n개씩 짤라서 no_set에 없으면 넣고 들어간 숫자의 개수를 카운트 한다. 길이의 역순으로 정렬해 두었기 때문에 뒤로 갈수록 넣는 횟수는 줄어든다.

ex) "1235"의 경우 -> "1", "12", "123", "1235"

```python
def solution(phone_book):
    no_set = set()
    sorted_phone_book = sorted(phone_book, key=len, reverse=True)

    for no in sorted_phone_book:
        added_nums = 0
        for i in range(len(no)): # 1-4
            if no[:i+1] not in no_set:
                no_set.add(no[:i+1])
                added_nums += 1
```

added_nums가 0이면 해당 no의 숫자는 이미 no_set에 들어있다는 의미이므로 False를 반환하고 종료한다. 순회가 모두 이상 없이 끝나면 중복이 없으므로 True를 반환한다.

```python
def solution(phone_book):
    no_set = set()
    sorted_phone_book = sorted(phone_book, key=len, reverse=True)

    for no in sorted_phone_book:
        added_nums = 0
        for i in range(len(no)):
            if no[:i+1] not in no_set:
                no_set.add(no[:i+1])
                added_nums += 1
        if added_nums == 0: # 1-5
            return False
    return True
```

### 번외. 출제자의 의도

이 사람의 풀이도 나처럼 글자를 하나씩 끊어서 찾아보고 찾아보는 방식이긴 하지만, 나와는 다르게 처음에 정렬을 하지 않는다.

정렬이 O(nlogn)이라 해시맵을 채워나가는 과정인 O(n)보다는 더 느려서 그런지 내 풀이보다 수행시간이 2배 빨랐다.

```python
def solution(phone_book):
    answer = True
    hash_map = {}
    for phone_number in phone_book:
        hash_map[phone_number] = 1
    for phone_number in phone_book:
        temp = ""
        for number in phone_number:
            temp += number
            if temp in hash_map and temp != phone_number:
                answer = False
    return answer
```

### 번외2. 개썅마이웨이

아름다워 (풀이들 중 수행속도가 젤 빠른게 유머)

```python
def solution(phoneBook):
    phoneBook = sorted(phoneBook)

    for p1, p2 in zip(phoneBook, phoneBook[1:]):
        if p2.startswith(p1):
            return False
    return True
```

![life sucks](/assets/img/algorithm/programmers/practice/phone-book/life_sucks.png)
_네 형제님 어떤 기분인지 압니다._

## Comment

정리되지 않은 리스트에 대한 접근방식이 나는 아직 좀 미숙한 것 같다. (무작정 정렬부터 해버리다 보니...) 다른 사람의 Dictionary를 사용한 풀이를 보고 나서 무언가 힌트를 얻은 것 같기도 하지만.
