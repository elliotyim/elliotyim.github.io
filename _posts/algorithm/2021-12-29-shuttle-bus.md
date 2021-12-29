---
title: 2018 Kakao Blind Recruitment 1차 프로그래머스 셔틀버스
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2021-12-29 21:25:00 +0900
categories: [알고리즘, 프로그래머스]
tags: [알고리즘, 프로그래머스, 셔틀버스, 큐]
---

## Note

- 큐를 사용해서 풀 수 있는 문제이다.
- 시간 비교의 편의를 위해 모든 시간은 초 단위로 바꾸어 비교한다.

## 해설

1. 크루들을 정류장 도착 시간 순으로 정렬하여 큐에 넣는다.
2. 버스의 대수만큼 현재 시간을 t씩 계속 증가시키면서 정류장에 더 올 사람이 있는지 체크하고 없는데 자리가 남으면 콘을 태우고 버스에 탄 시간을 반환한다.
3. 큐의 맨 앞에 있는 사람이 정류장에 도착하는 시간이 버스보다 느리면 해당 버스를 그냥 보낸다.
4. 큐에 있는 사람들을 버스에 계속 태우다가 마지막 한 자리가 남았다면 콘을 태우기 위해 현재 정류장의 가장 앞에 있는 사람보다 1분 빠르게 콘이 도착하게 하도록 하고 그 시간을 반환한다.
5. 마지막 버스가 떠나기 전에 정류장에 사람이 기다리고 있다면 그 사람들보다 1분 빠르게 도착하게 한 뒤 그 시간을 반환하고, 마지막 버스가 도착했을 때 정류장에 사람이 없다면 마지막 버스가 떠나는 시간을 반환한다.

```
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

            prev = current
            seats -= 1

    if queue:
        now = min(now, get_second(queue[0])-1)

    return format_time(now)
```

## 잡담

어려운 문제는 아닌데 문제가 길어서 풀기 전에 계속 멍 때렸다. 이러니까 시간 제한 있는 코딩테스트에서 맨날 시간 부족해서 못 풀지
![BoredShibaInu](/assets/img/meme/BoredShibaInu.jpg){: width="50%" height="50%"}

Level 3 치고는 쉽다. 2018년도 문제인데, 확실히 코딩 테스트는 해를 거듭할 수록 어려워지는 듯
