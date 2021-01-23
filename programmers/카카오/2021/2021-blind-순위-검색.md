## 2020 카카오 블라인드 채용 - 순위 검색 [🔗](https://programmers.co.kr/learn/courses/30/lessons/72412)

 ### 풀이1

```python
import collections

def hit(q_info, p_info):
    for q, p in zip(q_info, p_info):
        if q != '-' and q != p:
            return False
    return True

def solution(info, query):
    score_list = []
    info_list = []
    for v in info:
        value_split = v.split()
        score_list.append(int(value_split[-1]))
        info_list.append(value_split[:-1])
    
    result = []
    for q in query:
        q_split = q.replace("and ", "").split()
        score = int(q_split[-1])
        q_info = q_split[:-1]
        higher_score_index = [ i for i, v in enumerate(score_list) if v >= score]
        count = 0
        for i in higher_score_index:
            p_info = info_list[i]
            if not hit(q_info, p_info):
                continue
            count += 1
        result.append(count)
    
    return result
```

### 풀이2

```javascript
import collections

def solution(info, query):
    lang = collections.defaultdict(set)
    field = collections.defaultdict(set)
    career = collections.defaultdict(set)
    food = collections.defaultdict(set)
    score_list = []
    
    for i, v in enumerate(info):
        value_split = v.split()
        score_list.append(int(value_split[-1]))
        lang[value_split[0]].add(i)
        field[value_split[1]].add(i)
        career[value_split[2]].add(i)
        food[value_split[3]].add(i)
        
    result = []
    for q in query:
        q_lang, q_field, q_career, q_food, q_score = q.replace(" and", "").split()
        q_score = int(q_score)
        higher_scores_index = [ i for i, v in enumerate(score_list) if v >= q_score ]
        all_set = set(higher_scores_index)
        if q_lang != '-':
            all_set = all_set & lang[q_lang]
        if q_field != '-':
            all_set = all_set & field[q_field]
        if q_career != '-':
            all_set = all_set & career[q_career]
        if q_food != '-':
            all_set = all_set & food[q_food]
        result.append(len(all_set))
    
    return result
```

