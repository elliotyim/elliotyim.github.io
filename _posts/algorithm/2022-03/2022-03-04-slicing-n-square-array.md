---
title: (프로그래머스) 월간 코드 챌린지 시즌3 - n^2 배열 자르기
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-04 17:43:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "월간 코드 챌린지"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/87390](https://programmers.co.kr/learn/courses/30/lessons/87390)

## Introduction

생각하는 것이 귀찮았던 나는 범위 내에서 하나 하나 체크한 다음에 정답리스트에 넣는 순진한 방식으로 구현했고, 시간초과가 났다. (n이 천만이나 되는걸 확인도 안함)

결국 배열에 매겨진 숫자의 특성을 파악하여 문제를 푸는 방식으로 바꿨더니 문제가 해결되었다.

## Note

- n이 커서 최악의 케이스에서 O(n)이 나오면 시간 초과가 난다.
- 프로그래머스의 문제인건지 큰 숫자가 파라미터로 오면 런타임 에러가 나므로 int로 형변환 해줄 것

## Solution

```python
def solution(n, left, right):
    return [max(no//n, no%n)+1 for no in range(int(left), int(right)+1)]
```

### 1. Brute Force

생각하기 귀찮을 땐 뇌 비우고 Brute Force가 짱이다! 우선 배열을 전부 순회해 보자

```python
def solution(n, left, right):
    answer = []
    for y in range(n):
        for x in range(n): # 1-1
            pass
    return answer
```

주어진 2^n 배열을 자세히 보니 x나 y 중 더 큰 Index에 1을 더한 값이 그 위치의 값이다. 이 값을 정답에 넣어주면 된다.

![array](/assets/img/algorithm/programmers/practice/slicing-n-square-array/array.png)

```python
def solution(n, left, right):
    answer = []
    for y in range(n):
        for x in range(n):
            value = max(x, y) + 1 # 1-2
            answer.append(value)
    return answer
```

근데 값을 다 넣을 수는 없으니까, 범위 안에 있을 때만 값을 넣도록 바꿔보자. 우선 배열을 1차원 배열로 바꿔서 넘버링 해보자.

넘버링을 하는 규칙은 y의 인덱스가 하나 올라갈 때마다 n씩 값이 더해지고 거기에 x의 값을 더하면 된다.

![array](/assets/img/algorithm/programmers/practice/slicing-n-square-array/array.png)

![array2](/assets/img/algorithm/programmers/practice/slicing-n-square-array/array2.png)
_y가 몇 층인지, x가 몇 번째인지라고 생각하면 된다._

따라서 번호의 넘버링은 x + n\*y로 계산할 수 있다. 이걸 적용해주자.

1. 이 숫자가 right를 넘어가면 더 이상 체크할 필요가 없으므로 순회를 종료한다.
2. 현재 넘버링이 left와 right 사이에 있으면 정답에 추가한다.

```python
def solution(n, left, right):
    answer = []
    for y in range(n):
        for x in range(n):
            no = x + n * y
            if right < no:
                return answer
            elif left <= no <= right:
                value = max(x, y) + 1
                answer.append(value)
    return answer
```

오잉? 시간초과가 난다. 이상해서 문제를 다시 보니 n이 천만......

![time out](/assets/img/algorithm/programmers/practice/slicing-n-square-array/time-out.png)

### 2. Fun하고 Cool하고 Sexy하게 풀기

1번의 풀이의 경우 O(n) 처럼 보이지만 사실 if문을 체크하기 위해 `no = x + n * y`라는 연산을 한 번 더 하기 때문에 더 느리다.

아까 현재 위치의 값을 구하는 방법은 x나 y중 큰 수 + 1이라고 했는데, 넘버링된 숫자로도 비슷하게 구할 수 있을까?

![array](/assets/img/algorithm/programmers/practice/slicing-n-square-array/array.png)

![array2](/assets/img/algorithm/programmers/practice/slicing-n-square-array/array2.png)

우선 그림을 다시 가져와서 살펴보니 n = 3인 정사각형일 때,

x의 경우 0, 1, 2가 계속 반복 된다. 즉, `x = no % n`인 것을 알 수 있다.

y의 경우 3번에 한 번씩 값이 올라간다. 즉, `y = no // n`인 것을 알 수 있다.

이를 토대로 다시 구현해 보자. left부터 right까지의 no를 가지고 x, y를 계산해 value를 구해서 바로 리스트로 만들어 반환했다.

```python
def solution(n, left, right):
    return [max(no//n, no%n)+1 for no in range(left, right+1)]
```

![runtime error](/assets/img/algorithm/programmers/practice/slicing-n-square-array/runtime-error.png)
_이 놈의 지긋지긋한 런타임 에러..._

지난 포스팅처럼 또 큰 숫자를 그대로 썼다고 에러나는건가? 파라미터로 받은 숫자를 다루기 전에 int로 형변환 시켜주자.

```python
def solution(n, left, right):
    return [max(no//n, no%n)+1 for no in range(int(left), int(right)+1)]
```

## Comment

문제의 입출력 예에 굉장히 직관적이고 알기 쉬운 애니메이션이 있어서 정말 흥미롭게 봤던 것 같다. 프로그래머스팀의 진심을 알 수 있는 문제였다.
