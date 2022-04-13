---
title: Leetcode - Longest Substring Without Repeating Characters
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-04-13 14:50:00 +0900
categories: ["알고리즘", "leetcode"]
tags: ["알고리즘", "leetcode", "슬라이딩 윈도우"]
---

## Link

- [https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Introduction

주어진 문자열 s에서 반복되는 문자가 없는 가장 긴 부분 문자열을 찾는 문제이다.

## Note

- Index Out of Range 에러에 주의

## Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        answer, ch_set = 0, set()
        left, right = 0, 0

        while right < len(s):
            if s[right] in ch_set:
                answer = max(answer, len(s[left:right]))
                while left < right and s[right] in ch_set:
                    ch_set.remove(s[left])
                    left += 1
            ch_set.add(s[right])
            right += 1

        return max(answer, len(s[left:right]))
```

### I. 필요한 변수 준비

정답을 담을 변수 answer와 중복된 문자를 체크하기 위한 ch_set 변수, 그리고 슬라이딩 윈도우 알고리즘을 사용해서 풀 것이기 떄문에 left, right 포인터 변수를 준비한다.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        answer, ch_set = 0, set()
        left, right = 0, 0
```

### II. Sliding Window

right를 0부터 s의 문자열 끝까지 전진시킨다.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        answer, ch_set = 0, set()
        left, right = 0, 0

        while right < len(s):
            right += 1 # 2-1
```

전진시키면서 오른쪽 포인터가 가리키는 문자를 ch_set에 넣는다.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        answer, ch_set = 0, set()
        left, right = 0, 0

        while right < len(s):
            ch_set.add(s[right]) # 2-2
            right += 1
```

right 포인터가 가리키는 문자열이 중복되는 경우 해당 문자 이전까지의 substring의 길이값을 최대값과 비교하여 갱신한다.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        answer, ch_set = 0, set()
        left, right = 0, 0

        while right < len(s):
            if s[right] in ch_set:
                answer = max(answer, len(s[left:right])) # 2-3
            ch_set.add(s[right])
            right += 1
```

갱신 후에는 left 포인터의 위치를 중복이 안되는 문자가 들어가는 지점까지 옮겨줘야 한다.

1. left는 당연히 right보다 크면 안된다.
2. right 포인터가 위치한 문자가 ch_set에 포함되면 안된다.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        answer, ch_set = 0, set()
        left, right = 0, 0

        while right < len(s):
            if s[right] in ch_set:
                answer = max(answer, len(s[left:right]))
                while left < right and s[right] in ch_set: # 2-4
                    ch_set.remove(s[left])
                    left += 1
            ch_set.add(s[right])
            right += 1
```

입력 예 "dvdf"와 같이 right 포인터가 문자열에 끝에 들어갈 때까지 `if s[right] in ch_set` 조건에 걸리지 않는 경우가 있다. (중복되는 문자를 끝까지 못 찾을 경우)

이 경우를 생각해서 마지막에 문자열의 길이를 최대값이랑 비교한 후 결과값을 반환해 준다.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        answer, ch_set = 0, set()
        left, right = 0, 0

        while right < len(s):
            if s[right] in ch_set:
                answer = max(answer, len(s[left:right]))
                while left < right and s[right] in ch_set:
                    ch_set.remove(s[left])
                    left += 1
            ch_set.add(s[right])
            right += 1

        return max(answer, len(s[left:right])) # 2-5
```

## Comment

문제를 보자마자 어떻게 해결해야 할지 바로 떠올랐지만 포인터가 어긋나는거 때문에 실수를 좀 많이 했다.

근데 역시 더 깔끔하게 푼 코드들 보면 뭔가 자괴감이 들기도...

```java
// by jeantimex

public int lengthOfLongestSubstring(String s) {
    int i = 0, j = 0, max = 0;
    Set<Character> set = new HashSet<>();

    while (j < s.length()) {
        if (!set.contains(s.charAt(j))) {
            set.add(s.charAt(j++));
            max = Math.max(max, set.size());
        } else {
            set.remove(s.charAt(i++));
        }
    }

    return max;
}
```
