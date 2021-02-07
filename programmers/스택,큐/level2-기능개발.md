# 스택/큐 - 기능개발 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42586)

##### 문제 설명

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

##### 제한 사항

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

##### 입출력 예

| progresses               | speeds             | return    |
| ------------------------ | ------------------ | --------- |
| [93, 30, 55]             | [1, 30, 5]         | [2, 1]    |
| [95, 90, 99, 99, 80, 99] | [1, 1, 1, 1, 1, 1] | [1, 3, 2] |

## 풀이

완료된 기능 순서대로 처리해야 하므로 큐를 이용한다.

우선 작업 진도와 작업 속도로부터 완성까지 남은 일수를 구해 큐에 추가한다. 남은 진도를 작업 속도로 나누어 올림한 값이 남은 일수다.

이후 큐에서 값을 하나 pop하고, 해당 값보다 높거나 같은 값들은 이미 완성된 기능이란 의미이므로 전부 pop하고 카운트한다. 이를 반복하여 결과를 구한다.

```python
import collections
import math

def solution(progresses, speeds):
    queue = collections.deque()
    for progress, speed in zip(progresses, speeds):
        left_days = math.ceil((100 - progress) / speed)
        queue.append(left_days)
    
    result = []
    
    while queue:
        publish_day = queue.popleft()
        count = 1
        while queue and queue[0] <= publish_day:
            queue.popleft()
            count += 1
        result.append(count)
    
    return result
```

