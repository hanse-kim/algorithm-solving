## 3. Longest Substring Without Repeating Characters [🔗](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

문자열 `s`가 주어진다. 중복되는 문자가 없는 가장 긴 부분 문자열의 길이를 찾아라.

#### Example

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
```

```
Input: s = ""
Output: 0
```

<br>

### 큐를 이용한 방법

* 로직
  * 큐를 선언한다. (파이썬엔 큐가 없으므로 데크로 대신함)
  * 문자열의 각 문자를 순환하며 다음을 수행한다.
    * 큐에 문자를 추가한다.
    * 최대 부분 문자열 길이를 갱신한다.
    * (루프의 처음에서) 만약 문자가 이미 큐에 있다면, 해당 문자가 없어질 때까지 pop한다.
  * 최대 부분 문자열 길이를 반환한다.
* python

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        longest_length = 0
        substring_queue = collections.deque()
        
        for char in s:
            while char in substring_queue:
                substring_queue.popleft()
            
            substring_queue.append(char)
            longest_length = max(longest_length, len(substring_queue))
            
        return longest_length
```

