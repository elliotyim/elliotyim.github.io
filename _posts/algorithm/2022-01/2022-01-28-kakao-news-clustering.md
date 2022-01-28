---
title: 2018 KAKAO BLIND RECRUITMENT [1차] 뉴스 클러스터링
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-28 11:10:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "카카오", "블라인드", "집합"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/17683](https://programmers.co.kr/learn/courses/30/lessons/17683)

## Introduction

다중집합이라는 키워드를 보고 바로 set을 썼다가 중복된 원소 다루는 게 까다로워서 dictionary를 써서 풀었는데 다른 사람 풀이 보니까 set을 쓰고도 잘만 푼거 보고 놀랐다.

언제나 다른 사람들의 풀이를 보고 많이 배운다. 참 대단한 사람들이 많은듯

집합(Hashed Set)은 정말 매력적인 자료구조인 것 같다. 엄청 많은 데이터를 중복 없이 저장하면서 해당 자료를 O(1)로 가져오기 때문에 n이 많은 경우를 고려하는 알고리즘 문제에서 사용하기에 너무 좋다.

## Note

- 특수문자나 숫자가 포함된 문자열을 버리는 타이밍이 문자쌍을 만드는 순간이어야 한다.
- 교집합, 합집합이 맞게 잘 만들었는지 체크

## Solution

```python
import re
from collections import defaultdict

def get_valid_str(s):
    result = re.findall(r'[A-Za-z]', s)
    return result[0] if result else ''

def get_dict(string):
    result = defaultdict(int)
    for i in range(1, len(string)):
        s1 = get_valid_str(string[i-1])
        s2 = get_valid_str(string[i])
        if len(s1+s2) == 2:
            result[s1+s2] += 1
    return result

def solution(str1, str2):
    s1, s2 = str1.lower(), str2.lower()
    intersection, union = [], []

    s1_dict = get_dict(s1)
    s2_dict = get_dict(s2)

    for k, v in s1_dict.items():
        if k in s2_dict:
            most = max(s1_dict[k], s2_dict[k])
            least = min(s1_dict[k], s2_dict[k])
            union += [k] * most
            intersection += [k] * least
        else:
            union += [k] * v

    for k, v in s2_dict.items():
        if k not in s1_dict:
            union += [k] * v

    answer = len(intersection) / len(union) if len(union) != 0 else 1
    return int(answer * 65536)
```

---

아래는 set과 내장함수를 이용한 다른 사람의 멋진 풀이다.

- isalpha()는 해당 문자열이 알파벳으로만 이루어져 있는지를 체크한다.

```python
def solution(str1, str2):

    list1 = [str1[n:n+2].lower() for n in range(len(str1)-1) if str1[n:n+2].isalpha()]
    list2 = [str2[n:n+2].lower() for n in range(len(str2)-1) if str2[n:n+2].isalpha()]

    tlist = set(list1) | set(list2)
    res1 = [] #합집합
    res2 = [] #교집합

    if tlist:
        for i in tlist:
            res1.extend([i]*max(list1.count(i), list2.count(i)))
            res2.extend([i]*min(list1.count(i), list2.count(i)))

        answer = int(len(res2)/len(res1)*65536)
        return answer

    else:
        return 65536
```
