Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return _an array of all the integers in the range_ `[1, n]` _that do not appear in_ `nums`.

**Example 1:**

**Input:** nums = [4,3,2,7,8,2,3,1]
**Output:** [5,6]

**Example 2:**

**Input:** nums = [1,1]
**Output:** [2]

**Constraints:**

- `n == nums.length`
- `1 <= n <= 105`
- `1 <= nums[i] <= n`

## 思路
- **关键点**：利用数值范围 `[1, n]` 与索引范围 `[0, n-1]` 的映射关系  
1. **第一遍遍历**：  
   - 对于每个数 `num`，找到对应索引 `pos = abs(num) - 1`  
   - 将 `nums[pos]` 置为负数，表示该位置对应的数字 `pos + 1` 已出现过  
   - 使用 `abs()` 避免重复标记时受负号影响  
2. **第二遍遍历**：  
   - 找出仍为正数的位置 `i`，说明 `i + 1` 从未出现过  

---

## 代码实现
```python
from typing import List

class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        # 第一遍遍历：标记出现过的数字
        for num in nums:
            pos = abs(num) - 1
            if nums[pos] > 0:
                nums[pos] = -nums[pos]
        
        # 第二遍遍历：收集未出现的数字
        return [i + 1 for i in range(len(nums)) if nums[i] > 0]
```

---

## 复杂度分析

- **时间复杂度**：`O(n)`
    
    - 两次遍历数组
        
- **空间复杂度**：`O(1)`
    
    - 在原地修改数组，仅使用常数额外空间（不计输出结果）
        
