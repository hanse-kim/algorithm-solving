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

---

## 풀이 - JavaScript

Map 객체도 사용해보고 오브젝트도 사용해봤는데, Map의 속도가 2배 가량 빨랐다.

어차피 타입스크립트에선 오브젝트를 해시맵처럼 사용하지도 못하니 익숙해지도록 Map을 계속 사용해야겠다. 

```javascript
// Map 사용

const solution = (clothes) => {
  const clothesCount = new Map();
  for (const cloth of clothes) {
    if (!clothesCount.has(cloth[1])) {
      clothesCount.set(cloth[1], 1);
    }

    clothesCount.set(cloth[1], clothesCount.get(cloth[1]) + 1);
  }

  let answer = 1;
  for (const v of clothesCount.values()) {
    answer *= v;
  }

  return answer - 1;
};
```

```javascript
// Object 사용

const solution = (clothes) => {
  const clothesCount = {};
  for (const cloth of clothes) {
    if (!(cloth[1] in clothesCount)) {
      clothesCount[cloth[1]] = 1;
    }

    clothesCount[cloth[1]] += 1;
  }

  let answer = 1;
  for (const [k, v] of Object.entries(clothesCount)) {
    answer *= v;
  }

  return answer - 1;
};
```
