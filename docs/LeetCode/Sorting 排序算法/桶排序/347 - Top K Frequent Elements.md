Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,1,1,2,2,3], k = 2
**Output:** [1,2]

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `k` is in the range `[1, the number of unique elements in the array]`.
- It is **guaranteed** that the answer is **unique**.

## 思路
1. **统计频次**：  
   使用哈希表（`Counter`）统计每个数出现的次数，相当于**第一次桶排序**。  
2. **按频次分桶**：  
   将“频次”作为桶的下标，桶中存放出现该频次的元素，相当于**第二次桶排序**。  
3. **从高频到低频收集结果**：  
   从可能的最大频次 `len(nums)` 开始向下遍历，直到收集到前 `k` 个元素为止。

---

## 代码实现
```python
from collections import Counter
from typing import List

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # 第一次桶排序：统计频次
        counts = Counter(nums)

        # 第二次桶排序：按频次分桶
        buckets = dict()
        for num, count in counts.items():
            if count in buckets:
                buckets[count].append(num)
            else:
                buckets[count] = [num]

        # 从高到低遍历，收集前 k 个元素
        top_k = []
        for count in range(len(nums), 0, -1):
            if count in buckets:
                top_k += buckets[count]
                if len(top_k) >= k:
                    return top_k[:k]
        return top_k[:k]
````

---

## 复杂度分析

- **时间复杂度**：`O(n)`
    
    - 统计频次：`O(n)`
        
    - 建桶：`O(u)`，其中 `u` 为不同元素个数（`u ≤ n`）
        
    - 遍历收集：均摊 `O(n)`
        
    - 总体：`O(n)`
        
- **空间复杂度**：`O(n)`
    
    - 哈希表 + 桶数组所需空间
    