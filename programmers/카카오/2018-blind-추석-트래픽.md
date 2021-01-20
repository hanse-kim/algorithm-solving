## 2018 카카오 블라인드 채용 - 추석 트래픽 [🔗](https://programmers.co.kr/learn/courses/30/lessons/17676)

 ### 풀이

1. 주어진 각 로그를 트래픽의 (시작시간, 끝시간) 튜플의 리스트로 만든다. (이때 시작시간을 포함하는 것을 고려한다)
2. 각 로그가 끝나는 시간부터 1초간 해당 로그를 포함한 이후의 로그들을 체크하여 1초간에 포함되거나 걸쳐 있는 로그들을 카운팅한다.
3. 최대 트래픽을 구하여 return한다.

```python
import datetime

def solution(lines):
    MINIMUM_TIME_DELTA = datetime.timedelta(seconds=0.001)
    ONE_SECOND = datetime.timedelta(seconds=0.999)
    TIME_FORMAT = "%Y-%m-%d %H:%M:%S.%f"
    times = []
    
    for log in lines:
        log_split = log.split()
        time = datetime.datetime.strptime(" ".join(log_split[:2]), TIME_FORMAT)
        delta = datetime.timedelta(seconds=float(log_split[2][:-1]))
        times.append((time - delta + MINIMUM_TIME_DELTA, time))
    
    max_trafic = 0
    for i, time in enumerate(times):
        start_check = time[1]
        end_check = start_check + ONE_SECOND
        trafic = 0
        for start_time, end_time in times[i:]:
            if (start_time <= start_check and end_time >= start_check) or \
                (start_time >= start_check and end_time <= end_check) or \
                (start_time <= end_check and end_time >= end_check):
                trafic += 1
        max_trafic = max(trafic, max_trafic)
    
    return max_trafic
```

### 풀이 2

프로그래머스에서 가장 많은 추천을 받은 풀이.

나도 처음에는 이런 식으로 풀려고 하다가 "1초간" 최대 트래픽을 어떻게 구해야 할지 몰라서 못했었는데, 이 풀이에서는 로그가 끝나는 시간을 +1초 함으로써 그 부분을 해결했다.

또 datetime을 쓰지 않고 각 시각을 초로 환산하여 저장한 것이 인상적이다.

1. 각 로그로부터 트래픽이 시작하는 시간과 끝나는 시간을 분리하여 각각 리스트에 저장한다. 이때, 끝나는 시간에 1초 추가함으로써 실제로 트래픽이 없어진 후에도 1초간 카운팅이 가능하도록 만든다.
2. 트래픽이 발생할 때 traffic을 +1하고 트래픽이 사라질 때 traffic을 -1한다. 시작시간을 기준으로 하기 위해 start_times를 오름차순 정렬한다. (end_times는 종료시간을 기준으로 정렬되어 있기 때문에 필요 없다)
   * 이 방식이 가능한 이유는 단순히 트래픽의 갯수만 따질 뿐 각각의 트래픽을 구분하지 않기 때문이다. 즉, 두 개의 트래픽 (1s, 4s), (2s, 3s)이 발생하는 경우와 (1s, 3s), (2s, 4s)이 발생하는 경우는 완전히 동일하다.

```python
def solution(lines):
    SECOND_INTERVAL = 1
    MIN_TIME_DELTA = 0.001
    
    start_times, end_times = [], []
    lines_length = len(lines)
    
    for line in lines:
        days, time, delta = line.split()
        hours, minutes, seconds = time.split(":")
        
        seconds = float(hours) * 3600 + float(minutes) * 60 + float(seconds)
        delta = float(delta[:-1])
        
        start_times.append(seconds - delta + MIN_TIME_DELTA)
        end_times.append(seconds + SECOND_INTERVAL)
    
    start_times.sort()
    traffic = 0
    max_traffic = 0
    start_time_index, end_time_index = 0, 0
    
    while start_time_index < lines_length and end_time_index < lines_length:
        if start_times[start_time_index] < end_times[end_time_index]:
            traffic += 1
            max_traffic = max(traffic, max_traffic)
            start_time_index += 1
        else:
            traffic -= 1
            end_time_index += 1
    
    return max_traffic
```

### 풀이 2 -  JavaScript

완전히 동일한 로직으로 문법만 고쳤는데 틀리는 테스트 케이스가 있다.  뭐지???

```javascript
function solution(lines) {
    const SECOND_INTERVAL = 1;
    const MIN_TIME_DELTA = 0.001;
    const SPACE = " ";
    const TIME_SEPARATOR = ":";

    let start_times = [];
    let end_times = [];
    let lines_length = lines.length;
    
    for (let line of lines) {
        let line_split = line.split(SPACE);
        let time_split = line_split[1].split(TIME_SEPARATOR);

        let seconds = Number(time_split[0]) * 3600 +
            Number(time_split[1]) * 60 +
            Number(time_split[2]);
        let delta = Number(line_split[2].slice(0, -1));

        start_times.push(seconds - delta + MIN_TIME_DELTA);
        end_times.push(seconds + SECOND_INTERVAL);
    }

    start_times.sort();
    let traffic = 0;
    let max_traffic = 0;
    let start_time_index = 0;
    let end_time_index = 0;

    while (start_time_index < lines_length && end_time_index < lines_length) {
        if (start_times[start_time_index] < end_times[end_time_index]) {
            traffic += 1;
            max_traffic = Math.max(traffic, max_traffic);
            start_time_index += 1;
        } else {
            traffic -= 1;
            end_time_index += 1;    
        }
    }

    return max_traffic;
}
```

