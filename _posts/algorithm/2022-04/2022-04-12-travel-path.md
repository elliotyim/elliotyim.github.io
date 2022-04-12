---
title: (프로그래머스) 연습 문제 - 여행경로
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-04-12 11:30:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "DFS", "BFS"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/43164](https://programmers.co.kr/learn/courses/30/lessons/43164)

## Introduction

탐색 문제는 어떻게 하면 탐색을 덜 하고 정답을 찾을 것인지가 중요하다.

처음 이 문제를 풀 때는 n이 10,000으로 작아서 무슨 짓을 해도 통과가 될 것 같아 백트래킹으로 풀어버렸지만, 백트래킹은 잘못 사용하면 콜 스택 초과나 불필요하게 수행시간이 늘어나기 때문에 다른 방식의 풀이도 정리하게 됐다.

## Note

- 이어져 있는 길들을 전부 탐색하되, 남는 경로가 없도록 탐색한 경로들을 이어 붙인다.

## Solution

```python
import heapq
from collections import deque, defaultdict

def solution(tickets):
    graph = defaultdict(list)
    for _from, _to in tickets:
        heapq.heappush(graph[_from], _to)

    stack, path = ["ICN"], deque()
    while stack:
        _from = stack[-1]
        if graph[_from]:
            stack.append(heapq.heappop(graph[_from]))
        else:
            path.appendleft(stack.pop())
    return list(path)
```

### I. 그래프 생성

우선 각 공항에서 어떤 공항으로 이어져 있는지를 파악하기 위해 아래와 같은 티켓 경로 그래프를 만들어준다.

```json
{
  "ICN": ["ATL", "SFO"],
  "SFO": ["ATL"],
  "ATL": ["ICN", "SFO"]
}
```

```python
from collections import defaultdict

def solution(tickets):
    graph = defaultdict(list) # 1-1
```

조건 중에는 `만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 한다`라는 조건이 있다. 따라서, 처음부터 알파벳 순으로 정렬시키기 위해 heapq를 사용하여 정렬된 상태로 graph의 리스트에 넣어준다.

```python
import heapq
from collections import defaultdict

def solution(tickets):
    graph = defaultdict(list)
    for _from, _to in tickets:
        heapq.heappush(graph[_from], _to) # 1-2
```

### II. 탐색

예를 들어, 이런 형태의 공항을 생각해보자.

```
tickets = [["ICN", "B"], ["B", "ICN"], ["ICN", "A"], ["A", "D"], ["D", "A"]]
```

이 공항을 그래프로 변환하면 다음과 같다.

```json
{
  "ICN": ["A", "B"],
  "B": ["ICN"],
  "A": ["D"],
  "D": ["A"]
}
```

"ICN"에서 시작할 때,

```
"ICN"->"A"->"D"->"A"
```

로 티켓을 써서 A에 도착하고 나면 티켓 경로 그래프는 아래와 같이 바뀐다.

```json
{
  "ICN": ["B"],
  "B": ["ICN"],
  "A": [],
  "D": []
}
```

티켓이 남아버린다. 남는 경로는 `"ICN"->"B"->"ICN"`이다.

이 `"ICN"->"A"->"D"->"A"` 경로가 분명 틀린 경로는 아니지만 이렇게 가면 모든 티켓을 사용하지 못하기 때문에 이 경우 해당 경로 트래킹 결과를 저장해 둘 필요가 있다.

`"ICN"->"A"->"D"->"A"`의 경로의 앞에
시작점인 "ICN"으로 다시 돌아올 수 있는 `"ICN"->"B"->"ICN"` 경로를 붙이는 것이다.

```
"ICN"->"B"->"ICN"->"A"->"D"->"A"
```

이를 구현하기 위해 stack과 path(deque)를 준비한다. 항상 "ICN" 공항에서 출발하므로 stack에는 "ICN"을 넣어준다.

```python
import heapq
from collections import deque, defaultdict

def solution(tickets):
    graph = defaultdict(list)
    for _from, _to in tickets:
        heapq.heappush(graph[_from], _to)

    stack, path = ["ICN"], deque()
```

아래와 같은 과정을 따른다.

1, 스택의 가장 위에 있는 공항에서 다른 공항으로 갈 수 있는 티켓이 있는 경우 그 공항을 stack에 넣어 준다.

```python
import heapq
from collections import deque, defaultdict

def solution(tickets):
    graph = defaultdict(list)
    for _from, _to in tickets:
        heapq.heappush(graph[_from], _to)

    stack, path = ["ICN"], deque()
    while stack:
        _from = stack[-1]
        if graph[_from]:
            stack.append(heapq.heappop(graph[_from])) # 2-1
```

2, 아까와 같이 더 이상 갈 곳이 없는 경우 ("ICN"->"A"->"D"->"A") 해당 경로들을 path 큐에 차례로 집어넣는다. (이 때, 시작점 "ICN"에는 아직 갈 수 있는 경로가 남아있으므로 path에는 "A"->"D"->"A" 까지만 담긴다.)

```python
import heapq
from collections import deque, defaultdict

def solution(tickets):
    graph = defaultdict(list)
    for _from, _to in tickets:
        heapq.heappush(graph[_from], _to)

    stack, path = ["ICN"], deque()
    while stack:
        _from = stack[-1]
        if graph[_from]:
            stack.append(heapq.heappop(graph[_from]))
        else:
            path.appendleft(stack.pop()) # 2-2
```

이 과정을 반복하면 "ICN"에서 갈 수 있는 곳이 없어질 때까지 stack에 "ICN"에서 부터 갈 수 있는 공항 싸이클을 저장해뒀다가 path 큐에 넣는 작업을 오른쪽에서 부터 붙여나가 경로를 완성 시킬 수 있다.

```
1. path: "A"->"D"->"A"
2. path: "ICN"->"B"->"ICN"->"A"->"D"->"A"
```

queue는 리스트가 아니므로 list로 변환해서 반환한다.

```python
import heapq
from collections import deque, defaultdict

def solution(tickets):
    graph = defaultdict(list)
    for _from, _to in tickets:
        heapq.heappush(graph[_from], _to)

    stack, path = ["ICN"], deque()
    while stack:
        _from = stack[-1]
        if graph[_from]:
            stack.append(heapq.heappop(graph[_from]))
        else:
            path.appendleft(stack.pop())
    return list(path)
```

## Comment

풀어놓고 보니 힙, 스택, 큐를 전부 다 사용한 풀이가 됐다.

근데 풀면서 느낀거지만 틈만 나면 백트래킹이나 재귀로 문제를 해결하려고 하는 버릇을 고쳐야겠다. 그런 코드가 이해하기 쉬울지는 모르겠지만 수행시간이 너무 길어지고, 결국엔 콜스택을 쌓는 것이기 때문에 부담이 된다.

solution의 풀이는 0.03ms정도로 굉장히 빠르고 메모리도 10MB밖에 안쓰는데, 무지성 백트래킹으로 푸니까 더 시간도 많이 걸리고 메모리도 많이 썼다

처음에 풀었던 풀이 (330ms, 15MB)

```python
def f(tickets, visited, arr, answer, index):
    if len(arr) == len(tickets)+1:
        answer.append([_ for _ in arr])
        return

    for i, ticket in enumerate(tickets):
        if visited[i]:
            continue
        elif ticket[0] == tickets[index][1]:
            arr.append(ticket[1])
            visited[i] = True
            f(tickets, visited, arr, answer, i)
            arr.pop()
            visited[i] = False

def solution(tickets):
    answer = []
    for i, ticket in enumerate(tickets):
        if ticket[0] == 'ICN':
            arr, visited = [ticket[0]], [False for _ in tickets]
            arr.append(ticket[1])
            visited[i] = True
            f(tickets, visited, arr, answer, i)
    return sorted(answer)[0]
```
