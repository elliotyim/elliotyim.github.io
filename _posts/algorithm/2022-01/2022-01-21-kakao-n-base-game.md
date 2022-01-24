---
title: 2018 KAKAO BLIND RECRUITMENT [3차] n진수 게임
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-21 14:48:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "카카오", "블라인드", "큐"]
---

## Link

- https://programmers.co.kr/learn/courses/30/lessons/17687

## Introduction

알고리즘 문제를 풀다보면 언제나 드는 고민이 있다.

> 1, 내가 머리를 써서 좋은 방법을 생각해 컴퓨터에게 가르쳐 줄 것인가?

> 2, 주어진 조건을 컴퓨터에게 주고 시뮬레이션을 시켜 문제를 풀게 할 것인가?

![minecraft](/assets/img/algorithm/programmers/kakao/minecraft.jfif){: width="75%" height="75%"} _소프트웨어의 세계에서 우리는 무한한 자원을 가지고 뭐든 할 수 있다_

머리가 좋지 않고(~~그리고 귀찮아 하는~~) 간결한 코드를 선호하는 나는 주로 후자를 선택하곤 하는데, 이러한 경향이 코딩 스타일에도 드러나는 편이다.

그래서 DP 문제에서 시간초과가 안나면 일단 재귀를 써서 풀려고 하고(base condition과 k일 때, k+1일 때의 경우만 지정해 주면 계산이 빠른 컴퓨터가 알아서 풀어줄테니까), 지난 포스팅인 [2022 KAKAO BLIND RECRUITMENT 양궁대회](https://elliotyim.github.io/posts/kakao-archery/)에서도 더 적은 화살을 맞춘 개수를 구할 때에도 직접 골라내는 함수를 구현하기 보다 sort() 함수를 먼저 돌려보는 방식을 선택했다.

이번 문제도 그런식이었다. 숫자를 n진수로 바꾸고 자릿수를 몇으로 나누고 몇 번째 숫자만 골라서 반환하면 수행시간도 적고 좀 더 괜찮은 알고리즘이 나올 수도 있을지 모르겠지만(아마도 그럴 것이다.) 일단 컴퓨터한테 하라고 시켜보고 시간초과나면 바꿔야지라는 생각으로 조건을 주고 시뮬레이션 시켰다.

물론 절대 좋은 습관은 아닌 것 같다. 효율성을 생각하면 코드를 작성하면서 이게 수행시간이 얼마나 걸릴것인지를 꼼꼼히 따져봐야하는데...아직 갈 길이 멀은 것 같다.

## Note

- 숫자가 0부터 시작하는 것에 주의
- 10진수 이상부터는 알파벳이 들어간다.

## Solution

```python
from collections import deque

NO_DIC = {
    10: "A",
    11: "B",
    12: "C",
    13: "D",
    14: "E",
    15: "F"
}

def convert_no(no, n):
    nums, quotient = deque(), no
    while quotient > 0:
        quotient, remainder = quotient // n, int(quotient % n)
        remainder = str(remainder) if remainder < 10 else NO_DIC[remainder]
        nums.appendleft(remainder)
    return nums

def solution(n, t, m, p):
    answer = ''
    order_queue = deque([i+1 for i in range(m)])

    no = 0
    while len(answer) < t:
        nums = convert_no(no, n) if no > 0 else deque(['0'])
        while nums and len(answer) < t:
            converted_no = nums.popleft()
            answer += converted_no if order_queue[0] == p else ''
            order_queue.rotate(-1)
        no += 1
    return answer
```

1. 순서큐를 만들어준다.
2. 숫자를 0부터 시작해서 n진수로 변환한다. (맨 처음만 '0'을 그대로 쓴다.)
3. 변환한 숫자를 체크하며 순번대로 부르고 순서큐를 한 칸씩 돌린다.
