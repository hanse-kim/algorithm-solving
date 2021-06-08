# 깊이/너비 우선 탐색 - 단어 변환 [🔗](https://programmers.co.kr/learn/courses/30/lessons/43163)

## 풀이

bfs로 순회하며 `count` 딕셔너리에 각 단어에 도달하는 최소거리를 기록한다. `word`가 `target`과 동일할 때 루프문을 멈추고 `count[target]`을 반환한다.

```python
import collections

def compare(word1, word2):
    differ = 0
    for c1, c2 in zip(word1, word2):
        if c1 != c2:
            differ += 1
            if differ > 1:
                return False
    return True

def get_next_words(word, words):
    result = []
    for next_word in words:
        if compare(word, next_word):
            result.append(next_word)
    return result

def solution(begin, target, words):
    if target not in words:
        return 0
    
    queue = collections.deque([begin])
    len_max = len(words) + 1
    count = {word:len_max for word in words}
    count[begin] = 0
    while queue:
        word = queue.popleft()
        if word == target:
            break
        next_words = get_next_words(word, words)
        for next_word in next_words:
            if count[next_word] == len_max:
                queue.append(next_word)
                count[next_word] = count[word] + 1

    return count[target] if count[target] != len_max else 0
```

