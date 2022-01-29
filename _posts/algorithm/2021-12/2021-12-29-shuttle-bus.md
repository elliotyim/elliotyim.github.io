---
title: 2018 KAKAO BLIND RECRUITMENT [1차] 셔틀버스
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2021-12-29 21:25:00 +0900
categories: [알고리즘, 프로그래머스]
tags: ["알고리즘", "프로그래머스", "2018", "카카오", "셔틀버스", "큐"]
---

**Updated at 2022-01-29**

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/17678](https://programmers.co.kr/learn/courses/30/lessons/17678)

## Introduction

작년(2021년)에 포스팅 한 문제이긴 하지만 다시 봤을 때 한 눈에 안 들어와서 다시 풀어보기로 했다. 한 달 정도 지났지만 풀어나가는 스타일이 조금 바뀌기도 했고, 모든 코드에 스토리를 넣는 방식으로 풀어나가는 거라 이번에는 좀 더 시간이 지나고 봤을 때도 납득이 갈만한 풀이를 작성했다.

기본 틀은 똑같다. 버스가 지나가고 사람들은 줄을 서고, 막차 자리가 한 자리 남았을 때 콘을 새치기 시킨다. 이 스토리를 코드로 풀어나간다.

## Note

- 큐를 사용해서 풀 수 있는 문제이다.
- timetable이 정렬되어 있지 않은 점에 주의
- 시간 비교의 편의를 위해 모든 시간은 분 단위로 바꾸어 비교한다.

## Solution

1, n개의 버스가 정류장에 도착하고 떠나는 것을 구현한다. 이 버스들은 9시(540분)에 처음으로 출발하여 t분 간격으로 정류장에 도착하고 떠난다. 마지막엔 막차 시간을 반환한다.

```python
from collections import deque

def add_leading_zero(no):
    return str(no) if no > 9 else f'0{no}'

def format_time(time):
    h, m = time // 60, time % 60
    return f'{add_leading_zero(h)}:{add_leading_zero(m)}'

def solution(n, t, m, timetable):
    dept_time = 9 * 60

    while n > 0: # 1
        dept_time += t
        n -= 1

    dept_time -= t
    return format_time(dept_time)
```

2, 이제 이 버스를 타기 위해 줄을 서고 있는 크루들이 있는 정류장을 구현해보자. 버스가 출발하는 시간 전까지 정류장에 도착한 크루들은 모두 정류장에 줄 세운다.(deque에 집어넣는다.)

```python
from collections import deque

def get_minute(time):
    h, m = time.split(':')
    return 60 * int(h) + int(m)

def add_leading_zero(no):
    return str(no) if no > 9 else f'0{no}'

def format_time(time):
    h, m = time // 60, time % 60
    return f'{add_leading_zero(h)}:{add_leading_zero(m)}'

def solution(n, t, m, timetable):
    i, dept_time, queue = 0, 9 * 60, deque()
    timetable.sort()

    while n > 0:
        while i < len(timetable): # 2
            queued_time = get_minute(timetable[i])
            if queued_time > dept_time:
                break
            queue.append(queued_time)
            i += 1

        dept_time += t
        n -= 1

    dept_time -= t
    return format_time(dept_time)
```

3, 줄은 세웠으니, 버스에 태우는 것을 구현한다. 좌석이 더 이상 없거나 정류장 줄에 더 이상 사람이 없을 때까지 버스에 태운다.

```python
from collections import deque

def get_minute(time):
    h, m = time.split(':')
    return 60 * int(h) + int(m)

def add_leading_zero(no):
    return str(no) if no > 9 else f'0{no}'

def format_time(time):
    h, m = time // 60, time % 60
    return f'{add_leading_zero(h)}:{add_leading_zero(m)}'

def solution(n, t, m, timetable):
    i, dept_time, queue = 0, 9 * 60, deque()
    timetable.sort()

    while n > 0:
        while i < len(timetable):
            queued_time = get_minute(timetable[i])
            if queued_time > dept_time:
                break
            queue.append(queued_time)
            i += 1

        seats = m
        while seats > 0 and queue: # 3
            queued_time = queue.popleft()
            seats -= 1

        dept_time += t
        n -= 1

    dept_time -= t
    return format_time(dept_time)
```

4, 문제는 콘 보다 더 부지런한 크루가 마지막 좌석에 탔을 때 벌어진다.

여기서 콘은 고민한다. 자신이 버스에 가장 마지막으로 탈 수 있는 타이밍은 언제일까? 바로 버스가 막차(n == 1)이고, 자리가 딱 한 자리 남았을 때 그 마지막 좌석에 앉으려는 사람보다 1분 더 일찍 줄을 서면 된다.

```python
from collections import deque

def get_minute(time):
    h, m = time.split(':')
    return 60 * int(h) + int(m)

def add_leading_zero(no):
    return str(no) if no > 9 else f'0{no}'

def format_time(time):
    h, m = time // 60, time % 60
    return f'{add_leading_zero(h)}:{add_leading_zero(m)}'

def solution(n, t, m, timetable):
    i, dept_time, queue = 0, 9 * 60, deque()
    timetable.sort()

    while n > 0:
        while i < len(timetable):
            queued_time = get_minute(timetable[i])
            if queued_time > dept_time:
                break
            queue.append(queued_time)
            i += 1

        seats = m
        while seats > 0 and queue:
            queued_time = queue.popleft()
            if n == 1 and seats == 1: # 4
                return format_time(queued_time - 1)
            seats -= 1

        dept_time += t
        n -= 1

    dept_time -= t
    return format_time(dept_time)
```

## Legacy

지난 풀이. (코드는 더 간결한듯 보이지만 뭔가 시간이 지나고 다시 봤을 때 기억이 잘 안날정도로 와닿지가 않았다.)

1. 크루들을 정류장 도착 시간 순으로 정렬하여 큐에 넣는다.
2. 버스의 대수만큼 현재 시간을 t씩 계속 증가시키면서 정류장에 더 올 사람이 있는지 체크하고 없는데 자리가 남으면 콘을 태우고 버스에 탄 시간을 반환한다.
3. 큐의 맨 앞에 있는 사람이 정류장에 도착하는 시간이 버스보다 느리면 해당 버스를 그냥 보낸다.
4. 큐에 있는 사람들을 버스에 계속 태우다가 마지막 한 자리가 남았다면 콘을 태우기 위해 현재 정류장의 가장 앞에 있는 사람보다 1분 빠르게 콘이 도착하게 하도록 하고 그 시간을 반환한다.
5. 마지막 버스가 떠나기 전에 정류장에 사람이 기다리고 있다면 그 사람들보다 1분 빠르게 도착하게 한 뒤 그 시간을 반환하고, 마지막 버스가 도착했을 때 정류장에 사람이 없다면 마지막 버스가 떠나는 시간을 반환한다.

```python
from collections import deque

def get_second(time):
    t = time.split(':')
    return 60 * int(t[0]) + int(t[1])

def add_leading_zero(no):
    return f'{no}' if no >= 10 else f'0{no}'

def format_time(time):
    h = time // 60
    m = time % 60
    return f'{add_leading_zero(h)}:{add_leading_zero(m)}'

def solution(n, t, m, timetable):
    queue = deque(sorted(timetable))
    now = 9 * 60 - t

    for i in range(n):
        now += t
        seats = m
        left_buses = n-i
        while seats > 0:
            if queue:
                current = get_second(queue.popleft())
            else:
                return format_time(now)

            if now < current:
                queue.appendleft(format_time(current))
                break

            left_seats = (left_buses-1) * m + seats
            if left_seats == 1:
                return format_time(current-1)

            seats -= 1

    if queue:
        now = min(now, get_second(queue[0])-1)

    return format_time(now)
```

## Comment

![BoredShibaInu](/assets/img/meme/BoredShibaInu.jpg){: width="50%" height="50%"}

어려운 문제는 아닌데 문제가 길어서 풀기 전에 계속 멍 때렸다. 이러니까 시간 제한 있는 코딩테스트에서 맨날 시간 부족해서 못 풀지

Level 3 치고는 쉽다. 2018년도 문제인데, 확실히 코딩 테스트는 해를 거듭할 수록 어려워지는 듯
