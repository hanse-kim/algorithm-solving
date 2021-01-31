# 완전탐색 - 카펫 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42842)

## 풀이

```python
def solution(brown, yellow):
    for h in range(1, yellow + 1):
        w = yellow / h
        if w % 1 != 0:
            continue
        
        if (w + h) * 2 + 4 == brown:
            return (w + 2, h + 2)
    
    return -1
```

