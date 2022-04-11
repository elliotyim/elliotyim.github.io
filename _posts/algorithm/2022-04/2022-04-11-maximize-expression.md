---
title: 2020 카카오 인턴십 - 수식 최대화
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-04-11 23:29:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags:
  ["알고리즘", "프로그래머스", "2020", "카카오", "인턴십", "스택", "후위표기법"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/67257](https://programmers.co.kr/learn/courses/30/lessons/67257)

## Introduction

우리가 사용하는 일반적인 수식은 모두 중위 표기법으로 표기되어 있는데, 이것이 사람이 이해하기 쉬운 표기법이라면 후위 표기법은 컴퓨터가 연산을 하기 쉽게 표현하는 표기법이다.

중위 표기법을 후위 표기법으로 바꿀 때 연산자의 우선 순위에 따라 어떤 연산자가 먼저 놓여 계산될지 정해지는데, 이를 이용하여 문제를 풀 수 있다.

## Note

- 후위 표기법을 이용하여 연산자 우선순위를 바꾸어 변환한뒤 계산한다.

## Solution

```python
import re

OP_ORDER = [
    "+-*", "+*-", "-+*", "-*+", "*+-", "*-+"
]

def calculate(expression):
    stack = []
    for exp in expression:
        if exp.isnumeric():
            stack.append(int(exp))
        else:
            b = stack.pop()
            a = stack.pop()
            if exp == '*':
                stack.append(a*b)
            elif exp == '+':
                stack.append(a+b)
            elif exp == '-':
                stack.append(a-b)
    return stack[-1]

def convert(expression, operators):
    result, stack = [], []
    for exp in expression:
        if exp.isnumeric():
            result.append(exp)
        elif not stack:
            stack.append(exp)
        else:
            while stack and operators.index(stack[-1]) <= operators.index(exp):
                result.append(stack.pop())
            stack.append(exp)

    while stack:
        result.append(stack.pop())
    return result

def split(expression):
    return re.findall(r'\d+|[^0-9]', expression)

def solution(expression):
    answer = 0
    split_exp = split(expression)
    for op in OP_ORDER:
        exp = convert(split_exp, op)
        result = calculate(exp)
        answer = max(answer, abs(result))
    return answer
```

### I. 필요한 함수 정의

우선 주어진 수식 문자열을 다루기 쉽게 숫자와 연산자로 분리된 리스트로 만들어 줄 함수를 정의한다.

```python
def split(expression):
    pass

def solution(expression):
    answer = 0
    split_exp = split(expression) # 1-1
    return answer
```

다음으로 분리된 식과 우선순위 순으로 나열된 연산자 문자열을 넣어주면 해당 식을 후위표기법으로 변형시켜주는 함수를 정의한다.

```python
OP_ORDER = [
    "+-*", "+*-", "-+*", "-*+", "*+-", "*-+" # 1-2
]

def convert(expression, operators):
    pass

def split(expression):
    pass

def solution(expression):
    answer = 0
    split_exp = split(expression)
    for op in OP_ORDER:
        exp = convert(split_exp, op) # 1-2
    return answer
```

변환한 수식을 계산하는 함수를 정의한다.

```python
OP_ORDER = [
    "+-*", "+*-", "-+*", "-*+", "*+-", "*-+"
]

def calculate(expression):
    pass

def convert(expression, operators):
    pass

def split(expression):
    pass

def solution(expression):
    answer = 0
    split_exp = split(expression)
    for op in OP_ORDER:
        exp = convert(split_exp, op)
        result = calculate(exp) # 1-3
    return answer
```

결과값의 절대값과 최대값을 비교하여 더 크면 갱신해 준다.

```python
OP_ORDER = [
    "+-*", "+*-", "-+*", "-*+", "*+-", "*-+"
]

def calculate(expression):
    pass

def convert(expression, operators):
    pass

def split(expression):
    pass

def solution(expression):
    answer = 0
    split_exp = split(expression)
    for op in OP_ORDER:
        exp = convert(split_exp, op)
        result = calculate(exp)
        answer = max(answer, abs(result)) # 1-4
    return answer
```

### II. 함수구현 - 수식 분리

수식은 1자리 이상의 숫자거나`\d+`, 숫자가 아닌 모든 문자`[^0-9]`를 기준으로 자른다.

```python
import re

def split(expression):
    return re.findall(r'\d+|[^0-9]', expression) # 2-1
```

이 함수를 사용하면 아래와 같이 숫자와 연산자를 분리할 수 있다.

```
"100-2145*458+12"
  -> ["100", "-", "2145", "*", "458", "+", "12"]
```

### III. 함수구현 - 수식 변환

아래의 과정을 통해 중위 표기식을 후위 표기법으로 변환한다.

1, 숫자는 바로 result에 넣는다.

```python
def convert(expression, operators):
    result, stack = [], []
    for exp in expression:
        if exp.isnumeric(): # 3-1
            result.append(exp)
```

2, 연산자는 스택이 비었으면 스택에 넣는다.

```python
def convert(expression, operators):
    result, stack = [], []
    for exp in expression:
        if exp.isnumeric():
            result.append(exp)
        elif not stack: # 3-2
            stack.append(exp)
```

3, 스택이 비어있지 않다면 스택의 가장 위에 있는 연산자가 현재 연산자의 우선순위보다 낮아질 때까지 스택에서 연산자를 계속 꺼내 result에 더하고, 마지막에 현재 연산자를 result에 더해준다.

```python
def convert(expression, operators):
    result, stack = [], []
    for exp in expression:
        if exp.isnumeric():
            result.append(exp)
        elif not stack:
            stack.append(exp)
        else:
            while stack and operators.index(stack[-1]) <= operators.index(exp):
                result.append(stack.pop()) # 3-3
            stack.append(exp) # 3-3
```

4, 마지막으로 스택에 남아있는 모든 것을 result에 넣어주고 result를 반환한다.

```python
def convert(expression, operators):
    result, stack = [], []
    for exp in expression:
        if exp.isnumeric():
            result.append(exp)
        elif not stack:
            stack.append(exp)
        else:
            while stack and operators.index(stack[-1]) <= operators.index(exp):
                result.append(stack.pop())
            stack.append(exp)
    while stack:
        result.append(stack.pop()) # 3-4
    return result
```

### IV. 함수구현 - 수식 계산

후위 표기법으로 변환된 수식은 다음과 같은 과정을 거쳐 계산한다.

1, 숫자면 스택에 넣는다.

```python
def calculate(expression):
    stack = []
    for exp in expression:
        if exp.isnumeric():
            stack.append(int(exp)) # 4-1
```

2, 연산자의 경우 스택에서 숫자를 2개 빼온다. (순서에 주의)

```python
def calculate(expression):
    stack = []
    for exp in expression:
        if exp.isnumeric():
            stack.append(int(exp))
        else:
            b = stack.pop() # 4-2
            a = stack.pop() # 4-2
```

3, 해당 연산자로 연산을 한 뒤 스택에 다시 넣어준다.

```python
def calculate(expression):
    stack = []
    for exp in expression:
        if exp.isnumeric():
            stack.append(int(exp))
        else:
            b = stack.pop()
            a = stack.pop()
            if exp == '*':
                stack.append(a*b) # 4-3
            elif exp == '+':
                stack.append(a+b) # 4-3
            elif exp == '-':
                stack.append(a-b) # 4-3
```

4, 모든 처리가 끝나면 스택에 담긴 숫자를 반환한다.

```python
def calculate(expression):
    stack = []
    for exp in expression:
        if exp.isnumeric():
            stack.append(int(exp))
        else:
            b = stack.pop()
            a = stack.pop()
            if exp == '*':
                stack.append(a*b)
            elif exp == '+':
                stack.append(a+b)
            elif exp == '-':
                stack.append(a-b)
    return stack[-1] # 4-4
```

## Comment

후위 표기법은 이상하리 만큼 구현하는 법이 안 외워졌었는데, 이번 문제를 정리하면서 머릿속에 제대로 넣을 수 있게 된 것 같다.
