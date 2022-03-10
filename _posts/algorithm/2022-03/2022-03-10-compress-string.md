---
title: 2020 KAKAO BLIND RECRUITMENT - 문자열 압축
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-10 14:40:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2020", "카카오"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/60057](https://programmers.co.kr/learn/courses/30/lessons/60057)

## Introduction

블라인드 코딩테스트에서 이 문제를 처음 풀었을 때는 풀이 언어가 C++과 Java 밖에 선택지가 없었지만, 이 문제가 공개되고 난 뒤 Python이 선택 가능해지면서 더욱 간단하고, 로직에만 집중하는 간결한 풀이가 가능해지게 되었다.

## Note

- 자른 문자열을 순회하면서 압축할 때 문자열 순서의 기준을 어디에 두느냐에 따라 마지막 string이 체크가 안 될수 있으므로 주의

## Solution

```python
def compress(sliced_strings):
    result, count, prev = '', 0, sliced_strings[0]

    for string in sliced_strings:
        if string == prev:
            count += 1
        else:
            result += f'{count}{prev}' if count > 1 else prev
            prev = string
            count = 1

    if string == prev:
        result += f'{count}{prev}' if count > 1 else prev

    return result

def split_string(string, no):
    return [string[i:i+no] for i in range(0, len(string), no)]

def solution(s):
    answer = float('inf')
    for no in range(1, len(s)+1):
        sliced_strings = split_string(s, no)
        string = compress(sliced_strings)
        answer = min(answer, len(string))
    return answer
```

### 1. 필요한 함수 정의

간단하게 문자열을 특정 개수만큼 자르고, 압축한 뒤에 길이를 재면 된다. 이를 위해 필요한 함수를 정의한다.

```python
def compress(sliced_strings):
    pass

def split_string(string, no):
    pass

def solution(s):
    answer = 0
    return answer
```

### 2. 주어진 함수 사용

아래의 과정을 수행한다.

1. 주어진 문자열을 1개~n개씩 잘라본다.
2. 잘라진 문자열을 압축한다.
3. 압축한 문자열이 더 작으면 해당 길이를 정답으로 저장한다.

```python
def compress(sliced_strings):
    pass

def split_string(string, no):
    pass

def solution(s):
    answer = float('inf')
    for no in range(1, len(s)+1):
        sliced_strings = split_string(s, no) # 2-1
        string = compress(sliced_strings) # 2-2
        answer = min(answer, len(string)) # 2-3
    return answer
```

### 3. 함수 구현 - 문자열 자르기

주어진 문자열의 0번 인덱스 부터 마지막 인덱스까지 no개 간격으로 잘라서 리스트를 만들어 반환한다.

```python
def split_string(string, no):
    return [string[i:i+no] for i in range(0, len(string), no)] # 3-1
```

### 4. 함수 구현 - 문자열 압축

우선 필요한 변수들을 정의해 준다.

```python
def compress(sliced_strings):
    result, count, prev = '', 0, sliced_strings[0] # 4-1
    return strings
```

sliced_strings를 순회하면서 prev와 현재 string이 같다면 count를 1 올려준다.

```python
def compress(sliced_strings):
    result, count, prev = '', 0, sliced_strings[0]

    for string in sliced_strings:
        if string == prev: # 4-2
            count += 1

    return result
```

다른 경우에는 카운트를 체크해서 1 이상이면 result에 압축해서 붙여주고 아니면 그대로 붙여준 뒤에 prev와 count를 초기화 한다.

```python
def compress(sliced_strings):
    result, count, prev = '', 0, sliced_strings[0]

    for string in sliced_strings:
        if string == prev:
            count += 1
        else: # 4-3
            result += f'{count}{prev}' if count > 1 else prev
            prev = string
            count = 1

    return result
```

마지막 string이 prev와 같은 경우 그 마지막 string만 체크가 안되므로 체크해주고 result를 반환한다.

```python
def compress(sliced_strings):
    result, count, prev = '', 0, sliced_strings[0]

    for string in sliced_strings:
        if string == prev:
            count += 1
        else:
            result += f'{count}{prev}' if count > 1 else prev
            prev = string
            count = 1

    if string == prev: # 4-4
        result += f'{count}{prev}' if count > 1 else prev

    return result
```

## Comment

이 문제는 내가 알고리즘 코딩 테스트를 준비하고 처음으로 접한 실전 문제였다.

그 당시에는 이 문제 딱 하나 푸는데 자바로 끙끙대며 풀면서 2시간 정도 걸렸던것 같은데 새삼 시간이 많이 흘렀구나 하는 생각이 든다.
