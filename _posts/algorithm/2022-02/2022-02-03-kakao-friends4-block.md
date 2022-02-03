---
title: 2018 KAKAO BLIND RECRUITMENT [1차] 프렌즈4블록
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-02-03 18:45:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2018", "카카오", "시뮬레이션"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/17679](https://programmers.co.kr/learn/courses/30/lessons/17679)

## Introduction

프로그래머스 문제에서는 이따금 게임 내용의 일부분을 구현하는 문제가 나온다.

이번 문제가 딱 그런데, 정말 재미있게 풀었던 것 같다. 게임보다 구현해야 할 요구조건이 명확한 것도 없고 이해하기도 쉬운 것도 없다고 생각한다. 머릿속에서 더 쉽게 그려지니까.

## Note

- 필요한 행동(함수)을 미리 정의하고 이를 구현해 나가는 방식으로 진행

## Solution

```python

from collections import deque

BLOCK_TABLE = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
BLOCK_DICT = {block:str(i) for i, block in enumerate(BLOCK_TABLE)}

def valid_block(to_be_checked, given_block):
    if to_be_checked == '-1' or given_block == '-1':
        return False
    elif to_be_checked == given_block:
        return True
    else:
        return to_be_checked == BLOCK_DICT[given_block]

def mark(board):
    marked = False
    for y in range(len(board)):
        for x in range(len(board[0])):
            block = board[y][x]
            if block == '-1':
                continue

            pos = [x-1, y, x, y-1, x-1, y-1]
            if any(p < 0 for p in pos) == True:
                continue

            if not valid_block(board[y][x-1], block) or \
                not valid_block(board[y-1][x], block) or \
                not valid_block(board[y-1][x-1], block):
                continue

            board[y][x-1] = BLOCK_DICT[block]
            board[y-1][x] = BLOCK_DICT[block]
            board[y-1][x-1] = BLOCK_DICT[block]
            board[y][x] = BLOCK_DICT[block]
            marked = True

    return marked

def delete(board):
    deleted_blocks = 0
    for y in range(len(board)):
        for x in range(len(board[0])):
            if not board[y][x].isalpha() and int(board[y][x]) > -1:
                board[y][x] = '-1'
                deleted_blocks += 1
    return deleted_blocks

def pulldown(board):
    for x in range(len(board[0])):
        blanks = deque()
        for y in range(len(board)-1, -1, -1):
            if board[y][x] == '-1':
                blanks.append((x, y))
            elif blanks:
                blank_x, blank_y = blanks.popleft()
                board[blank_y][blank_x] = board[y][x]
                board[y][x] = '-1'
                blanks.append((x,y))

def solution(m, n, board):
    answer = 0
    copied_board = [list(b) for b in board]

    while True:
        any_block_marked = mark(copied_board)
        if not any_block_marked:
            break
        deleted_blocks = delete(copied_board)
        answer += deleted_blocks
        pulldown(copied_board)
    return answer
```

**풀이**

### 1. 머릿속에서 시뮬레이션

우선 문제와 그림을 보고 생각한다. 컴퓨터가 아닌 내가 이 문제를 직접 해결한다면 어떤 행동들이 필요할까?

아마도 눈으로 퍼즐을 쭉 훑고, 4개씩 모여져 있는 곳에 마킹을 하고 지우지 않을까?

```python
def mark(board):
    pass

def delete(board):
    pass

def solution(m, n, board):
    answer = 0
    return answer
```

원래는 마킹과 지우는 것을 동시에 해야 하겠지만, 추후에 리팩토링을 하더라도 일단 단계별로 생각을 하자

지운 다음에 퍼즐은 밑으로 떨어지겠지?

```python
def mark(board):
    pass

def delete(board):
    pass

def pulldown(board):
    pass

def solution(m, n, board):
    answer = 0
    return answer
```

### 2. 주어진 함수들을 가지고 구현

자 그럼 이 함수들이 구현이 되었다고 가정하고 주어진 함수를 사용해 상황을 만들어 보자.

우선 파라미터로 받은 board를 변형시키는 것도 그렇고, string 형태면 중간에 있는 블록을 갈아끼거나 지우는게 번거로우므로 list 형태로 바꿔서 카피해준다.

```python
def mark(board):
    pass

def delete(board):
    pass

def pulldown(board):
    pass

def solution(m, n, board):
    answer = 0
    copied_board = [list(b) for b in board]
    return answer
```

