Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return_ `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1
**Output:** tail connects to node index 1
**Explanation:** There is a cycle in the linked list, where tail connects to the second node.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0
**Output:** tail connects to node index 0
**Explanation:** There is a cycle in the linked list, where tail connects to the first node.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1
**Output:** no cycle
**Explanation:** There is no cycle in the linked list.

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

## 思路

### 方法一：Hash Set
1. 从头开始遍历，将访问过的节点放入 `hash set` 中  
2. 每走一步检查当前节点是否已在 `set` 中  
   - 如果已存在，说明找到环路起点，返回当前节点  
   - 如果走到 `null`，说明没有环路，返回 `None`  

### 方法二：Floyd 判圈法（快慢指针 / Tortoise and Hare）
1. 定义两个指针：  
   - `slow` 每次走一步  
   - `fast` 每次走两步  
2. 如果 `fast` 或 `fast.next` 为 `None`，说明无环  
3. 若存在环路，则 `slow` 和 `fast` 一定会在环上的某一点相遇  
4. 相遇时将 `fast` 移回链表头，并让 `slow`、`fast` 都每次走一步  
5. 当 `slow == fast` 时，相遇节点就是环的起点（数学可证明）

---

## 代码实现

### 方法一：快慢指针
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        is_first_cycle = True

        # 阶段一：判断是否有环
        while fast != slow or is_first_cycle:
            if fast is None or fast.next is None:
                return None
            fast = fast.next.next
            slow = slow.next
            is_first_cycle = False
        
        # 阶段二：寻找环的起点
        fast = head
        while fast != slow:
            fast = fast.next
            slow = slow.next
        return fast
```

### 方法二：Hash Set

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        node_seen = set()  # 已访问节点集合

        node = head
        while node is not None:
            if node in node_seen:  # 当前节点已出现，说明是环路起点
                return node
            node_seen.add(node)
            node = node.next
        
        return None
```

---

## 复杂度分析

### 方法一：快慢指针

- **时间复杂度**：`O(n)`
    
    - 阶段一：最多遍历 `n` 步
        
    - 阶段二：最多遍历 `n` 步
        
    - 总体复杂度：`O(n)`
        
- **空间复杂度**：`O(1)`
    
    - 仅使用 `slow` 和 `fast` 两个指针
        

### 方法二：Hash Set

- **时间复杂度**：`O(n)`
    
    - 每个节点只访问一次
        
- **空间复杂度**：`O(n)`
    
    - 最坏情况需将所有节点放入集合
        
