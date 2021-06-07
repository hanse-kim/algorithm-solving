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

```python
def solution(n, lost, reserve):
    reservable = [n for n in reserve if n not in lost]
    real_lost = [n for n in lost if n not in reserve]
    
    for reservable_n in reservable:
        reservable_front = reservable_n - 1
        reservable_back = reservable_n + 1
        if reservable_front in real_lost:
            real_lost.remove(reservable_front)
        elif reservable_back in real_lost:
            real_lost.remove(reservable_back)
    
    return n - len(real_lost)
```

