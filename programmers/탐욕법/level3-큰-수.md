# 탐욕법 - 큰 수 만들기[🔗](https://programmers.co.kr/learn/courses/30/lessons/42883)

## 풀이

예전에 leetcode에서 풀어본 [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) 문제와 비슷하다. 그래서 스택으로 풀었는데, 오히려 탐욕법으로 어떻게 풀어야 할지 감을 못 잡겠다...

우선 `number`의 각 자리수를 순회하면서 스택에 담고, 만약 스택의 마지막 숫자가 현재 숫자보다 작을 경우, 스택에서 현재 숫자보다 작은 숫자들을 모두 pop하고 해당 인덱스를 `to_remove`에 추가한다.

이후 `to_remove`에 있는 숫자를 전부 -1로 치환하고, `to_remove`를 비워도 k가 남을 경우 맨 뒤의 수를 차례로 제거한다.

마지막으로 -1로 치환한 자리수를 제외하고 전부 한 문자열로 합쳐 반환한다.

```python
import collections

def solution(number, k):
    stack = [('9', -1)]
    to_remove = collections.deque()
    
    for i, n in enumerate(number):
        while n > stack[-1][0]:
            to_remove.append(stack.pop()[1])
        stack.append((n, i))
        continue
    
    n_list = list(number)
    while k > 0:
        if to_remove:
            n_list[to_remove.popleft()] = ''
        else:
            n_list.pop()    
        k -= 1
    
    return "".join([ n for n in n_list if n != '' ])
```

## 풀이2

기본적인 아이디어는 동일하지만 불필요한 부분을 제거하고 더 깔끔하게 수정했다.

`to_remove`를 없애고 스택에 결과값을 담는 것으로 바꾸었다. 

```python
def solution(number, k):
    stack = []
    for n in number:
        while k and stack and n > stack[-1]:
            stack.pop()
            k -= 1
        stack.append(n)
        continue
    
    if k > 0:
        stack = stack[:-k]
    
    return "".join(stack)
```

