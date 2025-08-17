Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

**Input:** matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid.jpg)

**Input:** matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
**Output:** false

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-109 <= matrix[i][j] <= 109`
- All the integers in each row are **sorted** in ascending order.
- All the integers in each column are **sorted** in ascending order.
- `-109 <= target <= 109`

## 思路
- 可以从 **右上角** 或 **左下角** 开始查找（以右上角为例）：  
  - 若当前值 **大于** 目标值 → 向左移动一位  
  - 若当前值 **小于** 目标值 → 向下移动一位  
- 若移动到左下角仍未找到目标值，则说明矩阵中不存在该值  
- 每次移动都能排除一整行或一整列，因此不会遗漏答案  

---

## 代码实现
```python
from typing import List

class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        i, j = 0, n - 1  # 从右上角开始

        while i < m and j >= 0:
            if matrix[i][j] == target:
                return True
            elif matrix[i][j] < target:
                i += 1  # 下移
            else:
                j -= 1  # 左移
        
        return False
```

---

## 复杂度分析

- **时间复杂度**：`O(m + n)`
    
    - 每次操作会移除一行或一列，最多进行 `m + n` 次移动
        
- **空间复杂度**：`O(1)`
    
    - 仅使用两个指针 `i` 和 `j`，额外空间恒定
        
