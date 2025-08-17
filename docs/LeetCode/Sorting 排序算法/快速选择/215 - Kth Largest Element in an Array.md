Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

**Example 1:**

**Input:** nums = [3,2,1,5,6,4], k = 2
**Output:** 5

**Example 2:**

**Input:** nums = [3,2,3,1,2,4,5,5,6], k = 4
**Output:** 4

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

## 思路
- **算法**：Quickselect（Hoare's Selection Algorithm）  
- **用途**：在无序数组中查找第 $k^{th}$ 大（或最小）元素  
- **核心步骤**：
  1. 随机选择一个 `pivot`  
  2. 将数组分成三部分：  
     - `left`：大于 `pivot` 的元素  
     - `mid`：等于 `pivot` 的元素  
     - `right`：小于 `pivot` 的元素  
  3. 判断第 `k` 大元素在哪一部分：  
     - 若 `k <= len(left)` → 答案在 `left`，递归查找  
     - 若 `k > len(left) + len(mid)` → 答案在 `right`，递归查找第 `k - len(left) - len(mid)` 大元素  
     - 否则 → `pivot` 即为答案  

- **注意**：  
  - 常用于查找第 $k^{th}$ **最小** 元素时，`left` 表示小于 `pivot` 的元素；  
  - 查找第 $k^{th}$ **最大** 元素时，需交换 `left` 与 `right` 的定义。  

## 代码实现
```python
import random
from typing import List

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def quick_select(nums, k):
            pivot = random.choice(nums)  # 随机选择 pivot
            left, mid, right = [], [], []

            # 划分数组
            for num in nums:
                if num > pivot:
                    left.append(num)
                elif num < pivot:
                    right.append(num)
                else:
                    mid.append(num)

            if k <= len(left):  # 在左边
                return quick_select(left, k)
            if k > len(left) + len(mid):  # 在右边
                return quick_select(right, k - len(left) - len(mid))
            return pivot  # 在中间，即 pivot

        return quick_select(nums, k)
```

## 复杂度分析

- 设数组长度为 `n`
    
- **时间复杂度**：
    
    - 平均：$O(n)$
        
    - 最坏：$O(n^2)$（每次 pivot 都选到极端位置，划分极不均匀）
        
- **空间复杂度**：`O(n)`
    
    - 由于使用 `left`、`mid`、`right` 三个额外数组存储分区
        