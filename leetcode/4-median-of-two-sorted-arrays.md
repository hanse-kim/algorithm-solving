## 4. Median of Two Sorted Arrays[🔗](https://leetcode.com/problems/median-of-two-sorted-arrays/)

크기 `m`과 `n`의 정렬된 배열 `nums1`과 `nums2`가 주어진다. 두 배열의 **중앙값**을 반환하라.

**주의**: 전체 실행 시간 복잡도는 `O(log (m+n))`이어야 한다.

#### Example

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000
```

```
Input: nums1 = [2], nums2 = []
Output: 2.00000
```

<br>

### 리스트를 합친 후 정렬 (80ms)

실행 속도 자체는 아래 방법보다 빠르지만, 파이썬의 정렬 알고리즘은 `O(nlog(n))`이므로 문제 조건에 부합하지 않는다.

* 로직
  * 두 배열을 합친 후 정렬한다.
  * 합친 배열의 길이가 짝수일 때, 가운데 두 숫자의 평균값을 반환한다.
  * 합친 배열의 길이가 홀수일 때, 가운데 숫자를 반환한다.

````python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        nums = sorted(nums1 + nums2)
        
        if not nums:
            return 0
        
        nums_len = len(nums)
        
        if nums_len % 2 == 0:
            return (nums[int(nums_len / 2)] + nums[int(nums_len / 2) - 1]) / 2
        
        return nums[int(nums_len / 2)]
````

<br>

### 두 리스트를 순회 (96ms)

두 배열의 숫자 중 가운데 두 숫자만 알면 되므로, 전체 길이의 절반에 다다를 때까지만 반복문을 돌린다.
시간 복잡도는 `O((m+n)/2)`가 된다.

* 로직
  * 두 배열 전체 길이의 절반에 해당하는 값을 `stop_index`에 저장한다.
  * 두 배열의 인덱스를 각각 `idx1`, `idx2`에 저장한다. 한 반복마다 각 배열에서 각 인덱스의 숫자를 비교한다.
    * 숫자가 작은 쪽을 `now_num`으로 삼고, 반복문의 마지막에서 해당 배열의 인덱스를 1 더한다.
  * 인덱스의 합이 `stop_index`에 다다랐을 때, 전체 길이가 홀수냐 짝수냐에 따라 반환을 달리 한다.
    * 홀수일 경우, `now_num`, 즉 전체 숫자 중 가운데 숫자를 반환한다.
    * 짝수일 경우, `now_num`과 `prev_num`의 평균, 즉 전체 숫자 중 가운데 두 숫자의 평균을 반환한다.
  * `prev_num`을 `now_num`으로 대체하고 다음 반복으로 넘어간다.

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        if not nums1 and not nums2:
            return 0
        
        len1 = len(nums1)
        len2 = len(nums2)
        len_total = len1 + len2
        idx1 = idx2 = prev_num = 0
        stop_index = int(len_total / 2)
        is_odd = len_total % 2 != 0
        
        while True:
            num1 = nums1[idx1] if idx1 < len1 else sys.maxsize
            num2 = nums2[idx2] if idx2 < len2 else sys.maxsize

            if num1 < num2:
                now_num = num1
            else:
                now_num = num2
            
            if idx1 + idx2 == stop_index:
                if is_odd:
                    return now_num
                else:
                    return (now_num + prev_num) / 2
            
            if num1 < num2:
                idx1 += 1
            else:
                idx2 += 1
            prev_num = now_num
```

