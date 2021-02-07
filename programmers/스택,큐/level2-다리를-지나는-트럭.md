# 스택/큐 - 다리를 지나는 트럭 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42583)

##### 문제 설명

트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.

예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.

| 경과 시간 | 다리를 지난 트럭 | 다리를 건너는 트럭 | 대기 트럭 |
| --------- | ---------------- | ------------------ | --------- |
| 0         | []               | []                 | [7,4,5,6] |
| 1~2       | []               | [7]                | [4,5,6]   |
| 3         | [7]              | [4]                | [5,6]     |
| 4         | [7]              | [4,5]              | [6]       |
| 5         | [7,4]            | [5]                | [6]       |
| 6~7       | [7,4,5]          | [6]                | []        |
| 8         | [7,4,5,6]        | []                 | []        |

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.

solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

##### 제한 조건

- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

##### 입출력 예

| bridge_length | weight | truck_weights                   | return |
| ------------- | ------ | ------------------------------- | ------ |
| 2             | 10     | [7,4,5,6]                       | 8      |
| 100           | 100    | [10]                            | 101    |
| 100           | 100    | [10,10,10,10,10,10,10,10,10,10] |        |

## 풀이

대기 트럭과 다리를 건너는 트럭을 각각 큐로 만들어 풀이했다. 트럭은 (무게, 진입시간) 튜플로 풀다가 가독성이 너무 떨어지는 것 같아 따로 클래스를 정의했다.

로직은 다음과 같다.

1. 다리가 견디는 무게만큼 트럭을 다리에 진입시킨다.
   * 진입 중에 다리를 다 건넌 트럭이 있는지도 확인한다.
2. 다리를 지나는 중인 트럭이 있을 경우, 맨 앞의 트럭을 큐에서 빼고 해당 트럭의 진입시간 + 다리 길이를 현재 시간으로 삼는다. -1을 해주는 이유는 한 트럭이 빠져나감과 동시에 진입하는 트럭이 있을 경우 동시에 처리하기 위해서이다.
3. 마지막으로 이전의 -1을 보상해주기 위해 현재시간에 +1해서 return한다.

```python
import collections

class truck:
    def __init__(self, weight, enter_time):
        self.weight = weight
        self.enter_time = enter_time
    
    def __str__(self):
        return f"truck(weight:{self.weight}, enter_time:{self.enter_time})"
    
    def __repr__(self):
        return f"truck(weight:{self.weight}, enter_time:{self.enter_time})"
        
def solution(bridge_length, weight, truck_weights):
    truck_weights = collections.deque(truck_weights)
    passing_trucks = collections.deque()
    passing_weight = 0
    time = 0
    
    while truck_weights or passing_trucks:
        while truck_weights and passing_weight + truck_weights[0] <= weight:
            time += 1
            passing_trucks.append(truck(truck_weights.popleft(), time))
            passing_weight += passing_trucks[-1].weight
            
            if time - passing_trucks[0].enter_time == bridge_length:
                passing_weight -= passing_trucks.popleft().weight
                
        if passing_trucks:
            passed_truck = passing_trucks.popleft()
            time = bridge_length + passed_truck.enter_time - 1
            passing_weight -= passed_truck.weight
    
    return time + 1
```

