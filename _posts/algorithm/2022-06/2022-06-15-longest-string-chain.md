---
title: Leetcode - Logest String Chain
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-06-15 19:41:00 +0900
categories: ["알고리즘"]
tags: ["DP"]
---

Link: [https://leetcode.com/problems/longest-string-chain/](https://leetcode.com/problems/longest-string-chain/)

## Introduction

LeetCode 오늘의 문제로 나오길래 풀어보니 이전에 이미 풀어본적 있는 문제였는데, 그 당시 정답을 보고나서도 이해를 잘 못하고 있었어서 그런지 제대로 기억이 안났다.

길이가 더 작은 단어에서 큰 단어로 찾아가면서 이전 값을 사용하는 DP 문제이다.

## Solution

### I. 문제 정의

words의 길이가 n, words에 포함된 단어의 길이가 m이라고 하면 n \* m만으로도 이미 16,000이므로, O((n \* m)^2)으로 풀면 시간초과가 나서 이보다는 더 적은 수행시간 내에 해결해야 한다.

### II. DP

word chain 값을 갱신해나가야 하기 위해 각 word의 word chain 값을 저장할 Dictionary와 answer 변수를 준비한다.

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp, answer = dict(), 0 # 2-1
```

가장 작은 단어부터 시작해서 word chain 값을 적어나갈 것이므로, words를 단어의 길이의 오름차순으로 정렬해준다.

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp, answer = dict(), 0
        sorted_words = sorted(words, key=len) # 2-2
```

우선 각 글자는 그 자체만으로도 word chain 값 1을 가지므로 dp에 이 값을 먼저 저장해 준다.

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp, answer = dict(), 0
        sorted_words = sorted(words, key=len)
        for word in sorted_words:
            dp[word] = 1 # 2-3
```

현재 단어의 word chain을 아는 방법은 현재 단어보다 길이가 하나 작은 단어가 dp에 있는지 체크하고, 있으면 그 단어의 word chain 값 + 1 해주면 된다.

```
예시) ["a", "b", "ba", "bc", "bca", "bda", "bdca"]의 경우

{'a': 1, 'b': 1, 'ba': 2, 'bc': 2}인 상태에서,

bca의 글자를 하나씩 뺀 [ca, bc, ba]중에, 해당되는 글자인 ba와 bc의 값 + 1이 bca의 word chain 값이 된다.
```

우선 글자를 잘라준다.

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp, answer = dict(), 0
        sorted_words = sorted(words, key=len)
        for word in sorted_words:
            dp[word] = 1
            for i in range(len(word)):
                sliced_word = word[:i] + word[i+1:] # 2-4
```

자른 글자가 dp에 없다면 (예시에서는 글자 `ca`) 넘어간다.

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp, answer = dict(), 0
        sorted_words = sorted(words, key=len)
        for word in sorted_words:
            dp[word] = 1
            for i in range(len(word)):
                sliced_word = word[:i] + word[i+1:]
                if sliced_word not in dp: # 2-5
                    continue
```

무작정 이전 값 + 1의 값으로 갱신하면 안된다.

words =
['a', 'ca', 'bda', 'dca', 'bdca']의 경우, bdca의 자른 글자의 word chain 값이 bda: 1, dca: 3으로 다르다.

따라서, 현재 dp[word]의 값과 찾은 이전 단어의 값 중 더 큰 값을 저장한다.

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp, answer = dict(), 0
        sorted_words = sorted(words, key=len)
        for word in sorted_words:
            dp[word] = 1
            for i in range(len(word)):
                sliced_word = word[:i] + word[i+1:]
                if sliced_word not in dp:
                    continue
                dp[word] = max(dp[word], dp[sliced_word]+1) # 2-6
```

각 단어의 체크가 끝날 때마다 최대값을 갱신해 주고 마지막에 이 값을 반환한다.

```python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp, answer = dict(), 0
        sorted_words = sorted(words, key=len)
        for word in sorted_words:
            dp[word] = 1
            for i in range(len(word)):
                sliced_word = word[:i] + word[i+1:]
                if sliced_word not in dp:
                    continue
                dp[word] = max(dp[word], dp[sliced_word]+1)
            answer = max(answer, dp[word]) # 2-7
        return answer
```

## Comment

DP 문제는 정말 풀어도 풀어도 적응이 안 된다...뭔가 알 듯 말 듯 하면서도 어떤 방식으로 적용해야 하는지 항상 고민이 된다.
