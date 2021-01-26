# 탐욕법 - 구명보트 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42885)

## 풀이

people을 오름차순으로 정렬한 뒤, 매번 최대값과 최소값의 합을 limit와 비교한다.

만약 최소값 + 최대값이 limit 이하이면, 둘이서 보트를 탈 수 있다는 뜻이므로 pair_count를 +1한다. 그리고 최소값 포인터를 한칸 위로 이동한다.

그리고 limit와 비교한 결과와 관계 없이 최대값 포인터를 한칸 아래로 이동한다.

두 포인터가 만나면 루프를 종료하고, (전체 사람 수) - (둘이서 보트를 탄 수)를 결과로 반환한다.

```python
def solution(people, limit):
    people.sort()
    left = 0
    right = len(people) - 1
    pair_count = 0
    
    while left < right:
        if people[left] + people[right] <= limit:
            left += 1
            pair_count += 1
        right -= 1
    
    return len(people) - pair_count
```

