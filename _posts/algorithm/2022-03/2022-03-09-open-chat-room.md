---
title: 2019 KAKAO BLIND RECRUITMENT - 오픈채팅방
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-09 20:32:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2019", "카카오"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/42888](https://programmers.co.kr/learn/courses/30/lessons/42888)

## Introduction

이름 변경을 제외하면 그냥 유저의 행동을 그대로 프린팅하기만 하면 되는 문제이다. 예전에 알고리즘 문제를 잘 못 풀던 시절에는 이 문제 푸는데 굉장히 끙끙댔던 것 같은데 왜 그랬는지 모르겠다.

## Note

- 이름 변경 여부를 체크 한 뒤 유저의 행동을 String으로 바꿔 정답리스트에 넣는다.

## Solution

```python
ACTION_DIC = {
    'Enter': '들어왔습니다.',
    'Leave': '나갔습니다.'
}

def solution(record):
    answer, actions, name_dic = [], [], {}
    for r in record:
        msg = r.split()
        action, uid = msg[0], msg[1]
        name = msg[2] if len(msg) == 3 else None

        if name is not None:
            name_dic[uid] = name
        if action != 'Change':
            actions.append([uid, action])

    for uid, action in actions:
        answer.append(f'{name_dic[uid]}님이 {ACTION_DIC[action]}')

    return answer
```

### 1. 필요한 변수 정의

action에 해당하는 문구를 바로 출력할 수 있도록 ACTION_DIC을 준비하고, uid를 key로, 이름을 value로 갖는 name_dic을 준비한다.

actions는 이름 변경이 아닌 경우의 모든 action을 다시 담아두기 위해 준비한다.

```python
ACTION_DIC = { # 1
    'Enter': '들어왔습니다.',
    'Leave': '나갔습니다.'
}

def solution(record):
    answer, actions, name_dic = [], [], {} # 1
    return answer
```

### 2. 이름 변경 처리

record를 순회하면서 action, uid, name을 추출한다. Leave의 경우는 name 값이 없으므로 None을 넣어준다.

```python
ACTION_DIC = {
    'Enter': '들어왔습니다.',
    'Leave': '나갔습니다.'
}

def solution(record):
    answer, actions, name_dic = [], [], {}
    for r in record:
        msg = r.split() # 2-1
        action, uid = msg[0], msg[1] # 2-1
        name = msg[2] if len(msg) == 3 else None # 2-1
    return answer
```

이름이 없는 경우를 제외하고는 name_dic에 uid별로 이름을 넣어준다. 이름 변경 action의 경우도 여기서 처리된다.

```python
ACTION_DIC = {
    'Enter': '들어왔습니다.',
    'Leave': '나갔습니다.'
}

def solution(record):
    answer, actions, name_dic = [], [], {}
    for r in record:
        msg = r.split()
        action, uid = msg[0], msg[1]
        name = msg[2] if len(msg) == 3 else None

        if name is not None: # 2-2
            name_dic[uid] = name
    return answer
```

이름 변경 action 이외의 경우에는 actions에 차곡차곡 쌓아준다.

```python
ACTION_DIC = {
    'Enter': '들어왔습니다.',
    'Leave': '나갔습니다.'
}

def solution(record):
    answer, actions, name_dic = [], [], {}
    for r in record:
        msg = r.split()
        action, uid = msg[0], msg[1]
        name = msg[2] if len(msg) == 3 else None

        if name is not None:
            name_dic[uid] = name
        if action != 'Change': # 2-3
            actions.append([uid, action])
    return answer
```

### 3. 메시지로 변환

아까 준비해뒀던 actions를 순회하며 메시지로 변환해 정답리스트에 넣어준다.

```python
ACTION_DIC = {
    'Enter': '들어왔습니다.',
    'Leave': '나갔습니다.'
}

def solution(record):
    answer, actions, name_dic = [], [], {}
    for r in record:
        msg = r.split()
        action, uid = msg[0], msg[1]
        name = msg[2] if len(msg) == 3 else None

        if name is not None:
            name_dic[uid] = name
        if action != 'Change':
            actions.append([uid, action])

    for uid, action in actions:
        answer.append(f'{name_dic[uid]}님이 {ACTION_DIC[action]}') # 3-1

    return answer
```

## Comment

블라인드 채용 문제 답지 않은 쉬운 문제였다. O(n)으로 풀어보려고 했지만 실패. 도중의 이름 변경 액션 때문에 가능한지 어떤지도 잘 모르겠다.
