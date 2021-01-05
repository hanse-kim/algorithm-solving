## 9. Palindrome Number[🔗](https://leetcode.com/problems/palindrome-number/)

주어진 정수가 회문인지 판별하라.

##### 추가사항:

- 정수를 문자열로 바꾸지 않고 풀 수 있을까?

#### Example

```
Input: x = 121
Output: true
```

```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

<br>

### 숫자를 리스트로 바꾸어 회문인지 판별

* 로직
  * 음수거나 1의 자릿수가 0일 경우 False를 반환한다.
  * 그 이외에 정수가 한 자리일 경우 True를 반환한다.
  * 정수를 10으로 나누면서 각 자릿수를 리스트에 추가한다.
  * 리스트가 뒤집은 리스트와 동일한지를 반환한다.

````python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0 or (x >= 10 and x % 10 == 0):
            return False

        if x < 10:
            return True
        
        num_digits = []
        while x > 0:
            x, remainder = divmod(x, 10)
            num_digits.append(remainder)
            
        return num_digits == num_digits[::-1]
````
