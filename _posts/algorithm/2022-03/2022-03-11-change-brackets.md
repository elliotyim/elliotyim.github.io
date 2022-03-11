---
title: 2020 KAKAO BLIND RECRUITMENT - 괄호 변환
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-11 22:08:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2020", "카카오"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/60058](https://programmers.co.kr/learn/courses/30/lessons/60058)

## Introduction

2019년 9월 시행된 카카오 블라인드 테스트에서 긴 시간동안 알고리즘에 대한 트라우마를 안겨주었던 정말 나로써는 두 번 다시 쳐다보기도 싫은 문제이다.

당시 알고리즘 공부를 막 시작했던 내가 경험삼아 신청했던 2020 블라인드 테스트에서 이 문제는 도무지 이해가 안됐고, 3시간을 고군분투 했지만 정답에는 근접하지도 못한채 테스트가 끝났고 나는 좌절했다.

3시간의 무력감을 견디며 그래도 좋은 경험일거라며 버텨야 했던 경험은 꽤 마음의 상처로 남아있다.

이 때 이후로 알고리즘이 정말 힘들었었다. 한동안은 문제만 보면 정신이 멍해지고 집중을 못하고 도망치고 싶다는 생각밖에 들지 않았었다.

대체 이 문제가 뭘 하고 싶어하는지도 모르겠고 솔직히 이 문제는 정리 안하고 넘어가려고 했었다.

시간이 지난 지금 다시 봐도 이 문제는 대체 무슨 근거로 `올바른 괄호 문자열`을 판단하는지 알 수가 없다.

예를 들어, ")()()()("을 입력 파라미터로 넣으면 내가 생각하기엔 "()()()()"이 되어야 할 것 같은데 정답은 "(((())))"이다.

그렇다고 문제에서 말하는 `올바른 괄호 문자열`라는 것이 어떤 건지 그것에 대한 정의가 제대로 서술되어 있는 것도 아니다.

이 포스팅은 아마도 정리만 해놓고 다신 안 볼 것 같다. 내가 알고리즘을 잘하는 것도 아니고, 문제를 잘 만들 수 있을 정도로 머리가 좋지는 않지만 그럼에도 불구하고 이 문제가 좋은 문제라는 말은 못해줄 것 같다.

## Note

- 알고리즘대로 구현 한다.

## Solution

```python
BRACKET_DIC = {"(": ")", ")": "("}

def reverse(brackets):
    return ''.join([BRACKET_DIC[bracket] for bracket in brackets])

def check_brackets(brackets):
    if not brackets:
        return False

    stack = []
    for bracket in brackets:
        if bracket == '(':
            stack.append(bracket)
        elif bracket == ')':
            if not stack or stack[-1] != '(':
                return False
            stack.pop()
    return True

def check_if_balanced(brackets):
    return brackets.count('(') == brackets.count(')')

def split_brackets(brackets):
    u, v = brackets, ''
    for i in range(2, len(brackets), 2):
        is_balanced = check_if_balanced(brackets[:i])
        if is_balanced:
            u, v = brackets[:i], brackets[i:]
            break
    return u, v

def f(w):
    if not w:
        return ''

    u, v = split_brackets(w)
    is_valid = check_brackets(u)
    if is_valid:
        return u + f(v)
    else:
        return '(' + f(v) + ')' + reverse(u[1:-1])

def solution(p):
    return f(p)
```

### 1. 알고리즘대로 구현

우선 알고리즘이 써져 있으니 그대로 따라서 구현해 보자

1. 입력이 빈 문자열인 경우, 빈 문자열을 반환합니다.

```python
def f(w):
    if not w: # 1-1
        return ''

def solution(p):
    return f(p)
```

2. 문자열 w를 두 "균형잡힌 괄호 문자열" u, v로 분리합니다. 단, u는 "균형잡힌 괄호 문자열"로 더 이상 분리할 수 없어야 하며, v는 빈 문자열이 될 수 있습니다.

```python
def split_brackets(brackets):
    pass

def f(w):
    if not w:
        return ''

    u, v = split_brackets(w) # 1-2

def solution(p):
    return f(p)
```

