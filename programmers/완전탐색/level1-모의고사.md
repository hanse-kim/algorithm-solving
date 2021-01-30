# 완전탐색 - 모의고사 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42862)

## 풀이

```python
def solution(answers):
    patterns = [
        [1,2,3,4,5],
        [2,1,2,3,2,4,2,5],
        [3,3,1,1,2,2,4,4,5,5]   
    ]
    
    score = [0, 0, 0]
    
    for i, answer in enumerate(answers):
        for j, p in enumerate(patterns):
            if answer == p[i % len(p)]:
                score[j] += 1
    
    return [ i + 1 for i, s in enumerate(score) if s == max(score) ]
```

