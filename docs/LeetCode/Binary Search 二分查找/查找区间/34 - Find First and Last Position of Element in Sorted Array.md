
Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [5,7,7,8,8,10], target = 8
**Output:** [3,4]

**Example 2:**

**Input:** nums = [5,7,7,8,8,10], target = 6
**Output:** [-1,-1]

**Example 3:**

**Input:** nums = [], target = 0
**Output:** [-1,-1]

**Constraints:**
- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` is a non-decreasing array.
- `-109 <= target <= 109`

## 思路
- 使用 **两次二分查找**：  
  1. **查找开始元素**（左边界）  
     - 若 `mid == left`，或者 `nums[mid-1] < target`，说明左边没有更多 `target`，此时 `mid` 即为第一个目标位置  
  2. **查找结束元素**（右边界）  
     - 若 `mid == right`，或者 `nums[mid+1] > target`，说明右边没有更多 `target`，此时 `mid` 即为最后一个目标位置  

- 核心思想：在常规二分查找的基础上，增加“是否到边界”的判断条件，从而确定左右边界。

## 代码实现
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        lower_bound = self.lowerBound(nums, target)
        if lower_bound == -1:
            return [-1, -1]
        upper_bound = self.upperBound(nums, target)
        return [lower_bound, upper_bound]
    
    def lowerBound(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                # 确定为第一个 target
                if mid == left or nums[mid - 1] < target:
                    return mid
                right = mid - 1
        return -1

    def upperBound(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                # 确定为最后一个 target
                if mid == right or nums[mid + 1] > target:
                    return mid
                left = mid + 1
        return -1
```

## 复杂度分析

- 设数组长度为 `n`
    
- **时间复杂度**：`O(log n)`
    
    - 进行两次二分查找
        
- **空间复杂度**：`O(1)`
    
    - 仅使用若干指针和变量
        
