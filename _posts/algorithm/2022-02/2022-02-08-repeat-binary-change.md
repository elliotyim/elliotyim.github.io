---
title: (프로그래머스) 월간 코드 챌린지 시즌1 - 이진 변환 반복하기
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-08 12:54:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "n진수"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/70129](https://programmers.co.kr/learn/courses/30/lessons/70129)

## Introduction

함수는 정말 매력적인 도구다.

함수의 역할은 그 함수가 가진 이름표에 적혀 있고, 어떻게 동작하는지에 대한 상세한 정보를 몰라도 어떤 입력 값을 주면 그에 맞는 출력 값이 나온다.

심플하고, 명확하다.

```python
# 1
def f(a):
    return ''.join([b for b in a if b != '0'])
```

파이썬을 전혀 모르는 사람이 1번 함수를 보면 이게 대체 뭔소린지 이해가 안된다.

```python
# 2
def remove_zeros(string):
    return ''.join([no for no in string if no != '0'])
```

하지만 2번과 같이 함수의 이름을 지정하고 파라미터의 이름을 알맞게 적어주면 완벽하게 이해는 안되더라도 어떻게 돌아가는지를 짐작해볼 수 있다.

```python
# 3
def remove_zeros(string):
    return string.replace('0', '')
```

좀 더 읽기 쉬운 문장으로 바꾼다면 3번과 같이 작성할 수도 있지만, 알고리즘을 생각하다 보면 필연적으로 모든걸 문장으로만 묘사할 수 없는 상황도 생겨난다.

이것은 해당 프로그래머의 역량 부족일 수도 있고, 알고리즘 자체가 수학적 증명에 의존한 것일 수도 있다. 가령 예를 들면 힙의 자료구조를 구현하면서 그 자체로서 문장으로 설명하는건 정말 난해하다.

인덱스가 i인 부모 노드의 왼쪽 자식 노드의 인덱스가 `2 * i` 이고 오른쪽 자식의 인덱스가 `2 * i + 1` 라는 것을 구현과 동시에 이해할 수 있는 설명문으로 적는 것은 쉽지 않다.

구구절절 코멘트를 달아놓을 수도 있겠지만 어찌되었든 내용이 길어지는것은 복잡도도 그만큼 증가하기 때문에 직관성이 떨어진다.

그래서 2번과 같이 로직을 작성하고 이를 함수로 래핑하는 경우도 많다.

## Note

- 이진수로 변환한 수 자체가 아니라 그 수의 길이를 다루는 것에 주의

## Solution

```python
def convert_no(no, n):
    nums, quotient = '', no
    while quotient > 0:
        quotient, remainder = quotient // n, int(quotient % n)
        nums = str(remainder) + nums
    return nums if no > 0 else '0'

def solution(s):
    answer, prev = [0, 0], s
    while True:
        current = prev.replace('0', '')
        converted_no = convert_no(len(current), 2)
        answer[0] += 1
        answer[1] += len(prev) - len(current)
        if converted_no == '1':
            break
        prev = converted_no
    return answer
```

**풀이**

### 1. 필요한 함수 지정

지문에는 어떤 행동을 하여 문제를 해결해야 할지 적혀 있다.

1. x의 모든 0을 제거합니다.
2. x의 길이를 c라고 하면, x를 "c를 2진법으로 표현한 문자열"로 바꿉니다.

1번: 0을 제거, 2번: 어떤 수를 n진법으로 표현.

```python
def convert_no(no, n):
    pass

def remove_zeros(string):
    pass

def solution(s):
    answer = []
    return answer
```

### 2. 지정된 함수들을 가지고 문제를 해결

우선 1번과 2번을 코드로 표현해보자.

```python
def convert_no(no, n):
    pass

def remove_zeros(string):
    pass

def solution(s):
    answer = [0, 0]
    while True:
        current = remove_zeros(s)
        converted_no = convert_no(len(current), 2)
        break
    return answer
```

이걸 숫자가 1이 될 때까지 계속 반복한다.

```python
def convert_no(no, n):
    pass

def remove_zeros(string):
    pass

def solution(s):
    answer, prev = [0, 0], s
    while True:
        current = remove_zeros(prev)
        converted_no = convert_no(len(current), 2)
        if conveted_no == '1':
            break
        prev = converted_no
    return answer
```

이 과정 중에서 정답의 값을 계속 갱신해준다.

```python
def convert_no(no, n):
    pass

def remove_zeros(string):
    pass

def solution(s):
    answer, prev = [0, 0], s
    while True:
        current = remove_zeros(prev)
        converted_no = convert_no(len(current), 2)
        answer[0] += 1
        answer[1] += len(prev) - len(current)
        if converted_no == '1':
            break
        prev = converted_no
    return answer
```

### 3. 지정된 함수들을 구현

필요한 함수들을 구현해 준다.

```python
def convert_no(no, n):
    nums, quotient = '', no
    while quotient > 0:
        quotient, remainder = quotient // n, int(quotient % n)
        nums = str(remainder) + nums
    return nums if no > 0 else '0'

def remove_zeros(string):
    return string.replace('0', '')

def solution(s):
    answer, prev = [0, 0], s
    while True:
        current = remove_zeros(prev)
        converted_no = convert_no(len(current), 2)
        answer[0] += 1
        answer[1] += len(prev) - len(current)
        if converted_no == '1':
            break
        prev = converted_no
    return answer
```

굳이 remove_zeros라는 함수를 쓸 필요 없이 string의 내장함수로 그대로 써주자

```python
def convert_no(no, n):
    nums, quotient = '', no
    while quotient > 0:
        quotient, remainder = quotient // n, int(quotient % n)
        nums = str(remainder) + nums
    return nums if no > 0 else '0'

def solution(s):
    answer, prev = [0, 0], s
    while True:
        current = prev.replace('0', '')
        converted_no = convert_no(len(current), 2)
        answer[0] += 1
        answer[1] += len(prev) - len(current)
        if converted_no == '1':
            break
        prev = converted_no
    return answer
```

## Comment

string에 내장된 replace라는 함수는 내부에서 어떻게 동작하는지 알지 모르더라도 이 함수를 사용한 코드를 읽어나갈 때 흐름을 파악하는데는 아무런 문제가 없다.

잘 쓰여진 글과 잘 만들어진 도구들은 일반적인 관점에서 매우 명료하고 이해하는데 추가적인 설명이 필요가 없이 직관적인데, 나는 그런 것들을 만들고 있는지 언제나 스스로를 돌아보게 된다.

아직은 더 많은 연습이 필요한 것 같다.

근데 이것도 문제가 Level 1 수준인데..?
