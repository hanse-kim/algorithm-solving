### Leetcode 1. Two Sum

정수 배열 `nums`와 정수 `target`이 주어질 때, 두 수의 합이 `target`이 되도록 하는 `nums`의 숫자의 인덱스들을 배열로 반환한다. 

하나의 정답만 존재하며 같은 수를 여러 번 사용할 수 없다.

#### Example

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

<br>

### Map을 이용한 풀이

* 로직
  * 맵을 선언한다.
  * nums를 순회하며 다음을 수행한다.
    * `nums`의 각 숫자마다, `target`에서 해당 숫자를 뺀 값과 해당 숫자의 index를 맵에 추가한다.
    * 만약 `nums` 의 숫자가 맵의 key값에 존재할 경우, 해당 key의 value와 숫자의 index를 반환한다.

* python3

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = {}
        for i, num in enumerate(nums):
            if num in dic.keys():
                return [dic[num], i]
            
            dic[target - num] = i
```

* java

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            if(map.containsKey(nums[i])){
                return new int[]{map.get(nums[i]), i};
            }
            
            map.put(target - nums[i], i);
        }
        
        throw new IllegalArgumentException();
    }
}
```

