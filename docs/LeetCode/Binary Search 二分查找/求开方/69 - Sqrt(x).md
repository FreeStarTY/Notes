Given a non-negative integer `x`, return _the square root of_ `x` _rounded down to the nearest integer_. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

**Example 1:**

**Input:** x = 4
**Output:** 2
**Explanation:** The square root of 4 is 2, so we return 2.

**Example 2:**

**Input:** x = 8
**Output:** 2
**Explanation:** The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.

**Constraints:**

- `0 <= x <= 231 - 1`

## 思路
- 使用 **二分查找** 在 `[1, x]` 范围内寻找平方根  
- 关键点：  
  - 当 `mid * mid == x` 时直接返回  
  - 当 `mid * mid < x` 时平方根在右半区间，移动 `left = mid + 1`  
  - 当 `mid * mid > x` 时平方根在左半区间，移动 `right = mid - 1`  
- 边界情况需考虑：  
  - `x = 0` 或较小值时，直接返回结果  

## 代码实现
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x == 0:
            return 0

        left, right = 1, x
        while left <= right:
            mid = (left + right) // 2
            if mid * mid == x:
                return mid
            elif mid * mid < x:
                left = mid + 1
            else:
                right = mid - 1
        return right
```

## 复杂度分析

- **时间复杂度**：`O(log x)`
    
    - 每次二分范围缩小一半
        
- **空间复杂度**：`O(1)`
    
    - 仅使用 `left`、`right`、`mid` 等常数额外空间
    