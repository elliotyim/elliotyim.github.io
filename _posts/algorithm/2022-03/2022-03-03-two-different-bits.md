---
title: (프로그래머스) 월간 코드 챌린지 시즌2 - 2개 이하로 다른 비트
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-03 15:09:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "월간 코드 챌린지"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/12913](https://programmers.co.kr/learn/courses/30/lessons/12913)

## Introduction

일반적인 코딩은 사람이 읽을 수 있는 스토리가 담긴 대화지만, 비트 연산은 컴퓨터와의 대화와 가깝다.

프로그래머는 컴퓨터와도 대화를 할 수 있어야 하는 직업임에도 불구하고 비트 연산이 약했던 나는 이번 문제를 푸는데에 엄청나게 많은 시간을 허비했다

## Note

- 비트 연산할 때 자바스크립트나 파이썬과 같이 long long 타입 등 큰 숫자를 다루는 타입이 없는 언어는 에러가 나는 경우가 있으므로 주의

## Solution

```python
def solution(numbers):
    answer = []
    for no in numbers:
        no = int(no)
        if no % 2 == 0:
            answer.append(no+1)
            continue

        rightmost_zero = (no+1) & ~no
        next_one = rightmost_zero >> 1

        no = no ^ rightmost_zero ^ next_one
        answer.append(no)
    return answer
```

### 1. 케이스 나누기

우선 숫자를 1부터 2진수로 쭉 나열해보자

```
1 -> 1 (2)
2 -> 10 (2)
3 -> 11 (2)
4 -> 100 (2)
5 -> 101 (2)
6 -> 110 (2)
...
```

여기서 짝수만 골라보면 당연한 얘기지만 다 0으로 끝난다.

```
2 -> 10 (2)
4 -> 100 (2)
6 -> 110 (2)
```

여기서 1을 더한다면?

```
2 -> 10 (2)   : 3 -> 11 (2)
4 -> 100 (2)  : 5 -> 101 (2)
6 -> 110 (2)  : 7 -> 111 (2)
```

오잉? 짝수의 경우는 손쉽게 문제가 해결된다. 달라진 비트가 딱 1개이면서 기존의 수보다 더 크다.

그럼 홀수의 경우는 어떨까? 대충 151이라는 숫자를 가져와 본다. 이 숫자를 이진수로 바꾸면 아래와 같은 느낌이다. (절대로 뮤 스티커 안 나와서 이러는거 아님)

```
151 -> 10010111 (2)
```

여기도 일단 1을 더해볼까?

```
152 -> 10011000 (2)
```

기존 숫자보다 커지긴 했는데 달라진 비트 수가 4개이다.(1001` 1``0``0``0 `) 이걸 2개 이하로 맞추려면 가장 오른쪽에서 첫 번째와 두 번째 비트를 1로 바꿔야 한다.

```
155 -> 10011011 (2)
(기존 숫자: 151 -> 10010111 (2))

151 -> 10010111 (2)
155 -> 10011011 (2)
```

![thinking](/assets/img/meme/thinking-meme-face.jpg){: width="33%" height="33%"}
_Hoxy?_

다른 숫자도 해볼까? 559를 가져와 1을 더해봤다.

```
559 -> 1000101111 (2)
560 -> 1000110000 (2)
```

그리고 다른 비트의 수를 2개 까지로 맞추기 위해 뒤쪽의 0을 전부 1로 바꿔주면...

```markdown
559 -> 10001`01`111 (2)
567 -> 10001`10`111 (2)
```

패턴이 보인다. 가장 오른쪽의 0, 그 바로 오른쪽의 1, 이 2개를 토글하면 된다.

### 2. 구현

우선 짝수의 경우는 원래 숫자에 1을 더해서 정답 리스트에 넣어준다.

```python
def solution(numbers):
    answer = []
    for no in numbers:
        if no % 2 == 0:
            answer.append(no+1) # 2-1
```

홀수는 가장 오른쪽 0의 자리와 그 바로 오른쪽에 위치한 1의 자리를 찾는다.

어떤 숫자의 가장 오른쪽에 위치한 비트의 위치를 찾는 법과 토글하는 법은 [Bitwise Operation & Bitmask](https://elliotyim.github.io/posts/bitwise-bitmask/)를 참고

```python
def solution(numbers):
    answer = []
    for no in numbers:
        if no % 2 == 0:
            answer.append(no+1)
            continue

        rightmost_zero = (no+1) & ~no # 2-2
        next_one = rightmost_zero >> 1 # 2-2
```

찾았으면 토글해주고 정답리스트에 넣는다.

```python
def solution(numbers):
    answer = []
    for no in numbers:
        if no % 2 == 0:
            answer.append(no+1)
            continue

        rightmost_zero = (no+1) & ~no
        next_one = rightmost_zero >> 1

        no = no ^ rightmost_zero ^ next_one # 2-3
        answer.append(no) # 2-3
    return answer
```

근데 7, 8, 9번의 케이스에서 런타임 에러가 난다.

![runtime error](/assets/img/algorithm/programmers/practice/two-different-bits/runtime-error.png)

테스트 케이스 안에 포함된 엄청 큰 숫자(long long 정도 되는)를 NOT 연산할 때 문제가 생기는 것 같다. (다른 행동을 하지 않고 그냥 for문 돌면서 NOT 연산만 해도 런타임 에러가 발생한다.)

신기한건 다른 언어로 하면 에러가 안 난다는 점이다.

```c++
#include <string>
#include <vector>

using namespace std;

vector<long long> solution(vector<long long> numbers) {
    vector<long long> answer;
    for (long long no : numbers)
    {
        if (no % 2 == 0)
        {
            answer.push_back(no+1);
            continue;
        }
        long long rightmost_zero = (no+1) & ~no;
        long long next_one = rightmost_zero >> 1;
        no = no ^ rightmost_zero ^ next_one;
        answer.push_back(no);
    }
    return answer;
}
```

시험삼아 Python Console을 켜고 C++ long long 타입의 상한 숫자 9,223,372,036,854,775,807에다가 10을 곱한 수를 NOT 연산 해봤는데도 문제가 일어나지 않았다.

아무래도 numbers의 길이가 MAX이면서 엄청나게 큰 숫자들만 계속 다루다 보면 메모리를 할당하다가 에러가 나는 것 같다.

이를 해결하기 위해 숫자를 다루기 전에 int로 형변환 해주면 해결이 된다는데 도무지 알 수가 없는 미스테리...<br/>(문제의 질문하기란에 질문을 남겼는데 누군가가 이것을 해결하는 방법은 제시해주셨지만 왜 이렇게 되는지 이유를 설명해주는 사람은 아무도 없었다.)

```python
def solution(numbers):
    answer = []
    for no in numbers:
        no = int(no) # 2-4
        if no % 2 == 0:
            answer.append(no+1)
            continue

        rightmost_zero = (no+1) & ~no
        next_one = rightmost_zero >> 1

        no = no ^ rightmost_zero ^ next_one
        answer.append(no)
    return answer
```

## Comment

비트 연산 넘모 어려운 것...근데 웹 개발하면서 비트 연산을 쓰는 경우는 많이 못 본 것 같다. 아무래도 가독성의 문제 때문이려나?

아니면...이런건가?

![past-developer](/assets/img/algorithm/programmers/practice/two-different-bits/past-developers.png)

![recent-developer](/assets/img/algorithm/programmers/practice/two-different-bits/recent-developers.png)
