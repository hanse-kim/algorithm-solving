## 7. Reverse Integer[🔗](https://leetcode.com/problems/reverse-integer/)

32비트 정수가 주어졌을 때, 이를 뒤집은 정수를 반환하라.

32비트 정수 범위 [−2<sup>31</sup>, 2<sup>31</sup> − 1]를 벗어나면 0을 반환한다.

#### Example

```
Input: x = 123
Output: 321
```

```
Input: x = -123
Output: -321
```

```
Input: x = 120
Output: 21
```

<br>

### 숫자를 문자열로 바꾸어 뒤집는 방법

입력받은 정수를 문자열로 바뀌 뒤집은 후 최대값을 넘었는지 체크한다.

* 로직
  * 자릿수별로 비교하기 편하도록 최대값을 배열로 선언해둔다.
  * 입력받은 정수의 부호를 체크하고 음수라면 임시로 양수로 만들어둔다.
  * 정수를 문자열로 바꾸고 뒤집는다.
  * 만약 문자열로 만든 정수가 최대값과 길이가 같다면 비교를 시작한다.
    * 정수의 부호에 따라 최대값을 선택한다.
    * 각 자릿수별로 값을 비교하면서 뒤집은 정수 쪽이 클 경우 0을 반환하고, 최대값 쪽이 클 경우 비교를 종료한다.
      * 모든 자릿수가 동일하다면 마지막 자릿수까지 비교 후 for문이 종료된다.
  * 뒤집은 정수 문자열을 정수형으로 바꾼 후 입력받은 정수가 음수였다면 다시 음수로 만들어 반환한다.

````python
class Solution:
    def reverse(self, x: int) -> int:
        max_value_positive = [2, 1, 4, 7, 4, 8, 3, 6, 4 ,7]
        max_value_negative = [2, 1, 4, 7, 4, 8, 3, 6, 4 ,8]
        is_negative = -1 if x < 0 else 1
        x *= is_negative
        
        reverse_num_str = str(x)[::-1]
        
        if len(reverse_num_str) == len(max_value_positive):
            max_value = max_value_positive if is_negative == 1 else max_value_negative
            for reverse_num_digit_str, max_value_digit in zip(reverse_num_str, max_value):
                reverse_num_digit = int(reverse_num_digit_str)
                if reverse_num_digit > max_value_digit:
                    return 0
                if reverse_num_digit < max_value_digit:
                    break
        
        return int(reverse_num_str) * is_negative
````

<br>

