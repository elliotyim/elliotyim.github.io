---
title: 2018 KAKAO BLIND RECRUITMENT [3차] 방금그곡
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-27 23:17:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags:
  ["알고리즘", "프로그래머스", "2018", "카카오", "블라인드", "시뮬레이션", "힙"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/17683](https://programmers.co.kr/learn/courses/30/lessons/17683)

## Introduction

나는 이미 짜둔 코드를 여러 번 다시 보는 것을 좋아한다. 시간이 없어 난장판으로 짠 코드를 리팩토링해서 다듬고 나만의 예술 작품으로 만들어 계속해서 감상하는 것을 좋아한다.

더 간결하고 이해하기 쉬운 문장(코드)을 작성할 때 매우 흐뭇하고 이게 내가 코딩을 하는 가장 원초적인 원동력이 되는 것 같다. 계속해서 글을 고쳐쓰는 것과 같은 즐거움. 나는 그래서 코딩을 멈출 수가 없다.

이번 문제도 풀어놓고 한참을 들여다 본 것 같다. 단순한 구현 문제이기 때문에 어려울 건 없지만, 그저 써놓은 문장을 여러번 고치면서 즐거웠다.

## Note

- `"ADEACC#BA#"`과 같이 연결된 string을 순회할 때 `C# -> c`로 변환하는 등의 방법으로 문자를 한 인덱스로 셀 수 있도록 해야함.
  - `"ADEACC#BA#"` -> `"ADEACcBa"`

## Solution

```python
import heapq

def replace_sharps(melody):
    result = []
    for mel in melody:
        if mel == '#':
            result[-1] = result[-1].lower()
        else:
            result.append(mel)
    return ''.join(result)

def play(melody, duration):
    n = len(melody)
    quotient, remainder = duration // n, int(duration % n)
    return melody * quotient + melody[:remainder]

def convert_to_minute(time):
    h, m = time.split(":")
    return 60 * int(h) + int(m)

def solution(m, musicinfos):
    answer, mel = [], replace_sharps(m)

    for i, info in enumerate(musicinfos):
        start, end, name, melody = info.split(',')
        start, end = convert_to_minute(start), convert_to_minute(end)
        duration = end - start

        melody = replace_sharps(melody)
        played_melody = play(melody, duration)
        if mel in played_melody:
            heapq.heappush(answer, [-len(played_melody), i, name])

    return answer[0][2] if answer else "(None)"
```

1. musicinfo에서 하나씩 시작시간과 끝나는 시간, 곡명, 멜로디 등을 추출한다. 이 때 멜로디에서 샵들은 전부 소문자로 변환하여 사용한다.
2. 연주시간 만큼 멜로디를 길게 늘어뜨리고 그 멜로디 안에 네오가 들었던 멜로디가 있는지 찾고 있으면 정답 힙에 정렬 순서대로 맞춰서 넣는다.
3. 정렬되어 있는 힙의 가장 맨 위의 멜로디의 곡명을 반환한다.
