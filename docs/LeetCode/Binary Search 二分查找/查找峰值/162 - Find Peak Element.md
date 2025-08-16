A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**

**Input:** nums = [1,2,3,1]
**Output:** 2
**Explanation:** 3 is a peak element and your function should return the index number 2.

**Example 2:**

**Input:** nums = [1,2,1,3,5,6,4]
**Output:** 5
**Explanation:** Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.

**Constraints:**
- `1 <= nums.length <= 1000`
- `-231 <= nums[i] <= 231 - 1`
- `nums[i] != nums[i + 1]` for all valid `i`.

## 思路
分情况讨论：  
1. **Case 1**：数组只有一个元素，直接返回该元素下标  
2. **Case 2**：数组长度 ≥ 2，先判断两端：  
   - 若 `nums[0] > nums[1]`，返回 `0`  
   - 若 `nums[n-1] > nums[n-2]`，返回 `n-1`  
3. **Case 3**：若前两种情况都不满足，则峰值一定出现在中间：  
   - 使用二分查找，判断 `mid` 是否为峰值：  
     - 若 `nums[mid] > nums[mid-1] and nums[mid] > nums[mid+1]`，则 `mid` 为峰值  
   - 若不是，则根据梯度方向缩小范围：  
     - 若 `nums[mid] > nums[mid-1]`，说明右侧存在峰值 → `left = mid + 1`  
     - 否则左侧存在峰值 → `right = mid - 1`  

## 代码实现
```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        n = len(nums)

        # case 1
        if n == 1:
            return 0

        # case 2
        if nums[0] > nums[1]:
            return 0
        if nums[n - 1] > nums[n - 2]:
            return n - 1
        
        # case 3
        left, right = 1, n - 2
        while left <= right:
            mid = (left + right) // 2
            # 判断是否为峰值
            if nums[mid] > nums[mid - 1] and nums[mid] > nums[mid + 1]:
                return mid
            elif nums[mid] > nums[mid - 1]:  # 往右上升
                left = mid + 1
            else:  # 往左上升或谷底情况
                right = mid - 1

        return -1
```

## 复杂度分析

- **时间复杂度**：`O(log n)`
    
    - 使用二分查找，范围每次缩小一半
        
- **空间复杂度**：`O(1)`
    
    - 仅使用常数额外变量
        
