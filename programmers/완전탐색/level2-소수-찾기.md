# 완전탐색 - 소수 찾기 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42839)

## 풀이

완전탐색은 풀이 자체는 쉽지만, 어떤 때에 완전탐색을 해야 하는지 빠르게 판단하는 게 중요해 보인다.

```python
import itertools

def is_prime_number(number: int) -> bool:
    if number <= 1:
        return False
    
    for n in range(2, number):
        if number % n == 0:
            return False
    
    return True

def solution(numbers: str) -> int:
    permutations = []
    
    for i in range(1, len(numbers) + 1):
        permutations.extend(itertools.permutations(numbers, i))
    
    permutations = set([ int("".join(v)) for v in permutations ])
    
    prime_numbers = 0
    
    for number in permutations:
        if is_prime_number(number):
            prime_numbers += 1
    
    return prime_numbers
```