3. 문자열 u가 "올바른 괄호 문자열" 이라면 문자열 v에 대해 1단계부터 다시 수행합니다.

   3-1. 수행한 결과 문자열을 u에 이어 붙인 후 반환합니다.

```python
def check_brackets(brackets):
    pass

def split_brackets(brackets):
    pass

def f(w):
    if not w:
        return ''

    u, v = split_brackets(w)
    is_valid = check_brackets(u) # 1-3
    if is_valid:
        return u + f(v)

def solution(p):
    return f(p)
```

4. 문자열 u가 "올바른 괄호 문자열"이 아니라면 아래 과정을 수행합니다.

   4-1. 빈 문자열에 첫 번째 문자로 '('를 붙입니다.

   4-2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과 문자열을 이어 붙입니다.

   4-3. ')'를 다시 붙입니다.

   4-4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙입니다.

   4-5. 생성된 문자열을 반환합니다.

```python
def reverse(brackets):
    pass

def check_brackets(brackets):
    pass

def split_brackets(brackets):
    pass

def f(w):
    if not w:
        return ''

    u, v = split_brackets(w)
    is_valid = check_brackets(u)
    if is_valid:
        return u + f(v)
    else: # 1-4
        return '(' + f(v) + ')' + reverse(u[1:-1])

def solution(p):
    return f(p)
```

### 2. 함수구현 - 괄호 분리

괄호를 균형잡힌 괄호 문자열 u와 그렇지 않은 문자열 v로 나눠야 한다.

이미 균형이 잡혀있을 경우 체크 한 뒤에 원본이 그대로 반환 될 것이기에 처음에는 u에 입력받은 괄호문자열을 그대로 넣어주고 v에는 공백을 넣어준다.

```python
def split_brackets(brackets):
    u, v = brackets, '' # 2-1
    return u, v
```

앞에서 부터 2개씩 체크하며 균형잡힌 문자열인지 체크한다. 균형이 잡힌 부분문자열을 발견하면 거기서 끊어준다.

```python
def check_if_balanced(brackets):
    pass

def split_brackets(brackets):
    u, v = brackets, ''
    for i in range(2, len(brackets), 2): # 2-2
        is_balanced = check_if_balanced(brackets[:i])
        if is_balanced:
            u, v = brackets[:i], brackets[i:]
            break
    return u, v
```

균형이 잡혀 있는지는 간단하게 `(`의 개수와 `)`가 같은지만 체크하면 된다.

```python
def check_if_balanced(brackets):
    return brackets.count('(') == brackets.count(')')

def split_brackets(brackets):
    u, v = brackets, ''
    for i in range(2, len(brackets), 2): # 2-2
        is_balanced = check_if_balanced(brackets[:i])
        if is_balanced:
            u, v = brackets[:i], brackets[i:]
            break
    return u, v
```

### 3. 함수구현 - 괄호 체크

올바른 괄호 문자열을 체크하는 함수를 구현해야 한다.

우선 공백을 받았으면 False를 반환한다.

```python
def check_brackets(brackets):
    if not brackets: # 3-1
        return False
```

stack을 가지고 체크를 한다. 괄호의 개수 체크는 stack의 기본 응용이고 너무 쉬운 내용이라 넘어간다.

```python
def check_brackets(brackets):
    if not brackets:
        return False

    stack = []
    for bracket in brackets: # 3-2
        if bracket == '(':
            stack.append(bracket)
        elif bracket == ')':
            if not stack or stack[-1] != '(':
                return False
            stack.pop()
    return True
```

### 4. 함수구현 - 괄호 뒤집기

`(`을 `)`로, `)`을 `(`로 변환할 함수를 Dictionary를 사용하여 편하게 구현한다.

```python
BRACKET_DIC = {"(": ")", ")": "("}

def reverse(brackets): # 4
    return ''.join([BRACKET_DIC[bracket] for bracket in brackets])
```

## Comment

때로는 도망치는게 현명한 방법인줄 알았다면 고집 피우지 말걸 그랬다. 우직한게 언제나 정답은 아니라는 걸 배웠다.

나는 이 문제를 감당할 수 있을 만큼 준비가 되었을 때 맞닥뜨렸어야 했다.
