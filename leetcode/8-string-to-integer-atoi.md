## 8. String to Integer(atoi)[🔗](https://leetcode.com/problems/string-to-integer-atoi/)

문자열을 정수형으로 변환하는 `atoi` 함수를 구현한다.

이 함수는 우선 공백이 아닌 문자가 나올 때까지 공백을 제거한다. 그리고 선택적으로 + 혹은 - 부호, 그리고 이어지는 숫자 자릿수 문자들을 숫자 값으로 해석한다.

숫자 뒤에 추가적인 문자가 올 수 있는데, 이들은 무시되고 함수 수행에 아무런 영향을 주지 않는다.

만약 처음 나온 공백이 아닌 문자가 부호나 숫자가 아니거나, 문자열이 비어 있거나, 문자열에 공백만 존재할 경우 변환은 수행되지 않는다.

유효한 변환이 수행되지 않을 경우, 0을 반환한다.

##### 주의:

- 공백 문자 ` ' '`만을 공백으로 취급한다.
- 정수형은 32-bit singned integer 범위에만 저장할 수 있는 환경이라 가정한다(-2<sup>31</sup>~2<sup>31</sup>-1). 만약 이 범위를 벗어난다면 -2<sup>31</sup> 혹은 2<sup>31</sup>-1을 반환한다.

#### Example

```
Input: str = "42"
Output: 42
```

```
Input: str = "   -42"
Output: -42
```

```
Input: str = "4193 with words"
Output: 4193
```

```
Input: str = "words and 987"
Output: 0
```

```
Input: str = "-91283472332"
Output: -2147483648
```

<br>

### 입력받은 문자열을 가공 후 변환

입력받은 문자열에서 불필요한 부분을 쳐낸 후 마지막에 정수형으로 변환해서 반환한다.

* 로직
  * 공백을 제거한다.
  * 문자열 맨 앞 문자가 부호일 경우 부호를 제거하고 해당 부호를 저장해둔다.
  * 문자열 왼쪽의 0들을 제거한다.
  * 문자열을 순회하며 숫자가 아닌 문자가 나올 경우, 그 문자부터 뒷부분은 잘라낸다.
  * 가공한 문자열 길이가 최대값 길이보다 클 경우 최대값을 반환한다.
  * 가공한 문자열 길이가 최대값 길이와 동일할 경우 각 자릿수를 비교하여 문자열의 수가 더 클 경우 최대값을 반환한다.
  * 가공한 문자열를 정수형으로 변환하고 부호를 곱하여 반환한다.

````python
class Solution:
    def myAtoi(self, s: str) -> int:     
        positive_max = 2147483647
        negative_max = -2147483648
        positive_max_digits = [2, 1, 4, 7, 4, 8, 3, 6, 4, 7]
        negative_max_digits = [2, 1, 4, 7, 4, 8, 3, 6, 4, 8]
        sign = 1
        
        s = s.strip()
        if not s:
            return 0
        if s[0] == '-' or s[0] == '+':
            sign = -1 if s[0] == '-' else 1
            s = s[1:]
        
        s = s.lstrip('0')
        
        s_last_index = None
        for i, c in enumerate(s):
            if not c.isdecimal():
                s_last_index = i
                break
        
        s = s[0:s_last_index]
        if not s:
            return 0
        
        if len(s) > len(positive_max_digits):
            return positive_max if sign == 1 else negative_max
        
        if len(s) == len(positive_max_digits):
            max_digits = positive_max_digits if sign == 1 else negative_max_digits
            for c, m in zip(s, max_digits):
                n = int(c)
                if n > m:
                    return positive_max if sign == 1 else negative_max
                if n < m:
                    break
    
        return int(s) * sign
````

<br>

### 입력받은 문자열을 순회하며 결과값을 완성

문자열을 순회하며 유효한 숫자가 나올 때마다 결과값을 왼쪽으로 shift하고(10을 곱하고) 해당 숫자를 더하면서 결과값을 완성하는 방법. leetcode의 솔루션을 참고했다.

속도는 비슷하지만 보다 깔끔하고 가독성이 좋다.

* 로직
  * 공백을 제거, 부호 제거는 위와 동일하다.
  * 문자열을 순회하며 각 문자에 대해 다음을 수행한다.
    * 유효하지 않은 0(숫자의 앞에 오는 0)은 전부 continue한다.
    * 문자를 정수형 자릿수로 변환한다. 불가능할 경우 (숫자 이외의 문자를 만난 것이므로) 루프를 종료한다.
    * 다음의 경우 중 하나라도 해당되면 부호에 맞는 최대값을 반환한다.
      * 자릿수 카운트가 최대값의 길이보다 클 경우
      * 현재까지의 결과값(즉 이번 반복의 숫자가 아직 더해지지 않은 값)이 최대값을 10으로 나눈 몫보다 클 경우
      * 현재까지의 결과값이 최대값을 10으로 나눈 몫과 동일하고, 현재 자릿수가 최대값의 1의 자릿수보다 클 경우
    * 결과값을 왼쪽으로 shift하고 현재 자릿수를 더한다. 그리고 자릿수 카운트를 +1한다.
  * 결과값에 부호를 곱하여 반환한다.

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        positive_max = 2147483647
        negative_max = positive_max + 1
        max_len = 10
        sign = 1
        
        s = s.strip()
        if not s:
            return 0
        if s[0] == '+' or s[0] == '-':
            sign = -1 if s[0] == '-' else 1
            s = s[1:]
            
        result = 0
        digit_count = 0
        divide_by_10, units = divmod(positive_max if sign == 1 else negative_max, 10)
        for c in s:
            if result == 0 and c == '0':
                continue
            try:
                digit = int(c)
            except:
                break
            if digit_count >= max_len or result > divide_by_10 or \
                (result == divide_by_10 and digit >= units):
                return positive_max if sign == 1 else negative_max * -1
            
            result = result * 10 + digit
            digit_count += 1
        
        return result * sign
```

