Given two strings `s` and `t` of lengths `m` and `n` respectively, return _the **minimum window**_ **_substring_** _of_ `s` _such that every character in_ `t` _(**including duplicates**) is included in the window_. If there is no such substring, return _the empty string_ `""`.

The testcases will be generated such that the answer is **unique**.

**Example 1:**

**Input:** s = "ADOBECODEBANC", t = "ABC"
**Output:** "BANC"
**Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

**Example 2:**

**Input:** s = "a", t = "a"
**Output:** "a"
**Explanation:** The entire string s is the minimum window.

**Example 3:**

**Input:** s = "a", t = "aa"
**Output:** ""
**Explanation:** Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

**Constraints:**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` and `t` consist of uppercase and lowercase English letters.

## 思路
- **方法**：滑动窗口 + 双指针  
- **关键变量**：
  - `freq`：使用 `Counter` 统计 `t` 中字符出现次数  
  - `count`：当前窗口内包含的 `t` 中字符个数  
  - `l`、`r`：左右指针  
  - `min_l`、`min_length`：记录最短覆盖子串的左边界和长度  
- **算法流程**：
  1. 移动右指针 `r`，将字符计入窗口  
  2. 若窗口已包含 `t` 中所有字符，则开始右移左指针 `l`  
  3. 在缩小窗口时，比较当前窗口是否比已有最短窗口更优，并更新结果  
  4. 若窗口不再满足覆盖条件，停止缩小，继续右移 `r`

## 代码实现
```python
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        freq = Counter(t)  # 统计 t 中的字符情况
        count = 0  # 当前窗口包含的 t 中字符个数
        min_l, min_length = None, None  # 最小窗口的起始索引与长度
        l = 0

        for r in range(len(s)):
            if s[r] not in freq:  # r 位置不含目标字符
                continue

            # r 位置包含目标字符
            freq[s[r]] -= 1
            if freq[s[r]] >= 0:  # 补齐一个缺失字符
                count += 1

            # 当窗口已包含 t 的全部字符，尝试右移 l 缩小窗口
            while count == len(t):
                # 更新最短窗口
                if min_length is None or r - l + 1 < min_length:
                    min_l = l
                    min_length = r - l + 1

                # 移除 l 位置字符，检查是否仍满足条件
                if s[l] in freq:
                    freq[s[l]] += 1
                    if freq[s[l]] > 0:
                        count -= 1
                l += 1

        return "" if min_length is None else s[min_l: min_l + min_length]
```

## 复杂度分析

- **时间复杂度**：
    
    - 构建 `Counter(t)`：`O(|t|)`
        
    - 双指针遍历 `s`：`O(|s|)`
        
    - **总时间复杂度**：`O(|s| + |t|)`
        
- **空间复杂度**：
    
    - 最坏情况下，窗口大小可达 `O(|s|)`
        
    - `t` 中字符集合存储需要 `O(|t|)`
        
    - **总空间复杂度**：`O(|s| + |t|)`
        
