---
title: 2022 KAKAO BLIND RECRUITMENT 양궁대회
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-19 19:04:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags: ["알고리즘", "프로그래머스", "2022", "카카오", "블라인드", "완전탐색"]
---

**Updated at 2022-01-30**

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/92342](https://programmers.co.kr/learn/courses/30/lessons/92342)

## Introduction

이번에도 달라진 풀이 방법을 적용하여 다시 풀어본다. 아이디어는 똑같다.

1. 화살을 가지고 큰 점수부터 점수판을 순회하며 쏠지 말지 결정한다.
2. 점수 계산을 하고 의미 있는 점수면 수집한다.
3. 점수판 중 가장 적게 맞춘 점수판을 골라 반환한다.

## Note

- n이 크지 않아 완전탐색으로 풀어도 충분하다.
- 같은 득점 중에서도 적은 점수를 많이 맞춘 결과를 선택할 것
- 문제를 잘 읽읍시다.
- 카카오 프렌즈의 캐릭터 이름은 **'lion'**이 아니라 **'ryan'** 입니다.

## Solution

```python
SCORE_TABLE = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

answer = []
max_score = 0

def get_score(ryan, apeach):
    score = 0
    for i in range(len(ryan)):
        if ryan[i] > apeach[i]:
            score += SCORE_TABLE[i]
        elif ryan[i] < apeach[i]:
            score -= SCORE_TABLE[i]
    return score

def shoot(i, arrows, ryan, apeach):
    global max_score
    if i == len(ryan)-1 or arrows == 0:
        ryan_board = [_ for _ in ryan]
        ryan_board[i] += arrows
        score = get_score(ryan_board, apeach)

        if 0 < score and score == max_score:
            answer.append(ryan_board[::-1]) # 7
        elif score > max_score:
            max_score = score
            answer.clear()
            answer.append(ryan_board[::-1]) # 7
        return

    if arrows > 0 and arrows >= apeach[i]+1:
        ryan[i] = apeach[i]+1
        shoot(i+1, arrows-ryan[i], ryan, apeach)
        ryan[i] = 0

    shoot(i+1, arrows, ryan, apeach)

def solution(n, info):
    i, ryan = 0, [0 for _ in info]
    shoot(i, n, ryan, info)
    return sorted(answer)[-1][::-1] if answer else [-1] # 7
```

---

**풀이**

1, info의 갯수 만큼(그래봐야 11개지만) 라이언을 위한 점수판을 마련한다.

```python
def solution(n, info):
    i, ryan = 0, [0] * len(info) # 1
```

2, 음...일단 한번 무지성으로 가지고 있는 화살을 큰 점수부터 화살을 다 쓸 때까지 한 발씩 점수별로 쏴보자.

여기서 shoot 함수의 base condition(함수의 탈출조건)은 과녁이 0점까지 도달했을 때(인덱스가 마지막일 때)와 화살이 다 떨어졌을 때이다.

```python
def shoot(i, arrows, ryan, apeach):
    if i == len(ryan)-1 or arrows == 0:
        return
    shoot(i+1, arrows-1, ryan, apeach)

def solution(n, info):
    i, ryan = 0, [0 for _ in info]
    shoot(i, n, ryan, info) # 2
```

3, 쏘는 건 구현했으니 좀 더 현명하게 쏠지 말지를 결정하면서 쏴보자.

갖고 있는 화살이 0개 보다 많고, 어피치가 쏜 화살보다 1개 이상 많으면 쏜다.

쏘고 나니 마음에 안드는 경우 방금 쏜 화살을 다시 회수(ryan[i] = 0)하고 해당 과녁은 지나간다.

![도르마무](/assets/img/meme/dormammu.jpg)

```python
def shoot(i, arrows, ryan, apeach):
    if i == len(ryan)-1 or arrows == 0:
        return

    # 과녁을 향해 쏘세요!
    if arrows > 0 and arrows >= apeach[i]+1: # 3
        ryan[i] = apeach[i]+1
        shoot(i+1, arrows-(apeach[i]+1), ryan, apeach)
        ryan[i] = 0

    # 안 쏘거나 위에서 못 쐈을 때 이 과녁은 그냥 지나간다.
    shoot(i+1, arrows, ryan, apeach)

def solution(n, info):
    i, ryan = 0, [0 for _ in info]
    shoot(i, n, ryan, info)
```

4, 점수판(ryan_board)을 찍어보자. 어 그런데 점수판 목록을 보니 뭔가가 이상하다.

```
...
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
...
```

쏴도 되고 안 쏴도 된다고 하니까 화살 10개를 줬는데도 한 번도 안 쏘고 10개를 계속 가지고 있다. 아직은 쏠 때가 아니라나 뭐라나

...뺏어서 지금 과녁에 남은 화살 다 쏴버렸다.

```python
def shoot(i, arrows, ryan, apeach):
    if i == len(ryan)-1 or arrows == 0:
        ryan_board = [_ for _ in ryan]
        ryan_board[i] += arrows # 4
        return

    if arrows > 0 and arrows >= apeach[i]+1:
        ryan[i] = apeach[i]+1
        shoot(i+1, arrows-(apeach[i]+1), ryan, apeach)
        ryan[i] = 0

    shoot(i+1, arrows, ryan, apeach)

def solution(n, info):
    i, ryan = 0, [0 for _ in info]
    shoot(i, n, ryan, info)
```

5, 이제 점수 계산을 하기 위해 점수판을 두 개를 넣어주면 점수차를 반환하는 함수를 구현하고 점수차를 구해보자.

```python
SCORE_TABLE = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

def get_score(ryan, apeach):
    score = 0
    for i in range(len(ryan)):
        if ryan[i] > apeach[i]:
            score += SCORE_TABLE[i]
        elif ryan[i] < apeach[i]:
            score -= SCORE_TABLE[i]
    return score

def shoot(i, arrows, ryan, apeach):
    if i == len(ryan)-1 or arrows == 0:
        ryan_board = [_ for _ in ryan]
        ryan_board[i] += arrows
        score = get_score(ryan_board, apeach) # 5
        return

    if arrows > 0 and arrows >= apeach[i]+1:
        ryan[i] = apeach[i]+1
        shoot(i+1, arrows-(apeach[i]+1), ryan, apeach)
        ryan[i] = 0

    shoot(i+1, arrows, ryan, apeach)

def solution(n, info):
    i, ryan = 0, [0 for _ in info]
    shoot(i, n, ryan, info)
```

6, 최고 점수를 갱신한 점수판들만 정답리스트에 모아 넣는다.
체크하는 기준은 아래와 같다.<br/>(최고점수의 초기값은 0)

1. 점수가 0보다 크면서(유의미한 점수) 점수가 최고점수와 같은가?
   - 정답 리스트에 똑같이 넣어주면 된다.
2. 최고점수보다 높은가?
   - 정답 리스트를 다시 만들고 거기에 지금 점수판을 넣어준다.

```python
SCORE_TABLE = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

answer = []
max_score = 0

def get_score(ryan, apeach):
    score = 0
    for i in range(len(ryan)):
        if ryan[i] > apeach[i]:
            score += SCORE_TABLE[i]
        elif ryan[i] < apeach[i]:
            score -= SCORE_TABLE[i]
    return score

def shoot(i, arrows, ryan, apeach):
    global max_score
    if i == len(ryan)-1 or arrows == 0:
        ryan_board = [_ for _ in ryan]
        ryan_board[i] += arrows
        score = get_score(ryan_board, apeach)

        if 0 < score and score == max_score: # 6
            answer.append(ryan_board)
        elif score > max_score:
            max_score = score
            answer.clear()
            answer.append(ryan_board)
        return

    if arrows > 0 and arrows >= apeach[i]+1:
        ryan[i] = apeach[i]+1
        shoot(i+1, arrows-ryan[i], ryan, apeach)
        ryan[i] = 0

    shoot(i+1, arrows, ryan, apeach)

def solution(n, info):
    i, ryan = 0, [0 for _ in info]
    shoot(i, n, ryan, info)
```

7, 마지막으로 정답리스트 중에 가장 적은 과녁에 많이 맞힌 점수판을 골라 반환한다.

이를 위해 정답 리스트에 값을 넣을 때 점수판 배열을 뒤집어서 넣어주는 걸로 수정하고([::-1] 연산) 정답을 반환할 때 정답리스트를 정렬한 뒤 가장 마지막에 위치한 점수판을 다시 원래대로 뒤집어서 반환한다.

뒤집은 점수판은 앞에서부터(0점 과녁부터) 숫자가 적은 순서대로 정렬될 것이고, 그 말은 정렬되고 난 뒤집힌 점수판 중에 가장 뒤에 위치하는 녀석들은 앞에서부터 숫자가 큰 녀석들만 들어간 상태로 배치된다는 것이다.

```python
SCORE_TABLE = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

answer = []
max_score = 0

def get_score(ryan, apeach):
    score = 0
    for i in range(len(ryan)):
        if ryan[i] > apeach[i]:
            score += SCORE_TABLE[i]
        elif ryan[i] < apeach[i]:
            score -= SCORE_TABLE[i]
    return score

def shoot(i, arrows, ryan, apeach):
    global max_score
    if i == len(ryan)-1 or arrows == 0:
        ryan_board = [_ for _ in ryan]
        ryan_board[i] += arrows
        score = get_score(ryan_board, apeach)

        if 0 < score and score == max_score:
            answer.append(ryan_board[::-1]) # 7
        elif score > max_score:
            max_score = score
            answer.clear()
            answer.append(ryan_board[::-1]) # 7
        return

    if arrows > 0 and arrows >= apeach[i]+1:
        ryan[i] = apeach[i]+1
        shoot(i+1, arrows-ryan[i], ryan, apeach)
        ryan[i] = 0

    shoot(i+1, arrows, ryan, apeach)

def solution(n, info):
    i, ryan = 0, [0 for _ in info]
    shoot(i, n, ryan, info)
    return sorted(answer)[-1][::-1] if answer else [-1] # 7
```

## Legacy

지난 풀이. 과거의 생각.

다 풀었는데 자꾸 테스트 케이스 8번과 18번이 통과가 안되서 삽질을 엄청 했다.

처음 내가 푼 방식은 백트래킹의 기본형으로 남은 화살의 개수가 0개가 될 때까지 체크하다가 0개가 되면 둘의 점수차를 비교하고 유효하면 정답 리스트에 넣는 방식이었다.

문제의 조건에는 이런게 있었다.

```
...

- 라이언이 가장 큰 점수 차이로 우승할 수 있는 방법이 여러 가지 일 경우,
  가장 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.
  - 예를 들어,
    [2,3,1,0,0,0,0,1,3,0,0]과
    [2,1,0,2,0,0,0,2,3,0,0]를 비교하면
    [2,1,0,2,0,0,0,2,3,0,0]를 return 해야 합니다.

...
```

혹시나 해서 두 리스트를 새로운 리스트에 넣고 sort()를 하니 원하는 대로 [2,1,0,2,0,0,0,2,3,0,0]가 가장 앞으로 왔다.

![aha](/assets/img/meme/cheers-pepe.jpg)
_ㅎㅎ 이거네 개꿀_

근데 자꾸만 테스트케이스 8번과 18번에서만 걸려서 하루종일 삽질을 하다가 이상하다 싶어서 문제를 다시 읽어보았다.

```
 ...

 - 다른 예로,
   [0,0,2,3,4,1,0,0,0,0,0]과
   [9,0,0,0,0,0,0,0,1,0,0]를 비교하면
   [9,0,0,0,0,0,0,0,1,0,0]를 return 해야 합니다.

 ...
```

![...](/assets/img/algorithm/programmers/kakao/nothing-to-say.jpg)
_dk.._

sort()를 해버리면 단순히 숫자의 높고 낮음으로 정렬하기에 위의 상황에서 [0,0,2,3,4,1,0,0,0,0,0]을 반환한다. 그래서 결국 pick_one()이라는 함수를 구현하여 더 적은 점수를 많이 맞춘 정답을 뽑도록 하였다.

그렇게 완성된게 이 풀이

```python
score_table = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
max_score = 0

def pick_one(answer):
    result = []
    for i in range(10, -1, -1):
        most_hit, count = 0, 0
        for ans in answer:
            if ans[i] > most_hit:
                most_hit, result = ans[i], ans
                count += 1
        if count == 1:
            return result
    return [-1]

def get_score(apeach, lion):
    score = 0
    for i in range(len(apeach)):
        if apeach[i] > lion[i]:
            score -= score_table[i]
        elif apeach[i] < lion[i]:
            score += score_table[i]
    return score

def helper(n, apeach, lion, answer, visited):
    global max_score
    if n == 0:
        score = get_score(apeach, lion)
        if score <= 0:
            return
        elif score == max_score:
            answer.append([_ for _ in lion])
        elif score > max_score:
            answer.clear()
            answer.append([_ for _ in lion])
            max_score = score
        return
    elif n > 0:
        for i in range(len(lion)):
            if not visited[i]:
                visited[i] = True
                lion[i] = n if i == len(lion)-1 else apeach[i]+1
                helper(n - lion[i], apeach, lion, answer, visited)
                lion[i] = 0
                visited[i] = False

def solution(n, info):
    answer, lion, visited = [], [0 for _ in info], [False for _ in info]
    helper(n, info, lion, answer, visited)
    return pick_one(answer)
```

![...](/assets/img/algorithm/programmers/kakao/archery_solve001.png){: width="33%" height="33%"}
_수행시간 편차가 엄청 크다_

근데 이상하게 수행시간도 엄청 길고 answer에도 같은 값이 6개씩 들어가 있고 뭔가 구현이 이상한 것 같아 결국 인터넷에서 다른 사람의 풀이를 참고해서 index를 늘려가며 탐색하는 방식으로 다시 풀었다.

---

```python
score_table = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
max_score = 0

def pick_one(answer):
    result = []
    for i in range(10, -1, -1):
        most_hit, count = 0, 0
        for ans in answer:
            if ans[i] > most_hit:
                most_hit, result = ans[i], ans
                count += 1
        if count == 1:
            return result
    return [-1]

def get_score(apeach, ryan):
    score = 0
    for i in range(len(apeach)):
        if apeach[i] > ryan[i]:
            score -= score_table[i]
        elif apeach[i] < ryan[i]:
            score += score_table[i]
    return score

def helper(index, arrows, apeach, ryan, answer):
    global max_score

    if index == len(ryan)-1 or arrows == 0: # 1-1
        ryan[index] += arrows
        score = get_score(apeach, ryan) # 1-2

        if 0 < score and score == max_score: # 1-3
            answer.append([r for r in ryan])
        elif score > max_score: # 1-4
            answer.clear()
            answer.append([r for r in ryan])
            max_score = score

        ryan[index] = 0 # 1-5
        return

    if arrows - (apeach[index]+1) >= 0: # 2
        ryan[index] = apeach[index]+1
        helper(index+1, arrows-ryan[index], apeach, ryan, answer)
        ryan[index] = 0

    helper(index+1, arrows, apeach, ryan, answer) # 3

def solution(n, info):
    answer, ryan = [], [0 for _ in info]
    helper(0, n, info, ryan, answer)
    return pick_one(answer)
```

1. 0~10까지 인덱스를 가지고 재귀를 돈다. base condition(탈출 조건)은 index가 배열의 끝에 도달했을 때 또는 가지고 있는 화살을 다 쏘았을 때이다.
   1. 마지막 인덱스(0점 과녁)인데도 화살이 남아있을 경우 다 쏜다.
   2. 어피치와 라이언의 점수차를 구한다.
   3. 이긴 경우만(0점 초과) 고려하고 점수가 최고 점수와 같다면 원래 있던 정답 리스트에 현재까지 쏜 화살의 기록을 적는다.
   4. 방금 라이언이 쏜 화살들이 최고점 보다 더 높다면 정답 리스트를 초기화 하고 현재까지 쏜 화살의 기록을 적는다.
   5. 1번에서 쏜 화살들을 전부 ~~도르마무~~ 초기화 하고 함수를 끝낸다.
2. 어피치가 쏜 화살보다 1개 더 쏠 수 있을 경우 쏘고(재귀함수 호출) 다시 돌아왔을 때 방금 쏜 화살을 초기화한다.
3. 안 쏘고 넘어간다.(인덱스만 +1)

![...](/assets/img/algorithm/programmers/kakao/archery_solve002.png){: width="33%" height="33%"}
