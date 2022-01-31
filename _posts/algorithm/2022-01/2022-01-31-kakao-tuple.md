---
title: 2019 카카오 개발자 겨울 인턴십 튜플
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-31 19:49:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2019", "카카오", "정규표현식"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/64065](https://programmers.co.kr/learn/courses/30/lessons/64065)

## Introduction

짧은 코드를 신봉하던 시절이 있었다. 읽어야 할 것도 적고 고려해야 할 것도 적기 때문에 짧을수록 다른 사람이 더 이해하기 좋은 코드라고 생각했었다.

물론 이건 반은 맞고 반은 틀린 이야기. 아무리 짧은 코드라도 이해하기 난해한 코드도 있다. 특히 어떤 언어에 종속된 문법에 의존할수록 더 그런 경우가 많은 것 같다.

그런 코드일수록 스토리가 담긴 문장보다는 무언가 자기네들끼리만 아는 규칙으로 도배해놓은 수학 공식 같은 느낌이 강하게 드는 코드가 많았다.

짧지만 농도가 짙은 한 줄은 오히려 그 한 줄을 해석하는데 더 많은 정신적 에너지를 소모해야 한다. 차라리 그걸 2~3줄로 풀어 설명하는 것이 훨씬 나을수도 있다.

## Note

- 튜플 안에 담기는 원소의 순서는 가장 많이 출현한 숫자의 순서이다.

## Solution

```python
import re
from collections import defaultdict

def solution(s):
    dic, no_sets = defaultdict(int), re.findall(r'(\{[\d,?]+\})', s)
    for no_set in no_sets:
        for no in no_set[1:-1].split(','):
            dic[no] += 1
    return list(map(lambda x: int(x[0]), sorted(dic.items(), key=lambda y: -y[1])))
```

## Comment

이번 코드는 일부러 짧지만 가독성이 나쁘게 작성해보았다. 그리고 하나쯤 포스트로 남겨놓고 이런 나쁜 습관을 몸에 지니지 않게 기억해두는 것도 좋겠다고 생각했다.

최근의 코딩테스트에는 언어의 제한이 없다. 이건 어떤 언어로 작성해도 이해하기 쉽게 내용이 명료한 방식으로 코드를 작성하라는 의미가 아닐까? ~~그러니까 제발 시간제한을 짧게 해서 마감에 쫓기듯이 코딩하지 않게 해주세요~~
