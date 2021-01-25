# 탐욕법 - 체육복 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42862)

## 풀이

```python
def solution(n, lost, reserve):
    lost_set = set(lost)
    reserve_set = set()
    
    for r in reserve:
        if r in lost_set:
            lost_set.remove(r)
            continue
        reserve_set.add(r)
    
    fail = 0
    
    for l in lost_set:
        if l-1 in reserve_set:
            reserve_set.remove(l-1)
            continue
        if l+1 in reserve_set:
            reserve_set.remove(l+1)
            continue
        fail += 1
    
    return n - fail
```

