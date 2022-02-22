---
title: (프로그래머스) 2017 팁스타운 - 짝지어 제거하기
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-22 14:25:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2017", "팁스타운"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/12973](https://programmers.co.kr/learn/courses/30/lessons/12973)

## Introduction

스택은 저장하고 있는 것 위에 다른 것을 얹어서 저장하는데, 이러한 특성 덕분에 현재 다루고 있는 것을 잠시 저장해두고 다른 것으로 전환하는 식의 활용이 가능한 독특한 형태의 자료구조이다.

마치 작업하고 있는 일이 있는데 끊임 없이 일을 더 갖고 오는 동료로 인해 고통받는 현대인들의 기구한 일상을 표현하고 있는것 같다. (심지어 별로 급해보이지도 않는데 그거 먼저 해달라고 한다.)

![problem](/assets/img/algorithm/programmers/practice/remove-pairs/problem.jpg){: width="50%" height="50%"}

이 문제는 스택이 가지는 작업의 전환의 특성을 이용한 정말 멋진 문제이다.

## Note

- 스택이 가지는 특성을 잘 이해할 것

## Solution

```python
def solution(s):
    stack = []
    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else:
            stack.append(ch)
    return 0 if stack else 1
```

### 1. 빠요엔~ 빠요엔~ 빠요엔~

스택의 특징은 위에서도 언급했듯이, 현재 작업중인 상태를 저장하고 다른 작업으로 넘어간다는 것이다.

이 특성을 활용해 지울 수 없는 문자들은 스택에 넣고, 지울 수 있는 타이밍이 오면 스택에서 빼는 것으로 삭제 처리를 한다.

예를 들면, s = baaabaa의 경우, 아래와 같은 과정으로 문자들이 왼쪽으로 밀리며 사라져간다. 색칠된 부분은 스택에 들어가 있다는 것을 의미한다.

![puyo-puyo](/assets/img/algorithm/programmers/practice/remove-pairs/remove-pairs001.jpg)
_1. b를 스택에 추가_

![puyo-puyo2](/assets/img/algorithm/programmers/practice/remove-pairs/remove-pairs002.jpg)
_2. a를 스택에 추가하고 다음 a를 체크할 때 제거_

![puyo-puyo3](/assets/img/algorithm/programmers/practice/remove-pairs/remove-pairs003.jpg)
_3. 다음 b를 체크하면서 스택의 b도 제거_

![puyo-puyo4](/assets/img/algorithm/programmers/practice/remove-pairs/remove-pairs004.jpg)
_4. a를 스택에 추가하고 다음 a를 체크할 때 제거_

이 과정을 코드로 옮긴다. 특별한 일이 없으면 스택에 넣고, 스택에 문자가 있으면서 그 문자와 현재 문자가 같으면 스택에서 하나씩 빼는 방식이다.

```python
def solution(s):
    stack = []
    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else:
            stack.append(ch)
```

마지막으로 스택에 무언가가 남아있다면 지워지지 않은 문자가 남아있다는 것이므로 0을 반환하고 아니면 1을 반환한다.

```python
def solution(s):
    stack = []
    for ch in s:
        if stack and stack[-1] == ch:
            stack.pop()
        else:
            stack.append(ch)
    return 0 if stack else 1
```

## Comment

자료구조는 모두 간단한 구조를 가지고 있고 누구나 쉽게 사용할 수 있다.

누구나 스택에 대해 설명해 달라고 하면 LIFO FIFO 이런 단어들을 말할수는 있다. 책에서 보고 배웠으니까.

근데 그것들의 특징을 제대로 파악하고 사용하지 않는다면, 실제 퍼포먼스의 절반도 못 쓰게 되는 것이 아닐까 하는 생각을 해본다.
