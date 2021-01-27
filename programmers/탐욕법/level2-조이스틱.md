# 탐욕법 - 조이스틱[🔗](https://programmers.co.kr/learn/courses/30/lessons/42860)

## 풀이

기본적으로 현재 위치에서 가장 가까운, 조작해야 할 위치로 이동해서 알파벳을 입력하는 작업을 반복한다.

 단, 만약 동일한 거리의 이동해야 할 위치가 2개 존재할 경우, 어느 쪽으로 가느냐에 따라 결과가 달라지는 입력값이 있다. (`"BBABAAAB"`, 정답은 `9`이지만 코드에 따라 `11`이 나올 수 있다) 나는 앞으로 가는 걸 우선 체크하느냐, 뒤로 가는 걸 우선 체크하느냐에 따른 2가지 방법을 모두 확인하고 그 최솟값을 반환하는 것으로 해결했다. 

```python
def check(name, front_first):
    ord_A = ord('A')
    ord_Z = ord('Z')
    
    progress = [ True if c == "A" else False for c in name ]
    
    count = 0
    pointer = 0
    
    while pointer != -1:
        count += min(ord(name[pointer]) - ord_A, ord_Z - ord(name[pointer]) + 1)
        progress[pointer] = True
        
        min_distance = len(name)
        next_pointer = -1
        for i, c in enumerate(name):
            if progress[i]:
                continue
            big, small = max(i, pointer), min(i, pointer)
            distance = min(big - small, len(name) - big + small)
            condition = distance < min_distance if front_first else distance <= min_distance
            if condition:
                min_distance = distance
                next_pointer = i
        if min_distance != len(name):
            count += min_distance
        pointer = next_pointer
    return count

def solution(name):
    return min(check(name, True), check(name, False))
```

