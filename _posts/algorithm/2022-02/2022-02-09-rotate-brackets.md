---
title: (프로그래머스) 월간 코드 챌린지 시즌2 - 괄호 회전하기
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-09 15:28:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "스택", "큐"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/76502](https://programmers.co.kr/learn/courses/30/lessons/76502)

## Introduction

괄호 체크에는 빠지지 않고 등장하는 자료구조인 스택과, 문자열을 간편하게 회전시킬 수 있는 큐를 사용하면 간단하게 풀 수 있는 문제이다.

여는 괄호(`(`, `{`, `[`)가 나왔을 때는 스택에 넣고, 닫는 괄호(`)`, `}`, `]`)가 나왔을 때 스택에서 하나씩 빼는 방식으로 체크할 수 있다.

## Note

- 괄호를 모두 체크 한 후 스택에 남아 있는 괄호가 있는지 체크

## Solution

```python
from collections import deque

OPENING = ["(", "{", "["]
CLOSING = [")", "}", "]"]

def check_brackets(brackets):
    stack = []
    for bracket in brackets:
        if bracket in OPENING:
            stack.append(bracket)
        elif bracket in CLOSING:
            if not stack or CLOSING.index(bracket) != OPENING.index(stack.pop()):
                return False
    return not stack

def solution(s):
    answer, bracket_queue = 0, deque(s)
    for _ in s:
        is_valid_brackets = check_brackets(bracket_queue)
        if is_valid_brackets:
            answer += 1
        bracket_queue.rotate(1)
    return answer
```

**풀이**

### 1. 필요한 함수 정의

괄호 문자열을 회전 시킬 큐는 이미 기본 라이브러리에 구현되어 있지만 괄호를 체크하는 기능은 직접 구현해야 한다.

```python
def check_brackets(brackets):
    pass

def solution(s):
    answer = 0
    return answer
```

### 2. 정의된 함수를 가지고 문제를 해결

우선 주어진 괄호 문자열을 가지고 괄호 큐를 만들어 준다.

```python
from collections import deque

def check_brackets(brackets):
    pass

def solution(s):
    answer, brackets_queue = 0, deque(s) # 2-1
    return answer
```

괄호 문자열에 있는 괄호가 다시 제자리로 돌아올때까지 회전시킬 것이므로 괄호 문자열의 길이만큼 회전시킨다.

```python
from collections import deque

def check_brackets(brackets):
    pass

def solution(s):
    answer, brackets_queue = 0, deque(s)
    for _ in s:
        brackets_queue.rotate(1) # 2-2
    return answer
```

회전하면서 괄호를 체크해 주고 올바른 괄호 문자열이면 정답 카운트를 하나씩 올린다.

```python
from collections import deque

def check_brackets(brackets):
    pass

def solution(s):
    answer, brackets_queue = 0, deque(s)
    for _ in s:
        is_valid_brackets = check_brackets(brackets_queue) # 2-3
        if is_valid_brackets:
            answer += 1
        brackets_queue.rotate(1)
    return answer
```

### 3. 정의된 함수를 구현

stack을 사용하여 여는 괄호면 넣고, 닫는 괄호면 스택에서 뺀다. 이 타이밍에 스택이 비어있거나 종류가 다른 괄호면 False를 반환하고 함수를 종료한다.

```python
from collections import deque

OPENING = ["(", "{", "["]
CLOSING = [")", "}", "]"]

def check_brackets(brackets):
    stack = []
    for bracket in brackets: # 3-1
        if bracket in OPENING:
            stack.append(bracket)
        elif bracket in CLOSING:
            if not stack or CLOSING.index(bracket) != OPENING.index(stack.pop()):
                return False


def solution(s):
    answer, brackets_queue = 0, deque(s)
    for _ in s:
        is_valid_brackets = check_brackets(brackets_queue)
        if is_valid_brackets:
            answer += 1
        brackets_queue.rotate(1)
    return answer
```

모든 괄호를 체크했음에도 스택에 괄호가 남아있다면 잘못된 문자열이므로 이 또한 체크해 준다.

```python
from collections import deque

OPENING = ["(", "{", "["]
CLOSING = [")", "}", "]"]

def check_brackets(brackets):
    stack = []
    for bracket in brackets:
        if bracket in OPENING:
            stack.append(bracket)
        elif bracket in CLOSING:
            if not stack or CLOSING.index(bracket) != OPENING.index(stack.pop()):
                return False
    return not stack # 3-2 스택이 비어 있으면 True 아니면 False


def solution(s):
    answer, brackets_queue = 0, deque(s)
    for _ in s:
        is_valid_brackets = check_brackets(brackets_queue)
        if is_valid_brackets:
            answer += 1
        brackets_queue.rotate(1)
    return answer
```

## Comment

스택을 사용한 알고리즘 문제 풀이에서 괄호 관련된 문제가 나오는 경우는 많지 않았던 것 같다.

괄호체크는 스택 입문용이고 보통 이것 보다는 더 난이도가 있는 DFS 문제로 나오는 경우가 많다.
