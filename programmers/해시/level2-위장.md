# 해시 - 위장 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42578)

## 풀이

```python
import collections
import functools

def solution(clothes):
    clothes_counter = collections.Counter([ category for name, category in clothes ])
    return functools.reduce(lambda x, y: x * (y + 1), clothes_counter.values(), 1) - 1
```

## 풀이 - 패키지 사용 X

위 풀이를 풀어서 작성한 것이다.

기본적으로 의상의 조합 수는 각 부위별 의상의 가짓수를 곱한 것이다. 단 특정 부위에 의상을 입지 않은 경우도 고려해야 하기 때문에, 결과는 (각 부위별 의상 수 + 해당 부위에 입지 않았을 경우들의 곱) - (모든 부위에 입지 않았을 경우) = (각 부위별 의상 수 + 1들의 곱) - 1이다. 

```python
def solution(clothes):
    clothes_counter = {}
    for name, category in clothes:
        if category not in clothes_counter:
            clothes_counter[category] = 0
        clothes_counter[category] += 1
    
    result = 1
    for i in clothes_counter.values():
        result *= (i + 1)
    
    return result - 1
```

