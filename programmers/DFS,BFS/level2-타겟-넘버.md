# 깊이/너비 우선 탐색 - 타겟 넘버 [🔗](https://programmers.co.kr/learn/courses/30/lessons/43165)

## 풀이

재귀 종료조건을 잘 설정해주면 쉽게 풀 수 있다.

```python
def recursive(numbers, target):
    if len(numbers) == 0:
        return 1 if target == 0 else 0
    return recursive(numbers[:-1], target + numbers[-1]) + recursive(numbers[:-1], target - numbers[-1])

def solution(numbers, target):
    return recursive(numbers, target)
```

