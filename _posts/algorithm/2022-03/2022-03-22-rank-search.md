---
title: 2021 KAKAO BLIND RECRUITMENT - 순위 검색
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-03-22 18:18:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags:
  ["알고리즘", "프로그래머스", "2021", "카카오", "블라인드", "Binary Search"]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/72412](https://programmers.co.kr/learn/courses/30/lessons/72412)

## Introduction

푸는 데 몇 일씩 걸렸던 문제였다. 풀다 안풀려서 다른 문제 풀고 틈틈이 한 번씩 다시 시도해보고, 안되서 포기하고 다른 문제 풀러가고...

처음에는 info에 tag를 붙이는 방식으로 구현했는데 태깅한 info의 인덱스가 저장된 set를 읽어들이는데 시간이 많이 걸려서 정확성은 문제 없었는데 효율성에서 실패 했었다.

```python
from collections import defaultdict

def parse_query(query):
    q = query.split()
    return [q[0], q[2], q[4], q[6], q[7]]

def attach_tag(info):
    info_dic = defaultdict(set)
    for index, inf in enumerate(info):
        information = inf.split()
        for j in range(4):
            tag = information[j]
            info_dic[tag].add(index)
    return info_dic

def solution(info, query):
    answer = [0] * len(query)
    info_dic = attach_tag(info)

    for i, q in enumerate(query):
        result_set = {j for j in range(len(info))}
        conditions = parse_query(q)

        for condition in conditions:
            if not condition.isalpha() or condition == '-':
                continue
            result_set &= info_dic[condition]

        score_cut = int(conditions[-1])
        for result in result_set:
            score = int(info[result].split()[-1])
            if score >= score_cut:
                answer[i] += 1

    return answer
```

27번 라인의 `result_set &= info_dic[condition]` 이게 시간이 엄청 많이 걸려서 효율성을 통과하지 못했다. (이게 왜 통과할거라고 생각했는지는 아직도 의문이다.)

결국 혼자 힘으로 못 풀고 카카오 코테 해설을 봤다. ([https://tech.kakao.com/2021/01/25/2021-kakao-recruitment-round-1/](https://tech.kakao.com/2021/01/25/2021-kakao-recruitment-round-1/))

## Note

- 와일드 카드 (`-`)를 포함해서 검색어 조건의 모든 조합을 만들고 그루핑한다.
- score를 찾을 때 조차도 이진 탐색으로 찾지 않으면 시간초과가 난다.

## Solution

```python
from bisect import bisect_left
from itertools import combinations
from collections import defaultdict

def search(query, info_dic):
    key, score = ''.join(query[:-1]), int(query[-1])
    scores = info_dic[key]
    return len(scores) - bisect_left(scores, score)

def parse(query):
    return [q for q in query.split() if q != 'and']

def group(info):
    info_dic = defaultdict(list)
    for i in info:
        conditions = i.split()
        score = int(conditions[-1])
        for n in range(len(conditions)):
            for comb in combinations([0, 1, 2, 3], n):
                con = conditions[:-1]
                for index in comb:
                    con[index] = '-'

                key = ''.join(con)
                info_dic[key].append(score)

    for scores in info_dic.values():
        scores.sort()

    return info_dic

def solution(info, query):
    answer = []
    info_dic = group(info)
    for q in query:
        parsed_query = parse(q)
        result = search(parsed_query, info_dic)
        answer.append(result)
    return answer
```

### I. 필요한 함수 정의

우선 정답리스트와 함께 info를 넣어주면 `key:value`가 `조건묶음:점수리스트`로 그루핑된 Dictionary를 반환하는 함수를 정의한다.

```json
{
  "javabackendjuniorpizza": [150],
  "-backendjuniorpizza": [150],
  "java-juniorpizza": [150],
  "javabackend-pizza": [150],
  "javabackendjunior-": [80, 150],
  "pythonfrontendsenior-": [150, 210],
  "----": [50, 80, 150, 150, 210, 260]
}
```

```python
def group(info):
    pass

def solution(info, query):
    answer = []
    info_dic = group(info) # 1-1
    return answer
```

쿼리를 순회하면서 사용하기 편하게 파싱해줄 함수를 정의한다.

```python
def parse(query):
    pass

def group(info):
    pass

def solution(info, query):
    answer = []
    info_dic = group(info)
    for q in query:
        parsed_query = parse(query) # 1-2
    return answer
```

파싱된 query와 info dictionary를 넣어주면 조건에 해당하는 지원자가 몇 명인지 반환하는 함수를 정의하고, 이 결과값을 정답리스트에 넣는다.

```python
def search(query, info_dic):
    pass

def parse(query):
    pass

def group(info):
    pass

def solution(info, query):
    answer = []
    info_dic = group(info)
    for q in query:
        parsed_query = parse(query)
        result = search(parsed_query, info_dic) # 1-3
        answer.append(result)
    return answer
```

### II. 함수구현 - 그루핑

우선 value가 list인 dictionary를 준비한다.

```python
from collections import defaultdict

def group(info):
    info_dic = defaultdict(list) # 2-1
    return info_dict
```

info를 순회하면서 conditions와 score를 정리해놓는다.

```python
from collections import defaultdict

def group(info):
    info_dic = defaultdict(list)
    for i in info:
        conditions = i.split() # 2-2
        score = int(conditions[-1]) # 2-2
    return info_dict
```

conditions 중에서 n개 만큼 와일드카드(`-`)를 넣기 위해 n번 순회한다.

```python
from collections import defaultdict

def group(info):
    info_dic = defaultdict(list)
    for i in info:
        conditions = i.split()
        score = int(conditions[-1])
        for n in range(len(conditions)): # 2-3
            pass
    return info_dict
```

conditions는 순서대로 언어[0], 직군[1], 경력[2], 소울푸드[3], 점수[4]의 순서대로 이루어져 있다.

key에 인덱스 0~3번까지의 조건을 조합하고, value에 인덱스 4인 점수를 저장할 것이므로, [0, 1, 2, 3] 리스트에서 n개를 뽑아 조합을 만든다.

```python
from itertools import combinations
from collections import defaultdict

def group(info):
    info_dic = defaultdict(list)
    for i in info:
        conditions = i.split()
        score = int(conditions[-1])
        for n in range(len(conditions)):
            for comb in combinations([0, 1, 2, 3], n): # 2-4
                pass
    return info_dict
```

마지막 인덱스에 있는 점수만 빼고 자를 conditions를 새 리스트에 담고, 이 conditions에 조합된 와일드 카드를 집어넣어 바꿔준다.

```python
from itertools import combinations
from collections import defaultdict

def group(info):
    info_dic = defaultdict(list)
    for i in info:
        conditions = i.split()
        score = int(conditions[-1])
        for n in range(len(conditions)):
            for comb in combinations([0, 1, 2, 3], n):
                con = conditions[:-1] # 2-5
                for index in comb: # 2-5
                    con[index] = '-'
    return info_dict
```

준비된 리스트를 string 형태로 이어 붙인 뒤, info_dic에 key로, value 점수 리스트에는 현재 점수를 추가해준다.

```python
from itertools import combinations
from collections import defaultdict

def group(info):
    info_dic = defaultdict(list)
    for i in info:
        conditions = i.split()
        score = int(conditions[-1])
        for n in range(len(conditions)):
            for comb in combinations([0, 1, 2, 3], n):
                con = conditions[:-1]
                for index in comb:
                    con[index] = '-'

                key = ''.join(con)
                info_dic[key].append(score) # 2-6
    return info_dict
```

점수를 찾기 좋게 info_dic을 순회하면서 점수를 오름차순으로 정렬해준다.

```python
from itertools import combinations
from collections import defaultdict

def group(info):
    info_dic = defaultdict(list)
    for i in info:
        conditions = i.split()
        score = int(conditions[-1])
        for n in range(len(conditions)):
            for comb in combinations([0, 1, 2, 3], n):
                con = conditions[:-1]
                for index in comb:
                    con[index] = '-'

                key = ''.join(con)
                info_dic[key].append(score)

    for scores in info_dic.values(): # 2-7
        scores.sort()

    return info_dict
```

### III. 함수구현 - 파싱

쿼리가 들어오면 split()을 통해 자르고, and를 제외한 모든 조건을 리스트에 넣어 반환한다.

```python
def parse(query):
    return [q for q in query.split() if q != 'and'] # 3-1
```

### IV. 함수구현 - 탐색

우선 쿼리에서 조건 키와 점수를 나눈다.

```python
def search(query, info_dic):
    key, score = ''.join(query[:-1]), int(query[-1]) # 4-1
```

info_dic에서 scores 리스트를 가져온다.

```python
def search(query, info_dic):
    key, score = ''.join(query[:-1]), int(query[-1])
    scores = info_dic[key] # 4-2
```

카카오 코테 해설에는 이런 말이 있다.

```
...
이때, X점 이상 맞은 지원자를 찾기 위해 해당 그룹의 최저점, 혹은 최고점부터 순차적으로 검색한다면 여전히 오랜 시간이 걸리게 됩니다. 이때, 숫자가 오름차순으로 정렬된 배열에서 X라는 숫자를 찾는 효율적인 방법으로 binary search를 사용할 수 있습니다. 이때, 배열에 X가 없을 수도 있으므로, 배열에서 X보다 크거나 같은 숫자가 처음 나타나는 위치를 찾아야 하며, 이는 lower bound를 이용하면 됩니다.
...
```

파이썬에는 이 lower bound의 binary search를 제공하는 라이브러리가 있다.

bisect_left()는 첫 번째 인자로 찾을 리스트를, 두 번째 인자로 받은 숫자 이상의 첫 번째 순서의 숫자의 인덱스를 반환한다.

```python
a = [1, 2, 3, 5, 7, 10]
bisect.bisect_left(a, 4) # output: 3
```

즉 위 예제에서 4보다 큰 숫자의 갯수를 구하려면 `a의 길이 - bisect_left()로 얻은 인덱스`의 값을 구하면 된다.

```python
def search(query, info_dic):
    key, score = ''.join(query[:-1]), int(query[-1])
    scores = info_dic[key]
    return len(scores) - bisect_left(scores, score) # 4-3
```

## Comment

정말 극한으로 효율성을 신경써야지 통과할 수 있는 문제였다.

처음에 생각했던 태깅하는 방식은 진짜 만들어 놓고 "아 이런 참신한 생각은 아무도 못했겠지? 역시 난 멋져"하고 자뻑하고 있었는데 효율성에 걸릴 줄이야...
