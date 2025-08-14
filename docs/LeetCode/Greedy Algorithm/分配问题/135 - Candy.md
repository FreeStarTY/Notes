There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return _the minimum number of candies you need to have to distribute the candies to the children_.

**Example 1:**

**Input:** ratings = [1,0,2]
**Output:** 5
**Explanation:** You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

**Example 2:**

**Input:** ratings = [1,2,2]
**Output:** 4
**Explanation:** You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.

**Constraints:**

- `n == ratings.length`
- `1 <= n <= 2 * 104`
- `0 <= ratings[i] <= 2 * 104`

## 思路
1. **初始化**：所有孩子的糖果数先设为 1。  
2. **从左向右遍历**：若右边孩子评分高于左边，则右边糖果数 = 左边糖果数 + 1。  
3. **从右向左遍历**：若左边孩子评分高于右边，且左边糖果数不大于右边，则左边糖果数 = 右边糖果数 + 1。  

### 贪心策略
将左右都需要比较的关系拆解为**分别对左边和右边的比较**：  
- 在每次遍历中，只考虑并更新一侧的大小关系。

## 代码实现
```python

class Solution:
    def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        candies = [1] * n
        
        # 从左向右遍历
        for i in range(1, n):
            if ratings[i] > ratings[i - 1]:
                candies[i] = candies[i - 1] + 1
        
        # 从右向左遍历
        for i in range(n - 1, 0, -1):
            if ratings[i] < ratings[i - 1]:
                candies[i - 1] = max(candies[i - 1], candies[i] + 1)

        return sum(candies)
````

## 复杂度分析

- **时间复杂度**：`O(n)`
    
    - 遍历 `candies` 数组 3 次（初始化 + 两次遍历）
        
- **空间复杂度**：`O(n)`
    
    - 额外使用一个长度为 `n` 的 `candies` 数组
        