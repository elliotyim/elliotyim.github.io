---
title: (프로그래머스) 찾아라 프로그래밍 마에스터 - 게임 맵 최단거리
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-10 13:58:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "BFS"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/1844](https://programmers.co.kr/learn/courses/30/lessons/1844)

## Introduction

채용 관련된 코딩 테스트 레벨에서는 어느 정도 참고할 수 있는 템플릿 같은게 있었다. 어떻게 보면 코딩테스트를 치루는 많은 사람들이 알고 있는 기초지식 같은 것이라고 해야하나

몇몇 알고리즘은 기본 원형 같은 것이 존재하는데, 이번 문제는 BFS의 원형을 응용할 것도 없이 정석대로 사용하면 그냥 풀린다.

처음 이 유형의 문제를 접했을 때 아니 코드 길이도 긴데 왜 난이도가 이렇게 낮게 측정된거야 하고 의아해했었다.(하필 그 문제가 영역을 다른 색으로 칠하는 플러드 필 관련 문제에, 자바로 풀었었어서 코드가 더 길었었다.)

지금 와서 다시 보면 채용 코테에서 문제가 이 수준으로 나오면 변별력이 없을 것 같기도 하다.

유형을 그냥 외우기만 하면 굉장히 짧은 시간 내에 풀어버릴 수 있을 테니까

알고리즘과 관련되어서 barkingdog님의 강의를 굉장히 추천하는 편인데, 이번 문제는 [[바킹독의 실전 알고리즘] 0x09강 - BFS](https://www.youtube.com/watch?v=ftOmGdm95XI)를 들으면 이론을 굉장히 쉽게 이해할 수 있다.

## Note

- 체크하려는 곳이 유효한지를 사전에 필터링할 것
- 체크하려는 곳의 거리가 `지금 내가 있는 곳의 거리 + 1`보다 크면 갱신해준다.

## Solution

```python
from collections import deque

DX = [1, 0, -1, 0]
DY = [0, 1, 0, -1]

def solution(maps):
    distances = [[-1 for i in maps[0]] for j in maps]
    queue = deque()
    queue.append([0,0])
    distances[0][0] = 1

    while queue:
        cur_x, cur_y = queue.popleft()
        for i in range(4):
            x, y = cur_x + DX[i], cur_y + DY[i]
            if not 0 <= x < len(maps[0]) or not 0 <= y < len(maps) or maps[y][x] == 0:
                continue

            if distances[y][x] == -1 or distances[y][x] > distances[cur_y][cur_x] + 1:
                queue.append([x, y])
                distances[y][x] = distances[cur_y][cur_x] + 1

    return distances[-1][-1]
```

**풀이**

### 1. 방문한 곳을 표시할 배열과 큐를 준비

BFS는 큐를 사용하여 구현하는데, 현재 지점으로부터 인접한 지점을 차례대로 큐에 넣고 그 순서대로 방문하기 때문에 탐색할 수 있는 모든 지점을 거리순으로 순차적으로 모두 방문한다.

이러한 특성을 이용해서 게임 맵을 순차적으로 방문하고 방문할 때마다 `기준 지점의 거리 + 1`만큼 더해 나가면 된다.

이를 위해 큐와 distances라는 배열을 준비하고 큐에 시작점의 좌표(0, 0)를 넣어준뒤 시작점의 거리를 갱신해 준다.

(거리를 고려하지 않는 경우 템플릿에서는 이 distances라는 변수명이 보통 visited라는 이름으로 정의된다.)

```python
from collections import deque

def solution(maps):
    distances = [[-1 for i in maps[0]] for j in maps]
    queue = deque()
    queue.append([0, 0])
    distances[0][0] = 1
```

### 2. 큐 안의 좌표 순회

큐 안에 좌표가 남아 있는 동안은 계속 큐에서 좌표를 꺼낸다.

```python
from collections import deque

def solution(maps):
    distances = [[-1 for i in maps[0]] for j in maps]
    queue = deque()
    queue.append([0, 0])
    distances[0][0] = 1

    while queue: # 2
        cur_x, cur_y = queue.popleft()
```

### 3. 큐 안의 좌표를 기준으로 상하좌우 탐색

![search](/assets/img/algorithm/programmers/practice/shortest-distance/search2.png)

위의 그림 처럼 기준점의 상하좌우 4방향을 탐색하려면 원래 좌표에서 1을 더하거나 빼주면 된다. 이를 코드로 표현해 보자.

current_x와 current_y를 기준으로 DX, DY를 각각 더한 x, y를 구한다.

```python
from collections import deque

DX = [1, 0, -1, 0]
DY = [0, 1, 0, -1]

def solution(maps):
    distances = [[-1 for i in maps[0]] for j in maps]
    queue = deque()
    queue.append([0,0])
    distances[0][0] = 1

    while queue:
        cur_x, cur_y = queue.popleft()
        for i in range(4): # 3
            x, y = cur_x + DX[i], cur_y + DY[i]
```

### 4. 유효한 칸이 아니면 걸러내기

체크하려는 x와 y가 유효하지 않으면 넘어간다.

1. x, y가 게임맵을 벗어났다.
2. maps의 (x, y) 좌표가 길이 아니다.(= 0)

```python
from collections import deque

DX = [1, 0, -1, 0]
DY = [0, 1, 0, -1]

def solution(maps):
    distances = [[-1 for i in maps[0]] for j in maps]
    queue = deque()
    queue.append([0,0])
    distances[0][0] = 1

    while queue:
        cur_x, cur_y = queue.popleft()
        for i in range(4):
            x, y = cur_x + DX[i], cur_y + DY[i]
            if not 0 <= x < len(maps[0]) or not 0 <= y < len(maps) or maps[y][x] == 0: # 4
                continue
```

### 5. 거리를 갱신

최단거리를 갱신하는 조건은 아래와 같다.

1. 방문한 적이 없어 거리 값이 `-1`일 때
2. 체크하려는 곳이 `기준점 + 1`보다 더 거리값이 클 때

이런 곳을 만나면 거리 값을 갱신해주고 해당 칸을 큐에 넣어 준다.

순회가 끝나면 가장 마지막 칸의 값을 반환하면 된다. 시작점과 길이 이어져 있다면 거리 값이 나올 것이고, 그렇지 않다면 처음에 넣어준 -1이 반환된다.

```python
from collections import deque

DX = [1, 0, -1, 0]
DY = [0, 1, 0, -1]

def solution(maps):
    distances = [[-1 for i in maps[0]] for j in maps]
    queue = deque()
    queue.append([0,0])
    distances[0][0] = 1

    while queue:
        cur_x, cur_y = queue.popleft()
        for i in range(4):
            x, y = cur_x + DX[i], cur_y + DY[i]
            if not 0 <= x < len(maps[0]) or not 0 <= y < len(maps) or maps[y][x] == 0:
                continue

            if distances[y][x] == -1 or distances[y][x] > distances[cur_y][cur_x] + 1: # 5
                queue.append([x, y])
                distances[y][x] = distances[cur_y][cur_x] + 1

    return distances[-1][-1]
```

## Comment

처음 스택과 큐라고 하는 자료구조를 공부하고 이걸 응용한 DFS, BFS를 통한 탐색에 대해서 배웠을 때 정말 소름이 돋았다.

"얼핏 보면 전혀 관려 없어 보이는 이 자료구조를 응용해서 어떻게 이런 알고리즘을 만들어 낼 수 있었을까 정말 천재들만 있나?"

주어진 도구들을 이용해 어떤 창의적인 방법들을 만들어내는 개발자들의 행보를 보면 정말 감탄 밖에 나오지 않는다.

나는 이런 업계에 있구나하는 생각이 들어 가슴이 벅차 오른다. 뭐 아직은 견습생과도 같은 신분이지만.
