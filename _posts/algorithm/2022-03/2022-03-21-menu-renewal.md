---
title: 2021 KAKAO BLIND RECRUITMENT - 메뉴 리뉴얼
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-21 22:48:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2021", "카카오", "블라인드"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/72411](https://programmers.co.kr/learn/courses/30/lessons/72411)

## Introduction

주문된 요리들을 가지고 만들 수 있는 메뉴 조합을 전부 생각한 뒤 그 중에 2명 이상의 주문한 같은 메뉴의 조합을 골라서 반환하면 되는 문제이다.

## Note

- orders의 메뉴가 정렬되어 있지 않다는 점에 주의

## Solution

```python
from itertools import combinations
from collections import defaultdict

def pick_menu(candidates):
    result = []
    for menu in candidates.values():
        for m, count in menu:
            result.append(m)
    return result

def create_menu_candidates(ordered_menu):
    candidates = defaultdict(list)

    for menu, count in ordered_menu.items():
        if count == 1:
            continue

        course_length = len(menu)
        if course_length not in candidates:
            pass
        elif count < candidates[course_length][0][1]:
            continue
        elif count > candidates[course_length][0][1]:
            candidates[course_length] = []
        candidates[course_length].append((menu, count))

    return candidates

def count_orders(orders, course):
    ordered_menu = defaultdict(int)
    for order in orders:
        for c in course:
            for o in combinations(sorted(order), c):
                menu = ''.join(o)
                ordered_menu[menu] += 1
    return ordered_menu

def solution(orders, course):
    ordered_menu = count_orders(orders, course)
    candidates = create_menu_candidates(ordered_menu)
    course_menu = pick_menu(candidates)
    return sorted(course_menu)
```

### I. 필요한 함수 정의

우선 orders와 course를 넣어주면 아래와 같이 `key:value`가 `메뉴조합:주문량`인 ordered_menu를 반환하는 함수를 정의한다.

```json
{
  "AB": 1,
  "AC": 4,
  "CDE": 3,
  "BCFG": 2
}
```

```python
def count_orders(orders, course):
    pass

def solution(orders, course):
    ordered_menu = count_orders(orders, course) # 1-1
```

그리고, 이 ordered_menu를 넣으면 아래와 같이 `key:value`가 `코스메뉴개수:[메뉴, 주문량]`인 candidates를 반환하는 함수를 정의한다.

```json
{
  "2": [["AC", 4]],
  "3": [["CDE", 3]],
  "4": [
    ["BCFG", 2],
    ["ACDE", 2]
  ]
}
```

```python
def create_menu_candidates(ordered_menu):
    pass

def count_orders(orders, course):
    pass

def solution(orders, course):
    ordered_menu = count_orders(orders, course)
    candidates = create_menu_candidates(ordered_menu) # 1-2
```

이 메뉴 후보들을 적절한 1차원 리스트로 바꿔주는 함수를 정의하고, 이 리스트를 알파벳 오름차순으로 정렬한 뒤 반환한다.

```python
def pick_menu(candidates):
    pass

def create_menu_candidates(ordered_menu):
    pass

def count_orders(orders, course):
    pass

def solution(orders, course):
    ordered_menu = count_orders(orders, course)
    candidates = create_menu_candidates(ordered_menu)
    course_menu = pick_menu(candidates) # 1-3
    return sorted(course_menu)
```

### II. 함수구현 - 주문 세기

우선 ordered_menu를 준비한다.

```python
from collections import defaultdict

def count_orders(orders, course):
    ordered_menu = defaultdict(int) # 2-1
    return ordered_menu
```

orders에 있는 order를 하나씩 꺼내서 (2-2)<br/>
course별로 (2-3)<br/>
메뉴 조합을 만든 뒤 (2-4)<br/>
이 메뉴 조합의 개수를 세준다. (2-5)

```python
from collections import defaultdict

def count_orders(orders, course):
    ordered_menu = defaultdict(int)
    for order in orders: # 2-2
        for c in course: # 2-3
            for o in combinations(sorted(order), c): # 2-4
                menu = ''.join(o)
                ordered_menu[menu] += 1 # 2-5
    return ordered_menu
```

### III. 함수구현 - 메뉴 후보 생성

메뉴 조합 중에서도 `2명 이상`이 시킨 메뉴 조합 중 `가장 인기 있는 메뉴 조합`만 유효하므로 모든 메뉴 조합에서 유효한 메뉴조합만 필터링해준다.

```python
from collections import defaultdict

def create_menu_candidates(ordered_menu):
    candidates = defaultdict(list)

    for menu, count in ordered_menu.items():
        if count == 1:
            continue

        course_length = len(menu)
        if course_length not in candidates:
            pass
        elif count < candidates[course_length][0][1]:
            continue
        elif count > candidates[course_length][0][1]:
            candidates[course_length] = []
        candidates[course_length].append((menu, count))

    return candidates
```

우선 value를 list로 갖는 candidates 딕셔너리를 준비한다.

```python
from collections import defaultdict

def create_menu_candidates(ordered_menu):
    candidates = defaultdict(list) # 3-1
    return candidates
```

ordered_menu를 순회할 건데, 1번 밖에 주문 안된 메뉴들은 다 걸러준다.

```python
from collections import defaultdict

def create_menu_candidates(ordered_menu):
    candidates = defaultdict(list)

    for menu, count in ordered_menu.items():
        if count == 1: # 3-2
            continue

    return candidates
```

아래의 기준에 따라 처리한다.

3-3. 후보에 없는 코스 메뉴면 넣어준다.

3-4. 후보에 있는 코스 메뉴의 주문량이 현재 메뉴의 주문량보다 많으면 넘어간다.

3-5. 후보에 있는 코스 메뉴의 주문량이 현재 메뉴의 주문량보다 적으면 코스 메뉴에 담겨 있던 메뉴들을 다 없앤다.

3-6. 마지막에는 현재 코스 메뉴를 후보에 넣어준다.

```python
from collections import defaultdict

def create_menu_candidates(ordered_menu):
    candidates = defaultdict(list)

    for menu, count in ordered_menu.items():
        if count == 1:
            continue

        course_length = len(menu)
        if course_length not in candidates: # 3-3
            pass
        elif count < candidates[course_length][0][1]: # 3-4
            continue
        elif count > candidates[course_length][0][1]: # 3-5
            candidates[course_length] = []
        candidates[course_length].append((menu, count)) # 3-6

    return candidates
```

### IV. 함수구현 - 메뉴 가공

코스 메뉴 후보의 Dictionary에서 메뉴만 빼서 1차원 배열에 담아주면 된다.

```python
def pick_menu(candidates):
    result = []
    for menu in candidates.values():
        for m, count in menu:
            result.append(m)
    return result
```

## Comment

뭔가 풀긴 풀었는데 피로가 쌓인 상태에서 풀어서 그런지 생각했던 것보다 횡설수설하고 코드도 깔끔하지 않은 이상한 풀이가 된 것 같다.

이건 나중에 다시 풀어봐야지
