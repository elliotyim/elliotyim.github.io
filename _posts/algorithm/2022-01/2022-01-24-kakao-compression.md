---
title: 2018 KAKAO BLIND RECRUITMENT [3차] 압축
author:
  name: Elliot Yim
  link: https://github.com/elliotyim
date: 2022-01-24 14:56:00 +0900
categories: ["알고리즘", "프로그래머스"]
tags:
  [
    "알고리즘",
    "프로그래머스",
    "2018",
    "카카오",
    "블라인드",
    "투포인터",
    "슬라이딩 윈도우",
  ]
---

## Link

- [https://programmers.co.kr/learn/courses/30/lessons/17684](https://programmers.co.kr/learn/courses/30/lessons/17684)

## Introduction

나한테는 안 좋은 습관이 있다. 바로 코딩하기 전에 내가 해결하려는 문제가 명확하고 깔끔한 어떤 절대적 진리와도 같은(마치 유명한 수학 공식들과도 같이 아름다운 형태를 지닌) 풀이가 있을 것이라고 생각하고 코딩하는 것이다.

이 사고방식은 때때로 내 발목을 잡는데 바로 처음 겪는 문제를 해결할 때 일단 도전해보기 전에 (또는 도전하는 도중에도) 생각이 많아지게 하는 것이 그렇다.

![So much thinking](/assets/img/meme/so-much-thinking.jpg)_더 좋은 방법이 있을거야..찾아내야 해_

그래서 문제를 풀기 전이나 심지어 푸는 도중에도 끊임없이 "아 이건 아닌거 같은데", "더 좋은 방법이 있을텐데"라는 생각을 하며 코딩을 한다. 지금 작성하고 있는 코드가 완성되기도 전에 말이다.

당연히 문제에 집중이 될리가 없고, 자꾸 제자리만 멤도는 경우가 많았다.

이번 문제를 풀면서 특히 그랬던 것 같다. 보자마자 딱 떠오른 것은 트라이 알고리즘이었는데 풀이를 보면 알겠지만 그런거 관계없고 그냥 문제에 써져 있는대로 구현하면 된다.

처음에는 정말 되는대로 풀었다가 리팩토링하고 나니 문제에 써져있는대로 구현이 되어 있었다.

문제를 해결할 때 처음부터 너무 완벽하게 하려고 하는게 언제나 좋지만은 않을 수도 있겠다는 생각이 들었다. 일단 해결하고 보완하는 것이 아예 모르는 문제를 해결할 때는 더 적합하겠다는 생각이 든다.

## Note

- 슬라이딩 윈도우 알고리즘을 사용

## Solution

1, 필요한 변수들과 사전(dictionary)을 준비해둔다. 사전에는 미리 알파벳에 인덱스를 붙인 값을 넣어서 준비한다.

> ex) word_dic[word] = 12

알파벳은 총 26글자 이므로 다음 인덱스는 27로 미리 세팅해 놓는다.

```python
WORD_TABLE = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

def solution(msg):
    answer, index = [], 27
    word_dic = {word:i+1 for i, word in enumerate(WORD_TABLE)} # 1
```

2, 포인터를 두 개 두고 right를 끝까지 움직이도록 한다.

```python
WORD_TABLE = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

def solution(msg):
    answer, index = [], 27
    word_dic = {word:i+1 for i, word in enumerate(WORD_TABLE)}

    left, right = 0, 1
    while right <= len(msg): # 2
        word = msg[left:right]
        right += 1

```

3, 사전에 있는 단어면 정답 리스트에 넣고 left의 위치를 옮겨준다.

```python
WORD_TABLE = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

def solution(msg):
    answer, index = [], 27
    word_dic = {word:i+1 for i, word in enumerate(WORD_TABLE)}

    left, right = 0, 1
    while right <= len(msg):
        word = msg[left:right]

        if word in word_dic: # 3
            answer.append(word_dic[word])
            left = right

        right += 1

```

4, 3번의 과정만으로는 압축이 안되므로, (사전에는 'A', 'B'와 같은 한 단어들만 있을테니) 압축을 시켜주기 위해 다음 글자까지 포함된 string이 사전에 있는지를 체크하고 있으면 right만 한 칸 옮기고, 없으면 다음 글자까지 포함된 string을 사전에 넣고 현재 글자를 정답에 넣는다.

```python
WORD_TABLE = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

def solution(msg):
    answer, index = [], 27
    word_dic = {word:i+1 for i, word in enumerate(WORD_TABLE)}

    left, right = 0, 1
    while right <= len(msg):
        word = msg[left:right]

        if word in word_dic:
            if msg[left:right+1] in word_dic: # 4
                right += 1
                continue
            else:
                word_dic[msg[left:right+1]] = index
                index += 1

            answer.append(word_dic[word])
            left = right

        right += 1

```

5, 위 로직은 마지막 단어가 사전에 들어있든 없든 절대 answer.append(word_dic[word]) 부분으로 가지 못한다. 따라서 마지막 문자를 정답에 넣어주고 반환한다.

```python
WORD_TABLE = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

def solution(msg):
    answer, index = [], 27
    word_dic = {word:i+1 for i, word in enumerate(WORD_TABLE)}

    left, right = 0, 1
    while right <= len(msg):
        word = msg[left:right]
        if word in word_dic:
            if msg[left:right+1] in word_dic:
                right += 1
                continue
            else:
                word_dic[msg[left:right+1]] = index
                index += 1
            answer.append(word_dic[word])
            left = right
        right += 1

    answer.append(word_dic[msg[left:]]) # 5
    return answer
```

근데 이렇게 작성하고 보니까 풀이가 너무 지저분하고, if문을 살펴보니 리팩토링을 할 여지가 보였다.

그래서 리팩토링을 하니 아래와 같이 깔끔하게 정리가 되었다.

---

```python
WORD_TABLE = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

def solution(msg):
    answer, index = [], 27
    word_dic = {word:i+1 for i, word in enumerate(WORD_TABLE)}

    left, right = 0, 1
    while right <= len(msg):
        word, next_word = msg[left:right], msg[left:right+1]
        if word in word_dic and next_word not in word_dic:
            answer.append(word_dic[word])
            word_dic[next_word] = index
            index += 1
            left = right
        right += 1

    answer.append(word_dic[msg[left:]])
    return answer
```

리팩토링 해놓고 보니 문제에 써져 있는 풀이방법과 유사하다.

```markdown
...

예를 들어 입력으로 `KAKAO`가 들어온다고 하자.

1. 현재 사전에는 `KAKAO`의 첫 글자 `K`는 등록되어 있으나,
   두 번째 글자까지인 `KA`는 없으므로,
   첫 글자 `K`에 해당하는 색인 번호 11을 출력하고,
   다음 글자인 `A`를 포함한 `KA`를 사전에 27 번째로 등록한다.
2. 두 번째 글자 `A`는 사전에 있으나,
   세 번째 글자까지인 `AK`는 사전에 없으므로,
   `A`의 색인 번호 1을 출력하고, `AK`를 사전에 28 번째로 등록한다.
3. 세 번째 글자에서 시작하는 `KA`가 사전에 있으므로,
   `KA`에 해당하는 색인 번호 27을 출력하고,
   다음 글자 `O`를 포함한 `KAO`를 29 번째로 등록한다.
4. 마지막으로 처리되지 않은 글자 `O`에 해당하는 색인 번호 15를 출력한다.

...
```

때로는 처음부터 정확한 접근법으로 풀어나가지 않아도 된다. 일단 도전하고 조금씩 고치고 수정하다보면 세련된 결과물이 나올거라 믿는다.
