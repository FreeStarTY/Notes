
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child `i` has a greed factor `g[i]`, which is the minimum size of a cookie that the child will be content with; and each cookie `j` has a size `s[j]`. If `s[j] >= g[i]`, we can assign the cookie `j` to the child `i`, and the child `i` will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Example 1:**

**Input:** g = [1,2,3], s = [1,1]
**Output:** 1
**Explanation:** You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.

**Example 2:**

**Input:** g = [1,2], s = [1,2,3]
**Output:** 2
**Explanation:** You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.
## 思路
**贪心策略**：  
给剩余孩子中**最小饥饿度**的孩子分配**最小的能使其饱腹的饼干**。

## 代码实现
```python

class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        # 先排序
        g.sort()
        s.sort()
        
        g_i, s_i = 0, 0
        g_n, s_n = len(g), len(s)

        # 双指针遍历实现贪心算法
        while g_i < g_n and s_i < s_n:
            if g[g_i] <= s[s_i]:
                g_i += 1
            s_i += 1

        return g_i
````

## Complexity Analysis

### 设定

- g 的长度为 **n**
    
- s 的长度为 **m**
    

### 时间复杂度

- **排序部分**：`O(n log n + m log m)`
    
- **双指针遍历部分**：`O(n + m)`
    
- **总时间复杂度**：  
    由于 `O(n + m)` 数量级小于排序的 `O(n log n + m log m)`，因此总复杂度为：
    O(n log n + m log m)

    

### 空间复杂度

- Python 的 `sort()` 使用 **Timsort**，需要额外 `O(n + m)` 空间
    
- 双指针部分只用常数空间 `O(1)`
    