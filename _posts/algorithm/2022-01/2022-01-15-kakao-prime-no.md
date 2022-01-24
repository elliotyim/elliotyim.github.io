---
title: 2022 KAKAO BLIND RECRUITMENT k진수에서 소수 개수 구하기

author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-15 12:15:00 +0900
categories: [알고리즘, 프로그래머스]
tags: ["알고리즘", "프로그래머스", "2022", "카카오", "블라인드", "투포인터"]
---

## Note

- python에서 % 연산자를 사용해 나머지를 구할 때는 소수점이 붙는 경우가 있다.
  - (ex) 1 -> 1.0, 0 -> 0.0)

## 풀이

1. 주어진 수를 n진수로 변환한다.
2. 커서를 두 개 두고 하나를 전진시키면서 커서 사이의 숫자가 소수이면서 0이 포함되어 있지 않다면 정답에 포함시키고 두 커서의 위치를 다음 숫자로 옮긴다.
3. 110011 같이 11을 정답에 포함시킨 후 left, right 커서들의 위치가 index 3인 0에(110**0**11) 위치하는 경우가 있다. 이 때는 커서들을 index 4인 1로(1100**1**1) 옮겨준다.

```python
import math
from collections import deque

def convert_no(n, k):
    nums, quotient = deque(), n
    while quotient > 0:
        quotient, remainder = quotient // k, int(quotient % k)
        nums.appendleft(str(remainder))
    return ''.join(nums)

def is_prime_no(no):
    if no < 2:
        return False

    offset = math.floor(math.sqrt(no)) + 1
    for i in range(2, offset):
        if no % i == 0:
            return False
    return True

def contain_zero(no):
    return True if '0' in str(no) else False

def solution(n, k):
    answer, left, right = [], 0, 0
    no = convert_no(n, k)
    while right < len(no):
        if no[right] == '0':
            if left == right:
                left = right = right+1
            else:
                cur_no = int(no[left:right])
                if is_prime_no(cur_no) and not contain_zero(cur_no):
                    answer.append(cur_no)
                left = right+1
        elif right == len(no)-1:
                cur_no = int(no[left:])
                if is_prime_no(cur_no) and not contain_zero(cur_no):
                    answer.append(cur_no)
        right += 1
    return len(answer)
```

## 잡담

투 포인터를 사용해서 풀긴 했는데 뭔가 내가 만들었던 테스트 케이스 중 틀린 게 있었는데 답안 제출할 때 맞다고 넘어가서 살짝 불안한 감이 있다.

코테에 시간제한이 있어야 변별력이 있겠지만 없었으면 좋겠다. 난 같은 코드를 진득하니 보면서 계속 깔끔하게 다듬어나가는 걸 좋아하는데 시간제한이 있으면 너무 허겁지겁 푸느라 가독성이 개판이 되어버린다.

아직 실력이 부족해서 한 번에 깔끔한 코드를 못 짜서 그런거겠지?
