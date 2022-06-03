---
title: Backtracking Basic - Combinations
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-06-03 19:18:00 +0900
categories: ["프로그래밍", "기초"]
tags: ["백트래킹", "조합"]
---

# Combinations

조합을 만드는 코드가 매번 짤 때마다 헷갈려서 그냥 한 번 정리해두기로 했다.

조합할 대상 iterables와 n을 인자로 받은 get_combinations()에서 사용하는 helper 함수에 들어 갈 것은 아래와 같다.

1. iterables
   - 조합할 대상
2. n
   - 조합될 결과물의 길이
3. result
   - 조합된 결과물들이 들어있는 결과물 리스트
4. arr
   - 조합된 결과물을 잠시 담고 있을 그릇
5. used
   - iterables 중에 한 번 사용된 것은 다시 사용하지 않기 위해 체크해두는 리스트. 중복 허용이면 필요 없음

```python
def get_combinations(iterables, n):
    result, arr, used = [], [], [False for _ in iterables]
    helper(iterables, n, result, arr, used)
    return result
```

helper()의 내용.

- base_condition arr.length == n일 때는 arr의 값을 result에 포함시킨다.
- iterables를 순회하며 사용하지 않은 item을 arr에 넣고 재귀호출한다.

```python
def helper(iterables, n, result, arr, used):
    if len(arr) == n:
        result.append([_ for _ in arr])
        return

    for i, item in enumerate(iterables):
        if not used[i]:
            used[i] = True
            arr.append(item)
            helper(iterables, n, result, arr, used)
            used[i] = False
            arr.pop()
```
