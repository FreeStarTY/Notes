Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping_.

**Note** that intervals which only touch at a point are **non-overlapping**. For example, `[1, 2]` and `[2, 3]` are non-overlapping.

**Example 1:**

**Input:** intervals = [[1,2],[2,3],[3,4],[1,3]]
**Output:** 1
**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.

**Example 2:**

**Input:** intervals = [[1,2],[1,2],[1,2]]
**Output:** 2
**Explanation:** You need to remove two [1,2] to make the rest of the intervals non-overlapping.

**Example 3:**

**Input:** intervals = [[1,2],[2,3]]
**Output:** 0
**Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.

## 思路
- **问题转化**：最少的移除区间个数 ⟺ 尽量多保留不重叠的区间  
- **关键观察**：选择的区间结尾越小，留给其它区间的空间越大，能保留的区间就越多。

### 贪心策略
优先保留**结尾小**且**不相交**的区间。

## 代码实现
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        # 按照结束点从小到大排序
        intervals.sort(key=lambda x: x[1])
        
        ans = 0   # 返回值：移除的区间个数
        k = -inf  # 当前的结束点

        for x, y in intervals:
            if x >= k:  # 不重叠，保留该区间
                k = y
            else:       # 重叠，移除该区间
                ans += 1
        
        return ans
````

## 复杂度分析

- **时间复杂度**：
    
    - 排序：`O(n log n)`
        
    - 遍历：`O(n)`
        
    - **总时间复杂度**：`O(n log n)`
        
- **空间复杂度**：
    
    - Python `sort()` 使用 **Timsort**，最坏情况额外空间 `O(n)`
        
