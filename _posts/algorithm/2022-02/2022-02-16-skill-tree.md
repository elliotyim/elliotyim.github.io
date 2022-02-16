---
title: (프로그래머스) Summer/Winter Coding(~2018) - 스킬트리
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-16 10:52:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "서머/윈터코딩"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/49993](https://programmers.co.kr/learn/courses/30/lessons/49993)

## Introduction

스킬 트리 중 선행스킬과 관련된 스킬만 뽑아서 순서대로 배웠는지만 체크하면 되는 간단한 문제이다.

## Note

- 특이사항 없음

## Solution

```python
def solution(skill, skill_trees):
    answer, skill_set = 0, set(skill) # 1
    for skill_tree in skill_trees:
        required_skills = ''.join([s for s in skill_tree if s in skill_set]) # 2
        if skill.startswith(required_skills): # 3
            answer += 1
    return answer
```

1. 비교 연산의 속도를 빠르게 하기 위해 skill로 집합을 만든다.
2. 스킬트리를 하나씩 순회하면서 선행 스킬만 뽑아 string을 다시 만든다.
3. 선행 스킬을 순서대로 배웠는지 체크한다.

## Comment

문자열과 배열을 손쉽게 다룰 수 있는 파이썬의 장점이 부각되는 문제였다.

정말 알고리즘 한정 최고의 언어라고 생각한다.
