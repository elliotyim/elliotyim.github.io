---
title: 2022 KAKAO BLIND RECRUITMENT 주차 요금 계산

author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-17 12:35:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2022", "카카오", "블라인드", "시뮬레이션"]
---

## Note

- 문제에 적혀 있는대로 풀면 된다.

## 풀이

1. 입차시간과 출차시간을 계산해서 차량별로 누적시간을 계산해둔다.
2. 입차만 하고 출차를 하지 않은 차량이 남아있으면 이 차량을 23시 59분에 출차한 것으로 요금을 계산한다.
3. 누적시간이 기본요금보다 많으면 기본요금 + 누적시간에 따른 단위요금을 계산하고, 적으면 기본요금만 청구하여 번호순으로 리스트를 만들어 반환한다.

```python
import math
from collections import defaultdict

def to_minute_time(time):
    h, m = time.split(':')
    return int(h) * 60 + int(m)

def solution(fees, records):
    answer, park_dic, acc_time_dic = [], dict(), defaultdict(int)

    for record in records:
        time, no, action = record.split()
        time = to_minute_time(time)
        if action == 'IN':
            park_dic[no] = time
        elif action == 'OUT':
            acc_time_dic[no] += time - park_dic.pop(no)

    close_time = 23 * 60 + 59
    for no, time in park_dic.items():
        acc_time_dic[no] += close_time - time

    for no, time in acc_time_dic.items():
        if time >= fees[0]:
            fee = fees[1] + math.ceil((time - fees[0]) / fees[2]) * fees[3]
            answer.append((no, fee))
        else:
            answer.append((no, fees[1]))

    return [time for (no, time) in sorted(answer)]
```

## 잡담

알고리즘 코테를 준비하는 사람 중에 이 문제를 못 푸는 사람은 거의 없을 것 같다. 어려운 문제도 아니고 문제에 풀이방법이 아예 써 있으니까

다만, 이번 문제의 풀이는 내 성향이자 단점을 잘 드러내는 것 같다. 난 수행시간 보다 코드의 가독성이나 짧은 정도를 더 선호한다.

23-30 라인의 풀이가 그런데, 저 과정에서 acc_time을 한 번, answer를 두 번(sorted()에서 한 번, 다시 time만 뽑아내기 위해 한 번) 순회한다.

처음엔 힙을 사용해서 리스트에 넣을 때 정렬하면서 넣고 그걸 순서대로 pop하면서 time을 뽑아낸 result를 만들까 생각했는데 그건 코드가 좀 더 투박해질 것 같고, 입출차 한 자동차의 대수가 많지 않다면 수행시간이 크게 차이가 나지 않을 것 같아 저렇게 구현했다.

수행시간과 가독성...정말 어렵다.
