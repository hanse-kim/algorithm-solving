# 해시 - 베스트앨범 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42579)

## 풀이

결과값을 구하기 위한 규칙은 다음과 같다.

1. 재생 횟수가 많은 장르 순으로 수록한다.
2. 장르 내에서는 재생횟수에 대해 내림차순, 고유 번호에 대해 오름차순으로 2곡 까지 수록한다.

따라서 장르별 재생횟수를 구해야 하고(1), 장르별로 각 노래에 대한 재생횟수와 고유번호를 저장해두어야 한다(2).

그러고 나서 재생횟수가 많은 순으로 장르를 정렬하고, 각 장르에 대해 가장 재생횟수가 많고 고유번호가 빠른 곡을 최대 2곡까지 결과값에 담는다.

장르별로 2곡씩 뽑을 때 재생횟수에 대해 내림차순 정렬을 해야 하는데, 고유번호에 대해서는 오름차순 정렬을 해야 하므로 이 부분은 따로 람다함수를 키로 사용하여 처리했다.

```python
import collections

def solution(genres, plays):
    album = collections.defaultdict(list)
    plays_per_genres = collections.defaultdict(int)
    n = len(genres)
    
    for i in range(n):
        g = genres[i]
        p = plays[i]
        
        plays_per_genres[g] += p
        album[g].append((p, i))

    plays_per_genres_items = sorted(plays_per_genres.items(), key=lambda x: x[1], reverse=True)
    most_played_genres = [ k for k, v in plays_per_genres_items ]
    
    result = []
    
    for g in most_played_genres:
        album[g].sort(key=lambda x: (x[0], n - x[1]), reverse=True)
        result += [ i for p, i in album[g][:2] ]
    
    return result
```

