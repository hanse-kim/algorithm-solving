## 2018 카카오 블라인드 채용 - 추석 트래픽 [🔗](https://programmers.co.kr/learn/courses/30/lessons/17676)

 ### 풀이

1. 각 문자열을 다중집합으로 만드는 처리를 한다.
   1. 2글자씩 순회하며 영문자가 아닌 경우를 거른다.
   2. 영문자일 경우 갯수를 카운트하고, `(현재까지 나온 갯수)(글자쌍)`과 같은 형식으로 집합에 저장한다. 이는 동일한 글자쌍을 구분하기 위함이다.
2. 처리가 끝난 두 다중집합으로부터 합집합과 교집합을 구하고 자카드 유사도를 구한다. 이때 두 집합 다 공집합일 경우 유사도는 1로 한다.
3. 유사도에 65536을 곱하고 소수점 아래는 버린 결과를 return한다.

```python
import collections

def string_to_set(string:str) -> set:
    CHAR_PAIR_FORMAT = "{0}{1}"
    duplicated_char_pairs = collections.defaultdict(int)
    result = set()
    for i in range(len(string) - 1):
        char_pair = string[i:i+2]
        if not char_pair.isalpha():
            continue
            
        duplicated_char_pairs[char_pair] += 1
        result.add(CHAR_PAIR_FORMAT.format(duplicated_char_pairs[char_pair], char_pair))
    
    return result
        
def solution(str1, str2):
    set1 = string_to_set(str1.lower())
    set2 = string_to_set(str2.lower())
    
    union = set1 | set2
    intersect = set1 & set2
    
    jaccard_coefficient = len(intersect) / len(union) if set1 or set2 else 1
    return int(jaccard_coefficient * 65536)
```

### 풀이 -  JavaScript

js에는 없는 함수들을 직접 구현한 것 이외에는 위 풀이와 동일하다.

```javascript
function is_alphabet(string) {
    let alphabet_regex = /^[A-Za-z]+$/;
    return string.match(alphabet_regex);
}

function string_to_set(string) {
    let duplicated_char_pairs = {}
    let result = [];

    for (let i = 0; i < string.length - 1; i++) {
        let char_pair = string.slice(i, i + 2)
        if (!is_alphabet(char_pair)) {
            continue
        }

        if (char_pair in duplicated_char_pairs == false) {
            duplicated_char_pairs[char_pair] = 0;
        }
        
        duplicated_char_pairs[char_pair] += 1
        result.push(duplicated_char_pairs[char_pair] + char_pair);
    }
    
    return result
}

function solution(str1, str2) {
    str1 = str1.toLowerCase();
    str2 = str2.toLowerCase();
    
    let set1 = string_to_set(str1);
    let set2 = string_to_set(str2);

    let union = {};
    let intersect = [];

    for (let i = 0; i < set1.length; i++) {
        union[set1[i]] = 1;
    }

    for (let i = 0; i < set2.length; i++) {
        if (union[set2[i]]) {
            intersect.push(set2[i]);
        }

        union[set2[i]] = 1;
    }
    
    let union_count = Object.keys(union).length;
    let intersect_count = intersect.length;

    let jaccard_coefficient = union_count !== 0 || intersect_count !== 0 ? intersect_count / union_count : 1;
    return parseInt(jaccard_coefficient * 65536);
}
```

