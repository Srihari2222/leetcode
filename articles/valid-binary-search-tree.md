## 1. Brute Force

::tabs-start

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    left_check = staticmethod(lambda val, limit: val < limit) 
    right_check = staticmethod(lambda val, limit: val > limit) 

    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        
        if (not self.isValid(root.left, root.val, self.left_check) or
            not self.isValid(root.right, root.val, self.right_check)):
            return False
        
        return self.isValidBST(root.left) and self.isValidBST(root.right)

    def isValid(self, root: Optional[TreeNode], limit: int, check) -> bool:
        if not root:
            return True
        if not check(root.val, limit):
            return False
        return (self.isValid(root.left, limit, check) and
                self.isValid(root.right, limit, check))
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

public class Solution {
    static boolean left_check(int val, int limit) {
        return val < limit; 
    }

    static boolean right_check(int val, int limit) {
        return val > limit; 
    }

    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (!isValid(root.left, root.val, Solution::left_check) || 
            !isValid(root.right, root.val, Solution::right_check)) {
            return false;
        }

        return isValidBST(root.left) && isValidBST(root.right);
    }

    public boolean isValid(TreeNode root, int limit, CheckFunction check) {
        if (root == null) {
            return true;
        }
        if (!check.apply(root.val, limit)) {
            return false;
        }
        return isValid(root.left, limit, check) && 
               isValid(root.right, limit, check);
    }
    
    interface CheckFunction {
        boolean apply(int val, int limit);
    }
}
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    static bool left_check(int val, int limit) {
        return val < limit;
    }

    static bool right_check(int val, int limit) {
        return val > limit;
    }

    bool isValidBST(TreeNode* root) {
        if (!root) {
            return true;
        }

        if (!isValid(root->left, root->val, left_check) || 
            !isValid(root->right, root->val, right_check)) {
            return false;
        }

        return isValidBST(root->left) && isValidBST(root->right);
    }

    bool isValid(TreeNode* root, int limit, bool (*check)(int, int)) {
        if (!root) {
            return true;
        }
        if (!check(root->val, limit)) {
            return false;
        }
        return isValid(root->left, limit, check) && 
               isValid(root->right, limit, check);
    }
};
```

```javascript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     constructor(val = 0, left = null, right = null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
    /**
     * @param {number} val
     * @param {number} limit
     * @returns {boolean}
     */
    left_check(val, limit) {
        return val < limit;
    }

    /**
     * @param {number} val
     * @param {number} limit
     * @returns {boolean}
     */
    right_check(val, limit) {
        return val > limit;
    }

    /**
     * @param {TreeNode} root
     * @returns {boolean}
     */
    isValidBST(root) {
        if (!root) {
            return true;
        }

        if (!this.isValid(root.left, root.val, this.left_check) ||
            !this.isValid(root.right, root.val, this.right_check)) {
            return false;
        }

        return this.isValidBST(root.left) && this.isValidBST(root.right);
    }

    /**
     * @param {TreeNode} root
     * @param {number} limit
     * @param {function} check
     * @returns {boolean}
     */
    isValid(root, limit, check) {
        if (!root) {
            return true;
        }
        if (!check.call(this, root.val, limit)) {
            return false;
        }
        return this.isValid(root.left, limit, check) &&
               this.isValid(root.right, limit, check);
    }
}
```

```csharp
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

public class Solution {
    static bool LeftCheck(int val, int limit) {
        return val < limit; 
    }

    static bool RightCheck(int val, int limit) {
        return val > limit; 
    }

    public bool IsValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (!IsValid(root.left, root.val, LeftCheck) || 
            !IsValid(root.right, root.val, RightCheck)) {
            return false;
        }

        return IsValidBST(root.left) && IsValidBST(root.right);
    }

    public bool IsValid(TreeNode root, int limit, Func<int, int, bool> check) {
        if (root == null) {
            return true;
        }
        if (!check(root.val, limit)) {
            return false;
        }
        return IsValid(root.left, limit, check) && 
               IsValid(root.right, limit, check);
    }
}
```

::tabs-end

### Time & Space Complexity

* Time complexity: $O(n ^ 2)$
* Space complexity: $O(n)$

---

## 2. Depth First Search

::tabs-start

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def valid(node, left, right):
            if not node:
                return True
            if not (left < node.val < right):
                return False

            return valid(node.left, left, node.val) and valid(
                node.right, node.val, right
            )

        return valid(root, float("-inf"), float("inf"))
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

public class Solution {
    public boolean isValidBST(TreeNode root) {
        return valid(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean valid(TreeNode node, long left, long right) {
        if (node == null) {
            return true;
        }
        if (!(left < node.val && node.val < right)) {
            return false;
        }
        return valid(node.left, left, node.val) && 
               valid(node.right, node.val, right);
    }
}
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return valid(root, LONG_MIN, LONG_MAX);
    }

    bool valid(TreeNode* node, long left, long right) {
        if (!node) {
            return true;
        }
        if (!(left < node->val && node->val < right)) {
            return false;
        }
        return valid(node->left, left, node->val) &&
               valid(node->right, node->val, right);
    }
};
```

