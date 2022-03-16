---
title: (프로그래머스) 연습 문제 - 다음 큰 숫자
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-16 19:05:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/12911](https://programmers.co.kr/learn/courses/30/lessons/12911)

## Introduction

비트 연산 문제가 또 있길래 가져와서 정리해봤다. 뭔가 [2개 이하로 다른 비트](https://elliotyim.github.io/posts/two-different-bits/)보다 좀 더 까다로운 느낌이다.

## Note

- 홀수와 짝수의 경우로 나누어 생각한다.

## Solution

```python
def solution(n):
    if n % 2 == 1:
        rightmost_zero = (n+1) & ~n
        next_one = rightmost_zero >> 1
        return n ^ rightmost_zero ^ next_one
    else:
        rightmost_one = n & ~(n-1)
        converted_no = n + rightmost_one
        diff = bin(n).count('1') - bin(converted_no).count('1')
        return converted_no | (1 << diff)-1
```

### 1. 홀수의 경우

우선 홀수부터 보자. 예시에 나와 있는 15를 변환하면 23이 된다고 한다.

15와 23의 이진수를 살펴보자

```
15 -> 01111(2)
23 -> 10111(2)
```

![Hoxy](/assets/img/meme/thinking-meme-face.jpg)_Hoxy.._

시험삼아 적당히 151이란 숫자를 선택해 1씩 더하면서 이진수를 살펴보자

```markdown
151 -> 10010111(2) (1이 5개)
152 -> 10011000(2) (1이 3개)
153 -> 10011001(2) (1이 4개)
154 -> 10011010(2) (1이 4개)
155 -> 10011011(2) (1이 5개)
```

자세히보니 홀수일 때의 규칙은 가장 오른쪽의 0과 그 다음 1을 토글해주면 된다.

151 -> 1001`01`11(2) <br/>
155 -> 1001`10`11(2)

```python
def solution(n):
    if n % 2 == 1:
        rightmost_zero = (n+1) & ~n
        next_one = rightmost_zero >> 1
        return n ^ rightmost_zero ^ next_one
    return -1
```

해당 내용은 [2개 이하로 다른 비트](https://elliotyim.github.io/posts/two-different-bits/)에서도 유사한 케이스를 다루었기 때문에 자세한 설명은 생략한다.

### 2. 짝수의 경우

78과 83의 이진수를 살펴보자

```
78 -> 1001110(2)
83 -> 1010011(2)
```

음...그냥 봐선 모르겠는데? 한 번 78부터 1씩 더하면서 규칙을 찾아보자

```
78 -> 1001110(2) (1이 4개)
79 -> 1001111(2) (1이 5개)
80 -> 1010000(2) (1이 2개)
81 -> 1010001(2) (1이 3개)
82 -> 1010010(2) (1이 3개)
83 -> 1010011(2) (1이 4개)
```

79부터 80으로 넘어가는 순간, 그러니까 78일 때 가장 오른쪽에 있었던 1에(10011`1`0) 1이 더해지는 순간을 보자

80의 2번째 1의(10`1`0000) 뒤로는 전부 0으로 채워지는 순간부터는 그 뒤에 1이 몇개 채워지느냐로 총 1의 개수를 맞춘다.

정리하자면 짝수일 때는,

1. n의 가장 오른쪽에 있는 1에 1을 더해서 올린다.
2. 그 수에 n의 1의 개수와 똑같아 지도록 뒤에서부터 1을 채워준다.

우선 가장 오른쪽에 있는 1을 찾아준다.

```python
def solution(n):
    if n % 2 == 1:
        rightmost_zero = (n+1) & ~n
        next_one = rightmost_zero >> 1
        return n ^ rightmost_zero ^ next_one
    else:
        rightmost_one = n & ~(n-1) # 2-1
```

원래 숫자인 n에 아까 찾은 가장 오른쪽의 1의 자리에다 1을 더해 숫자를 바꿔준다.

ex) 78의 경우: 78 -> 1001110(2), rightmost_one = 10(2)

```python
def solution(n):
    if n % 2 == 1:
        rightmost_zero = (n+1) & ~n
        next_one = rightmost_zero >> 1
        return n ^ rightmost_zero ^ next_one
    else:
        rightmost_one = n & ~(n-1)
        converted_no = n + rightmost_one # 2-2
```

78 = 1001110(2)의 경우 가장 오른쪽의 1에 1을 더해서 80 = 1010000(2)로 만든 후 1을 2개 더 채워주기 위해 011(2)를 만들어 준 후 OR 연산을 한다.

이를 위해 우선 원래 n이 가지고 있던 1의 개수에서 바뀐 숫자의 1의 개수의 차이를 구한다.

```python
def solution(n):
    if n % 2 == 1:
        rightmost_zero = (n+1) & ~n
        next_one = rightmost_zero >> 1
        return n ^ rightmost_zero ^ next_one
    else:
        rightmost_one = n & ~(n-1)
        converted_no = n + rightmost_one
        diff = bin(n).count('1') - bin(converted_no).count('1') # 2-3
```

011(2)는 100(2)를 만들어 준 후 1을 빼면 된다. 100(2)는 아까 구해놨던 1의 개수의 차이만큼 1이라는 숫자의 비트를 왼쪽으로 밀어주면 된다. 이걸 바뀐 숫자와 OR 연산해준다.

```python
def solution(n):
    if n % 2 == 1:
        rightmost_zero = (n+1) & ~n
        next_one = rightmost_zero >> 1
        return n ^ rightmost_zero ^ next_one
    else:
        rightmost_one = n & ~(n-1)
        converted_no = n + rightmost_one
        diff = bin(n).count('1') - bin(converted_no).count('1')
        return converted_no | (1 << diff)-1 # 2-4
```

## Comment

어째 푸는 시간보다 정리하는 시간이 더 오래 걸리는 것 같다..이게 맞나?
