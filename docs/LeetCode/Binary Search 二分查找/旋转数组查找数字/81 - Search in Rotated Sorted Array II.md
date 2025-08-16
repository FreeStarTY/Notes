There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).

Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` **after** the rotation and an integer `target`, return `true` _if_ `target` _is in_ `nums`_, or_ `false` _if it is not in_ `nums`_._

You must decrease the overall operation steps as much as possible.

**Example 1:**

**Input:** nums = [2,5,6,0,0,1,2], target = 0
**Output:** true

**Example 2:**

**Input:** nums = [2,5,6,0,0,1,2], target = 3
**Output:** false

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- `nums` is guaranteed to be rotated at some pivot.
- `-104 <= target <= 104`

## 思路
- 使用 **二分查找**，但需先确定哪一边是递增区间：  
  - 若 `nums[mid] < nums[right]` → 右边区间递增  
  - 若 `nums[mid] > nums[left]` → 左边区间递增  
- 判断目标值 `target` 是否落在递增区间内：  
  - 若在递增区间内，则在此区间继续二分  
  - 若不在，则在另一半区间继续查找  
- 特殊情况：  
  - 当 `nums[mid] == nums[left]` 或 `nums[mid] == nums[right]` 时，无法确定递增区间 → 移动 `left` 或 `right` 跳过重复元素  

## 代码实现
```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return True

            # 避开无法判断递增空间的情况
            if nums[mid] == nums[left]:
                left += 1
            elif nums[mid] == nums[right]:
                right -= 1
            # 左区间递增
            elif nums[mid] > nums[left]:
                if nums[left] <= target <= nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            # 右区间递增
            else:
                if nums[mid] <= target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return False
```

## 复杂度分析

- **时间复杂度**：
    
    - 最坏情况：`O(n)`
        
        - 当所有元素都相同而目标不在数组中时，需要线性扫描
            
    - 最好情况：`O(log n)`
        
        - 当不存在重复元素时，与普通二分查找一致
            
- **空间复杂度**：`O(1)`
    
    - 仅使用常数个指针变量
        
