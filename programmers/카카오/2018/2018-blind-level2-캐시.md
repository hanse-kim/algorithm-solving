## 2018 카카오 블라인드 채용 - 캐시 [🔗](https://programmers.co.kr/learn/courses/30/lessons/17680)

지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.
이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

### 입력 형식

- 캐시 크기(`cacheSize`)와 도시이름 배열(`cities`)을 입력받는다.
- `cacheSize`는 정수이며, 범위는 0 ≦ `cacheSize` ≦ 30 이다.
- `cities`는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
- 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.

### 출력 형식

- 입력된 도시이름 배열을 순서대로 처리할 때, "총 실행시간"을 출력한다.

### 조건

- 캐시 교체 알고리즘은 `LRU`(Least Recently Used)를 사용한다.
- `cache hit`일 경우 실행시간은 `1`이다.
- `cache miss`일 경우 실행시간은 `5`이다.

---

## 풀이

처음에는 LRU를 LFU와 헷갈려서 dictionary를 이용해 풀이했다.

- LFU: 가장 적은 참조횟수를 가진 item을 교체
- LRU: 가장 오래 참조되지 않은 item을 교체

중간부터 개념을 잘못 알고 있었다는 걸 깨달았다. LRU는 굳이 dictionary를 사용하지 않아도 되지만 일단 작성한 게 아까워서 조금 수정한 후 정답을 받았다.

```python
def getLeastUsed(cache):
    minTime = 100001
    leastUsed = ''
    for key in cache:
        if cache[key] < minTime:
            minTime = cache[key]
            leastUsed = key
    
    return leastUsed

def solution(cacheSize, cities):
    if cacheSize == 0:
        return len(cities) * 5

    cache = {}
    time = 0
    for i, city in enumerate(cities):
        city = city.lower()
        if city in cache:
            cache[city] = i
            time += 1
        else:
            if len(cache) >= cacheSize:
                cache.pop(getLeastUsed(cache))
            cache[city] = i
            time += 5

    return time
```

## 풀이 2

deque를 이용해 좀 더 깔끔한 풀이가 가능하다. 다른 분의 풀이를 보면 캐시를 쌓는 순서를 역순으로 하고 deque에 maxSize를 지정해서 코드를 줄인 풀이가 있던데, 괜찮은 생각인 것 같다.

```python
import collections

def solution(cacheSize, cities):
    if cacheSize == 0:
        return len(cities) * 5

    cache = collections.deque()
    time = 0
    for city in cities:
        city = city.lower()
        if city in cache:
            cache.remove(city)
            cache.append(city)
            time += 1
        else:
            if len(cache) >= cacheSize:
                cache.popleft()
            cache.append(city)
            time += 5

    return time
```

