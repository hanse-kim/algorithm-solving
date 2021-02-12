# 힙 - 디스크 컨트롤러 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42627)

##### 문제 설명

하드디스크는 한 번에 하나의 작업만 수행할 수 있습니다. 디스크 컨트롤러를 구현하는 방법은 여러 가지가 있습니다. 가장 일반적인 방법은 요청이 들어온 순서대로 처리하는 것입니다.

예를들어

```
- 0ms 시점에 3ms가 소요되는 A작업 요청
- 1ms 시점에 9ms가 소요되는 B작업 요청
- 2ms 시점에 6ms가 소요되는 C작업 요청
```

와 같은 요청이 들어왔습니다. 이를 그림으로 표현하면 아래와 같습니다.
![Screen Shot 2018-09-13 at 6.34.58 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/b68eb5cec6/38dc6a53-2d21-4c72-90ac-f059729c51d5.png)

한 번에 하나의 요청만을 수행할 수 있기 때문에 각각의 작업을 요청받은 순서대로 처리하면 다음과 같이 처리 됩니다.
![Screen Shot 2018-09-13 at 6.38.52 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/5e677b4646/90b91fde-cac4-42c1-98b8-8f8431c52dcf.png)

```
- A: 3ms 시점에 작업 완료 (요청에서 종료까지 : 3ms)
- B: 1ms부터 대기하다가, 3ms 시점에 작업을 시작해서 12ms 시점에 작업 완료(요청에서 종료까지 : 11ms)
- C: 2ms부터 대기하다가, 12ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 16ms)
```

이 때 각 작업의 요청부터 종료까지 걸린 시간의 평균은 10ms(= (3 + 11 + 16) / 3)가 됩니다.

하지만 A → C → B 순서대로 처리하면
![Screen Shot 2018-09-13 at 6.41.42 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/9eb7c5a6f1/a6cff04d-86bb-4b5b-98bf-6359158940ac.png)

```
- A: 3ms 시점에 작업 완료(요청에서 종료까지 : 3ms)
- C: 2ms부터 대기하다가, 3ms 시점에 작업을 시작해서 9ms 시점에 작업 완료(요청에서 종료까지 : 7ms)
- B: 1ms부터 대기하다가, 9ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 17ms)
```

이렇게 A → C → B의 순서로 처리하면 각 작업의 요청부터 종료까지 걸린 시간의 평균은 9ms(= (3 + 7 + 17) / 3)가 됩니다.

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)

##### 제한 사항

- jobs의 길이는 1 이상 500 이하입니다.
- jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
- 각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
- 각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
- 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.

##### 입출력 예

| jobs                     | return |
| ------------------------ | ------ |
| [[0, 3], [1, 9], [2, 6]] | 9      |

## 풀이

작업의 요청~종료 시간의 평균이 가장 짧으려면 다음과 같은 방식으로 처리해야 한다. (대기중인 작업의 유무를 기준으로 한다는 생각을 빨리 떠올리지 못해 시간을 많이 잡아먹었다...)

- 대기중인 작업이 있을 때는 가장 짧은 작업을 수행한다.
- 대기중인 작업이 없을 때는 가장 빨리 시작하는 작업을 수행한다.

그리고 작업을 수행할 때마다 새로이 대기상태가 되는 작업이 생길 수 있으므로 매번 대기중인 작업을 갱신해야 한다.

전체 작업은 가장 앞의 원소를 pop하기 위해 데크로 만들었고, 대기중인 작업은 가장 짧은 작업을 꺼내기 위해 힙으로 만들었다. 

```python
import heapq
import collections

def solution(jobs):
    jobs_queue = collections.deque(sorted(jobs))
    now = -1
    time_sum = 0
    job_pushed = []
    
    while jobs_queue or job_pushed:
        if job_pushed:
            _, job = heapq.heappop(job_pushed)
            time_sum += now - job[0] + job[1]
            now += job[1]
        else:
            job = jobs_queue.popleft()
            now = job[0] + job[1]
            time_sum += job[1]
            
        # 대기중인 작업을 갱신
        while jobs_queue and jobs_queue[0][0] <= now:
            job = jobs_queue.popleft()
            heapq.heappush(job_pushed, (job[1], job))
        
    return int(time_sum / len(jobs))
```