함수를 사용해서 구현 해보자. 아마 이런느낌이지 않을까?

보드에 마킹하고, 지우고, 지운 블록 갯수를 정답에 더하고, 떠있는 블록들을 끌어내린다.

```python
def mark(board):
    pass

def delete(board):
    pass

def pulldown(board):
    pass

def solution(m, n, board):
    answer = 0
    copied_board = [list(b) for b in board]

    any_block_marked = mark(copied_board)
    deleted_blocks = delete(copied_board)
    answer += deleted_blocks
    pulldown(copied_board)

    return answer
```

이걸 더 이상 마킹할 수 있는 블록이 없을 때까지 반복한다.

```python
def mark(board):
    pass

def delete(board):
    pass

def pulldown(board):
    pass

def solution(m, n, board):
    answer = 0
    copied_board = [list(b) for b in board]

    while True:
        any_block_marked = mark(copied_board)
        if not any_block_marked:
            break
        deleted_blocks = delete(copied_board)
        answer += deleted_blocks
        pulldown(copied_board)

    return answer
```

### 3-1. 주어진 함수들을 구현 - mark()

두 가지를 고려해야 한다. 어떻게 마킹해 나갈 것인가와, 마킹한 것을 어떤 방식으로 남겨놓을 것인가이다.

어떻게 마킹할지 보니, 지난 포스팅의 [가장 큰 정사각형 찾기](https://elliotyim.github.io/posts/biggest-square/) 문제와 유사하다.

이를 참고해서, 한 블록을 기준으로 그 블록의 왼쪽(x-1, y), 위쪽(x, y-1), 왼쪽 위(x-1, y-1)의 3방향을 체크하는 방식으로 마킹을 해보자.

그리고 우선은, 지워진 블록은 `"-1"`로 표기되어 있다고 가정하자.

```python
def mark(board):
    for y in range(len(board)):
        for x in range(len(board[0])):
            block = board[y][x]
            # 기준 블록이 이미 지워진 블록이면 넘어가고
            if block == '-1':
                continue

            # 3방향 좌표 중 하나라도 인덱스 0보다 작으면 넘어가고
            pos = [x-1, y, x, y-1, x-1, y-1]
            if any(p < 0 for p in pos) == True:
                continue

            # 아래의 3방향을 체크한다.
            board[y][x-1]
            board[y-1][x]
            board[y-1][x-1]
```

자 이제 기준 블록의 3방향의 블록들을 체크해보자.

```python
def valid_block(to_be_checked, given_block):
    pass

def mark(board):
    for y in range(len(board)):
        for x in range(len(board[0])):
            block = board[y][x]
            if block == '-1':
                continue

            pos = [x-1, y, x, y-1, x-1, y-1]
            if any(p < 0 for p in pos) == True:
                continue

            # 어느 하나라도 기준에 부합하지 않으면 넘어간다.
            if not valid_block(board[y][x-1], block) or \
                not valid_block(board[y-1][x], block) or \
                not valid_block(board[y-1][x-1], block):
                continue
```

블록은 마킹되고 나서도 마킹되지 않은 같은 종류의 블록과 동일하게 다룰 수 있어야 한다.

이를 위해 특정 알파벳 블록을 집어넣으면 그와 연결된 숫자로 변환해주는 사전을 만들자.

예를 들어 A(key)라는 블록을 집어넣으면 '0'(value)을, Z(key)를 집어넣으면 '25'(value)을 반환하는 사전이다.

```python

BLOCK_TABLE = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
BLOCK_DICT = {block:str(i) for i, block in enumerate(BLOCK_TABLE)}

def valid_block(to_be_checked, given_block):
    pass

def mark(board):
    for y in range(len(board)):
        for x in range(len(board[0])):
            block = board[y][x]
            if block == '-1':
                continue

            pos = [x-1, y, x, y-1, x-1, y-1]
            if any(p < 0 for p in pos) == True:
                continue

            if not valid_block(board[y][x-1], block) or \
                not valid_block(board[y-1][x], block) or \
                not valid_block(board[y-1][x-1], block):
                continue
```

3방향 블록들의 어떤 것들을 체크해야 할까?

1. 블록들이 이미 지워졌는가? (하나라도 지워져 있다면 2X2의 단위 사각형을 이룰 수 없다.)
2. 블록들이 같은 블록인가? <br/>
   2-2. 이미 마킹되어 있다면 그 블록과 기준점 블록이 같은 종류의 블록인가?

```python
BLOCK_TABLE = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
BLOCK_DICT = {block:str(i) for i, block in enumerate(BLOCK_TABLE)}

def valid_block(to_be_checked, given_block):
    if to_be_checked == '-1' or given_block == '-1':
        return False
    elif to_be_checked == given_block:
        return True
    else:
        return to_be_checked == BLOCK_DICT[given_block]

def mark(board):
    for y in range(len(board)):
        for x in range(len(board[0])):
            block = board[y][x]
            if block == '-1':
                continue

            pos = [x-1, y, x, y-1, x-1, y-1]
            if any(p < 0 for p in pos) == True:
                continue

            if not valid_block(board[y][x-1], block) or \
                not valid_block(board[y-1][x], block) or \
                not valid_block(board[y-1][x-1], block):
                continue
```

체크해서 문제가 없을 경우 4개의 블록을 전부 마킹해주고 marked를 True로 바꾼다.

```python
BLOCK_TABLE = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
BLOCK_DICT = {block:str(i) for i, block in enumerate(BLOCK_TABLE)}

def valid_block(to_be_checked, given_block):
    if to_be_checked == '-1' or given_block == '-1':
        return False
    elif to_be_checked == given_block:
        return True
    else:
        return to_be_checked == BLOCK_DICT[given_block]

def mark(board):
    marked = False
    for y in range(len(board)):
        for x in range(len(board[0])):
            block = board[y][x]
            if block == '-1':
                continue

            pos = [x-1, y, x, y-1, x-1, y-1]
            if any(p < 0 for p in pos) == True:
                continue

            if not valid_block(board[y][x-1], block) or \
                not valid_block(board[y-1][x], block) or \
                not valid_block(board[y-1][x-1], block):
                continue

            board[y][x-1] = BLOCK_DICT[block]
            board[y-1][x] = BLOCK_DICT[block]
            board[y-1][x-1] = BLOCK_DICT[block]
            board[y][x] = BLOCK_DICT[block]
            marked = True

    return marked
```

### 3-2. 주어진 함수들을 구현 - delete()

마킹된 블록은 모두 string 형태의 숫자이므로 이를 체크하고 삭제("-1")가 되어 있지 않다면 삭제해주고 삭제한 블록 개수를 세어준다.

isalpha()는 해당 string이 알파벳으로만 이루어져 있는지 체크하는 내장함수이다.

```python
def delete(board):
    deleted_blocks = 0
    for y in range(len(board)):
        for x in range(len(board[0])):
            if not board[y][x].isalpha() and int(board[y][x]) > -1:
                board[y][x] = '-1'
                deleted_blocks += 1
    return deleted_blocks
```

### 3-3. 주어진 함수들을 구현 - pulldown()

![pulldown](/assets/img/algorithm/programmers/kakao/friends4-block/01.png)

위 그림의 A, B 블록을 떨어뜨릴 때는 아래와 같은 순서로 진행된다.

1. 0번과 1번을 지나 2번의 빈칸을 발견한다.
2. 빈칸의 좌표를 큐에 넣고 다음 칸으로 이동한다.
3. 3번의 빈칸을 발견하고 똑같이 큐에 좌표를 넣고 다음칸으로 이동한다.
4. 4번의 블록을 발견하고 아까 큐에서 빈칸 좌표를 하나 꺼내 그곳에 블록을 이동시키고 4번칸은 빈칸("-1") 처리 후 현재 좌표를 큐에 넣는다.
5. 5번의 블록을 발견하고 큐에서 빈칸 좌표를 하나 꺼내 그곳에 블록을 이동시키고 5번칸은 빈칸처리 후 현재 좌표를 큐에 넣는다.

이 1~5번 과정을 각 세로줄 마다 진행해 주면 된다.

```python
from collections import deque

def pulldown(board):
    for x in range(len(board[0])):
        blanks = deque()
        for y in range(len(board)-1, -1, -1):
            if board[y][x] == '-1':
                blanks.append([x, y])
            elif blanks:
                blank_x, blank_y = blanks.popleft()
                board[blank_y][blank_x] = board[y][x]
                board[y][x] = '-1'
                blanks.append([x, y])
```

## Comment

풀고 나서 보니 너무 장황하고 길고 마킹할 때 게임판을 한 번, 지울 때 또 한 번 돌면서 수행시간도 더 길어지는것도 마음에 안 들어서 추후에 시간 나면 다시 풀어봐야겠다.