```javascript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     constructor(val = 0, left = null, right = null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
    /**
     * @param {TreeNode} root
     * @return {boolean}
     */
    isValidBST(root) {
        return this.valid(root, -Infinity, Infinity);
    }

    /**
     * @param {TreeNode} node
     * @param {number} left
     * @param {number} right
     */
    valid(node, left, right) {
        if (node === null) {
            return true;
        }
        if (!(left < node.val && node.val < right)) {
            return false;
        }
        return this.valid(node.left, left, node.val) &&
               this.valid(node.right, node.val, right);
    }
}
```

```csharp
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

public class Solution {
    public bool IsValidBST(TreeNode root) {
        return valid(root, long.MinValue, long.MaxValue);
    }

    public bool valid(TreeNode node, long left, long right) {
        if (node == null) {
            return true;
        }
        if (!(left < node.val && node.val < right)) {
            return false;
        }
        return valid(node.left, left, node.val) &&
               valid(node.right, node.val, right);
    }
}
```

::tabs-end

### Time & Space Complexity

* Time complexity: $O(n)$
* Space complexity: $O(n)$

---

## 3. Breadth First Search

::tabs-start

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True

        q = deque([(root, float("-inf"), float("inf"))])

        while q:
            node, left, right = q.popleft()
            if not (left < node.val < right):
                return False
            if node.left:
                q.append((node.left, left, node.val))
            if node.right:
                q.append((node.right, node.val, right))

        return True
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

public class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        Queue<Object[]> queue = new LinkedList<>();
        queue.offer(new Object[]{root, Long.MIN_VALUE, Long.MAX_VALUE});

        while (!queue.isEmpty()) {
            Object[] current = queue.poll();
            TreeNode node = (TreeNode) current[0];
            long left = (long) current[1];
            long right = (long) current[2];

            if (!(left < node.val && node.val < right)) {
                return false;
            }

            if (node.left != null) {
                queue.offer(new Object[]{node.left, left, (long) node.val});
            }
            if (node.right != null) {
                queue.offer(new Object[]{node.right, (long) node.val, right});
            }
        }

        return true;
    }
}
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) {
            return true;
        }

        queue<tuple<TreeNode*, long, long>> queue;
        queue.push(make_tuple(root, LONG_MIN, LONG_MAX));

        while (!queue.empty()) {
            auto [node, left, right] = queue.front();
            queue.pop();

            if (!(left < node->val && node->val < right)) {
                return false;
            }
            if (node->left) {
                queue.push(make_tuple(node->left, left, node->val));
            }
            if (node->right) {
                queue.push(make_tuple(node->right, node->val, right));
            }
        }

        return true;
    }
};
```

```javascript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     constructor(val = 0, left = null, right = null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
    /**
     * @param {TreeNode} root
     * @return {boolean}
     */
    isValidBST(root) {
        if (root === null) {
            return true;
        }

        const queue = new Queue([[root, -Infinity, Infinity]]);

        while (queue.size() > 0) {
            const [node, left, right] = queue.pop();
            
            if (!(left < node.val && node.val < right)) {
                return false;
            }
            if (node.left) {
                queue.push([node.left, left, node.val]);
            }
            if (node.right) {
                queue.push([node.right, node.val, right]);
            }
        }

        return true;
    }
}
```

```csharp
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

public class Solution {
    public bool IsValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        Queue<(TreeNode node, long left, long right)> queue = new Queue<(TreeNode, long, long)>();
        queue.Enqueue((root, long.MinValue, long.MaxValue));

        while (queue.Count > 0) {
            var (node, left, right) = queue.Dequeue();

            if (!(left < node.val && node.val < right)) {
                return false;
            }
            if (node.left != null) {
                queue.Enqueue((node.left, left, node.val));
            }
            if (node.right != null) {
                queue.Enqueue((node.right, node.val, right));
            }
        }

        return true;
    }
}
```

::tabs-end

### Time & Space Complexity

* Time complexity: $O(n)$
* Space complexity: $O(n)$