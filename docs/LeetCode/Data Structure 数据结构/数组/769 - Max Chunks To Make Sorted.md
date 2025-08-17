You are given an integer array `arr` of length `n` that represents a permutation of the integers in the range `[0, n - 1]`.

We split `arr` into some number of **chunks** (i.e., partitions), and individually sort each chunk. After concatenating them, the result should equal the sorted array.

Return _the largest number of chunks we can make to sort the array_.

**Example 1:**

**Input:** arr = [4,3,2,1,0]
**Output:** 1
**Explanation:**
Splitting into two or more chunks will not return the required result.
For example, splitting into [4, 3], [2, 1, 0] will result in [3, 4, 0, 1, 2], which isn't sorted.

**Example 2:**

**Input:** arr = [1,0,2,3,4]
**Output:** 4
**Explanation:**
We can split into two chunks, such as [1, 0], [2, 3, 4].
However, splitting into [1, 0], [2], [3], [4] is the highest number of chunks possible.

**Constraints:**
- `n == arr.length`
- `1 <= n <= 10`
- `0 <= arr[i] < n`
- All the elements of `arr` are **unique**.

## 思路
- **关键观察**：当且仅当 **每个段中的所有数字都严格大于前一个段中的所有数字** 时，拆分才是有效的  
  - 等价于：每个段的 **最小值** 必须大于前一个段的 **最大值**  

- **算法设计**：  
  - 从左向右遍历，记录当前的最大值  
  - 当 **当前最大值 == 当前下标 index** 时，可以进行一次分割  

- **情况分析**：  
  1. **当前最大值 > index**  
     - 说明右边一定有小于当前区间元素的数字（被最大值“覆盖”），不能在此处分割  
  2. **当前最大值 == index**  
     - 表示当前区间内的数字恰好就是 `[0..index]` 这些数，只是顺序不同，可以分割  
  3. **当前最大值 < index**  
     - 不可能出现，因为每个数不同，小的数字数量不足，因此不会发生  

---

## 代码实现
```python
from typing import List

class Solution:
    def maxChunksToSorted(self, arr: List[int]) -> int:
        chunks, max_val = 0, 0
        for i in range(len(arr)):
            # 更新区间最大值
            if arr[i] > max_val:
                max_val = arr[i]
            # 判断是否可以分块
            if max_val == i:
                chunks += 1
        return chunks
```

---

## 复杂度分析

- **时间复杂度**：`O(n)`
    
    - 遍历数组一次
        
- **空间复杂度**：`O(1)`
    
    - 仅使用 `chunks` 和 `max_val` 两个变量
        