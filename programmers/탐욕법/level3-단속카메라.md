# 탐욕법 - 단속카메라[🔗](https://programmers.co.kr/learn/courses/30/lessons/42884)

## 풀이

이전에 풀어본 카카오 [추석 트래픽](https://programmers.co.kr/learn/courses/30/lessons/17676) 문제와 비슷하다고 느꼈다. 차이점이라면 추석 트래픽에서는 특정 시점에 존재하는 모든 트래픽을 고려해야 했다면, 이 문제에서는 이전에 이미 처리된(즉, 카메라가 설치된) 구간은 고려할 필요가 없다는 점이었다. 이 차이 때문에 처음에는 접근방법을 실수해서 많이 애먹다가 테스트 케이스를 손으로 그려보고 잘못된 점을 알았다.

```python
def solution(routes):
    entry = []
    exit = []
    for i, route in enumerate(routes):
        entry.append((route[0], i))
        exit.append((route[1], i))
    entry.sort()
    exit.sort()

    entered_car = set()
    exited_car = set()
    camera = 0
    i = j = 0
    
    while j < len(exit):
        if i < len(entry) and entry[i][0] <= exit[j][0]:
            entered_car.add(entry[i][1])
            i += 1
        else:
            if exit[j][1] not in exited_car:
                exited_car = entered_car | exited_car
                entered_car = set()
                camera += 1
            j += 1
    
    return camera
```

## 풀이2

아마도 이 문제의 모범답안일 것 같은 풀이.

- `routes`를 차량이 나가는 시점을 기준으로 정렬한다.
- `routes`를 순회하며 다음을 수행한다.
  - 만약 이전 카메라가 차량 진입 시점보다 이전에 설치되어 있다면, 새 카메라를 차량 진출 시점에 설치한다.
  - 이전 카메라가 차량 진입 시점 에후에 설치되어 있다면 (이미 그 카메라에 찍혔을 것이므로) 무시한다.
- 설치한 카매라 개수를 return한다.

```python
def solution(routes):
    routes = sorted(routes, key=lambda x: x[1])
    prev_camera = -30000
    camera_count = 0

    for route in routes:
        if prev_camera < route[0]:
            camera_count += 1
            prev_camera = route[1]

    return camera_count
```

