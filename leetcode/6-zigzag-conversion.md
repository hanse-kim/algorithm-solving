## 6. ZigZag Conversion[🔗](https://leetcode.com/problems/zigzag-conversion/)

예를 들어 문자열 `"PAYPALISHIRING"`가 주어졌을 때, 이는 주어진 행수에 맞춰 지그재그로 쓰여진 것이다.

```
P       A       H       N
  A   P   L   S   I   I   G
    Y       I       R
```

각 줄별로 읽으면 `"PAHNAPLSIIGYIR"`이 된다.

이와 같이 지그재그 문자열과 행 수가 주어졌을 때 문자열을 변환하는 코드를 작성하라.

#### Example

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P           I          N
  A       L   S      I   G
    Y   A       H   R
      P           I
```

```
Input: s = "A", numRows = 1
Output: "A"
```

##### 제약사항

* `1 <= s.length <= 1000`
* `s` 는 영문 대소문자와  `','`, `'.'`로 이루어져 있다.
* `1 <= numRows <= 1000`

<br>

### 지그재그 배열을 완성하는 방법

실제 지그재그 순서대로 포인터를 이동해가며 각 행별로 지그재그 문자 배열을 완성시키고, 마지막에 합쳐서 반환하는 방법.

* 로직
  - 입력받은 행수가 1이면 바로 반환한다.
  - 입력받은 행수만큼 이중 배열을 만든다.
  - 문자열의 각 문자를 순회하며 다음을 수행한다.
    - 이중 배열의 현재 행에 문자를 추가한다.
    - 현재 진행방향으로 행을 1회 이동한다.
    - 만약 행이 입력받은 행 범위를 벗어났을 경우:
      - 진행방향을 반대로 한다.
      - (이미 한 행만큼 벗어났으므로) 반대로 바꾼 진행방향으로 2회 이동한다.
  - 이중 배열을 한 문자열로 합쳐 반환한다.

````python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1:
            return s
        
        zigzag_array = [list() for _ in range(numRows)]
        
        row = 0
        direction = 1
        
        for char in s:
            zigzag_array[row].append(char)
            
            row += direction
            
            if row >= numRows or row < 0:
                direction = -direction
                row += 2 * direction
        
        result = "".join(itertools.chain(*zigzag_array))
        
        return result
````

<br>

