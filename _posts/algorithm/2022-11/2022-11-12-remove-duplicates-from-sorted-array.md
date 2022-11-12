---
title: Leetcode - 26. Remove Duplicates from Sorted Array
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-11-12 22:50:00 +0900
categories: ["알고리즘"]
tags: []
---

Link: [https://leetcode.com/problems/remove-duplicates-from-sorted-array/](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Introduction

입력으로 받은 배열 안의 중복된 데이터를 in-place로 제거하는 문제이다.

중복된 숫자들을 제거하고 앞에서부터 숫자를 채우면 되는데, 뒤쪽에는 어떤 숫자가 위치하든 상관없고, 중복없는 숫자의 갯수 k만 반환하면 된다.


## Solution

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) < 2:
            return 1

        cursor = 1
        for i in range(1, len(nums)):
            if nums[i-1] != nums[i]:
                nums[cursor] = nums[i]
                cursor += 1

        return cursor
```

### I. 문제 정의

O(1)의 extra memory로 풀어야 하는게 문제의 조건이다. 보통 이런 경우는 포인터를 두고 문제를 풀면 된다.

### II. 포인터를 두고 숫자 채우기

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) < 2:
            return 1

        cursor = 1
        for i in range(1, len(nums)):
            if nums[i-1] != nums[i]:
                nums[cursor] = nums[i]
                cursor += 1

        return cursor
```

딱히 길게 적을 것이 없을 것 같다. length가 1 이하인 리스트를 받으면 1을 반환하면 되고,

length 2 이상 부터는 cursor를 index=1에 두고 거기서 부터 시작해서 1개씩 순회하면서 이전 숫자와 다르다면 cursor가 있는 자리에 하나씩 차곡차곡 뒤쪽의 숫자를 옮겨서 채워 넣으면 된다.

## Comment

오늘의 Daily Problem을 풀어보려고 하니까 난이도가 hard라 금요일의 문제를 가져왔다.

반년 정도 알고리즘 문제 정리를 쉬었더니 뭔가 어색하기도 하고 머리가 굳은 느낌이 없잖아 있어서 재활 느낌으로 쉬운 문제를 골랐다.
