---
title: (프로그래머스) Summer/Winter Coding(~2018) - 방문 길이
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-14 14:35:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "서머/윈터코딩"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/49994](https://programmers.co.kr/learn/courses/30/lessons/49994)

## Introduction

게임 판에서 게임 캐릭터가 움직이면서 벌어지는 일들을 묘사해주면 되는 문제다.

이런 일을 간단하게 할 수 있도록 도움을 준 기본 라이브러리들에 감사할 따름이다.

## Note

- 어떤 지점에 도착했느냐로 체크 하는게 아니라, 지점과 지점을 잇는 간선을 지나갔느냐를 체크 하는 것에 주의
- 같은 길은 두 방향으로 똑같이 지나갈 수 있다. 중복으로 체크하지 않도록 주의

## Solution

```python
DISTANCE = {
    "U": (0, 1),
    "D": (0, -1),
    "L": (-1, 0),
    "R": (1, 0),
}

def solution(dirs):
    visited = dict()
    answer, current = 0, [0, 0]

    for direction in dirs:
        from_x, from_y = current
        dx, dy = DISTANCE[direction]
        to_x, to_y = from_x + dx, from_y + dy

        if not -5 <= to_x <= 5 or not -5 <= to_y <= 5:
            continue

        if (from_x, from_y, to_x, to_y) not in visited:
            answer += 1

        visited[(from_x, from_y, to_x, to_y)] = True
        visited[(to_x, to_y, from_x, from_y)] = True

        current = to_x, to_y

    return answer
```

### 1. 필요한 좌표 사전/방문 사전 준비

게임 캐릭터는 U, D, L, R 4방향으로만 움직인다. 현재 좌표에서 입력받은 커맨드로 바로 좌표를 움직일 수 있게 커맨드와 좌표를 매핑한 사전을 준비한다.

현재 위치는 (0, 0) 위치에서 부터 시작한다.

```python
DISTANCE = { # 1
    "U": (0, 1),
    "D": (0, -1),
    "L": (-1, 0),
    "R": (1, 0),
}

def solution(dirs):
    visited = dict()
    answer, current = 0, [0, 0]
```

### 2. 실제로 게임 캐릭터를 이동

현재 위치를 from_x, from_y로 두고 입력받은 커맨드에 해당하는 만큼의 거리를 계속해서 이동해 준다.

```python
DISTANCE = {
    "U": (0, 1),
    "D": (0, -1),
    "L": (-1, 0),
    "R": (1, 0),
}

def solution(dirs):
    visited = dict()
    answer, current = 0, [0, 0]

    for direction in dirs: # 2
        from_x, to_x = current
        dx, dy = DISTANCE[direction]
        to_x, to_y = from_x + dx, from_y + dy

        current = to_x, to_y
```

### 3. 예외상황 고려

이동을 하려는 곳이 맵 밖을 벗어났을 수도 있다. 이런 경우는 제외한다.

```python
DISTANCE = {
    "U": (0, 1),
    "D": (0, -1),
    "L": (-1, 0),
    "R": (1, 0),
}

def solution(dirs):
    visited = dict()
    answer, current = 0, [0, 0]

    for direction in dirs:
        from_x, to_x = current
        dx, dy = DISTANCE[direction]
        to_x, to_y = from_x + dx, from_y + dy

        if not -5 <= to_x <= 5 or not -5 <= to_y <= 5: # 3
            continue

        current = to_x, to_y
```

### 4. 지나가지 않은 길 체크

지나가려는 길은 두 방향 다 한 번으로 체크되어야 한다. 갈 때 한 번, 올 때 한 번이다.

한 번도 지나간적이 없는 길이라면 길의 갯수를 하나 올려준다.

```python
DISTANCE = {
    "U": (0, 1),
    "D": (0, -1),
    "L": (-1, 0),
    "R": (1, 0),
}

def solution(dirs):
    visited = dict()
    answer, current = 0, [0, 0]

    for direction in dirs:
        from_x, to_x = current
        dx, dy = DISTANCE[direction]
        to_x, to_y = from_x + dx, from_y + dy

        if not -5 <= to_x <= 5 or not -5 <= to_y <= 5:
            continue

        if (from_x, from_y, to_x, to_y) not in visited: # 4
            visited[(from_x, from_y, to_x, to_y)] = True
            visited[(to_x, to_y, from_x, from_y)] = True
            answer += 1

        current = to_x, to_y

    return answer
```

## Comment

처음에는 지점을 체크해야하는걸로 착각해서 삽질을 조금 했지만, 게임 문제라 이해하기도 쉽고 금방금방 재밌게 풀었던 것 같다.
