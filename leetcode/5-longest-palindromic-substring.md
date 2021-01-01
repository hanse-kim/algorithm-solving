## 5. Longest Palindromic Substring[🔗](https://leetcode.com/problems/longest-palindromic-substring/)

문자열 `s`가 주어진다. `s`의 부분 문자열 중 가장 긴 회문을 반환하라.

#### Example

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

```
Input: s = "cbbd"
Output: "bb"
```

```
Input: s = "a"
Output: "a"
```

```
Input: s = "ac"
Output: "a"
```

<br>

### 브루트 포스 (7360ms)

이중 for문으로 모든 부분 문자열을 체크하는 방법.

* 로직
  * 모든 부분 문자열을 순회하며 회문인지 체크한다.
    * 부분 문자열이 이전 회문보다 더 긴 회문이라면 갱신한다.
  * 결과를 반환한다.

````python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) < 2 or s[::-1] == s:
            return s
        
        result = ""
        for i in range(len(s)):
            for j in range(i + 1, len(s) + 1):
                substring = s[i:j]
                if substring == substring[::-1]:
                    result = max(result, substring, key=len)
                    
        return result
````

<br>

### 회문을 양 방향으로 확장 (268ms)

문자열의 인덱스를 순회하며, 각 인덱스 위치에서 회문을 양 방향으로 확장해가며 체크하는 방법.

* 로직
  * 문자열의 인덱스를 순회하며, 각 인덱스 위치에 대해 다음 함수를 수행하고 더 긴 회문으로 갱신한다.
    * 좌측 인덱스, 우측 인덱스를 입력받는다.
    * 각 인덱스가 문자열 길이 범위 내이면서, 문자열의 각 인덱스의 문자가 동일할 경우, 좌측 인덱스는 왼쪽으로, 우측 인덱스는 오른쪽으로 이동한다.
    * while문이 끝난 경우, 즉 인덱스가 문자열 길이 범위를 벗어났거나 회문이 아니게 된 경우, 직전의 회문을 반환한다.
  * 결과를 반환한다.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # 체크하지 않아도 되는 경우는 빠르게 리턴한다
        if len(s) < 2 or s[::-1] == s:
            return s
        
        # 회문을 확장하는 함수
        def expand(left, right):
            while left >= 0 and right <= len(s) and s[left] == s[right - 1]:
                left -= 1
                right += 1
                
            return s[left + 1 : right - 1]
        
        result = ""
        for i in range(len(s)):
            result = max(
                result,
                expand(i, i+1), #홀수 회문 확장
                expand(i, i+2), #짝수 회문 확장
                key = len
            )
            
        return result
```

