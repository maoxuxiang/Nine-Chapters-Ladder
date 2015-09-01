# Binary Tree & Divide Conquer

- [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
- [Insert Node in a Binary Search Tree](#insert-node-in-a-binary-search-tree)
- [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal)
- [Validate Binary Search Tree](#validate-binary-search-tree)
- [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)
- [Balanced Binary Tree](#balanced-binary-tree)
- [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)
- [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)
- [Search Range in Binary Search Tree](#search-range-in-binary-search-tree)
- [Binary Search Tree Iterator](#binary-search-tree-iterator)

<hr />
## Maximum Depth of Binary Tree
https://leetcode.com/problems/maximum-depth-of-binary-tree/

思路：

左右子树求取最大值，然后+1

``` java
public class Solution {
    public int maxDepth(TreeNode root) {
        return root != null ? Math.max(maxDepth(root.left), maxDepth(root.right)) + 1 : 0;
    }
}
```

<hr />
## Insert Node in a Binary Search Tree
http://www.lintcode.com/en/problem/insert-node-in-a-binary-search-tree/

``` java
public class Solution {
    public TreeNode insertNode(TreeNode root, TreeNode node) {
        if (root == null) {
            return node;
        }
        if (root.val > node.val) {
            root.left = insertNode(root.left, node);
        } else {
            root.right = insertNode(root.right, node);
        }
        return root;
    }
}
```

<hr />
## Binary Tree Preorder Traversal
https://leetcode.com/problems/binary-tree-preorder-traversal/

思路：

1. 递归

    先序遍历：根左右

2. 非递归

    利用栈，先序遍历：先访问根结点，入栈时先入右结点，再入左结点

3. 分治

    自底向上

``` java
public class Solution {

    // 1. Recursion
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        reverse(root, result);
        return result;
    }

    private void reverse(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        result.add(root.val);
        reverse(root.left, result);
        reverse(root.right, result);
    }

    // 2. Non-Recursion
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();

        if (root == null) {
            return result;
        }

        stack.push(root);
        while (!stack.empty()) {
            TreeNode node = stack.pop();
            result.add(node.val);
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }

        return result;
    }

    // 3. Divide & Conquer
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if(root==null) {
            return result;
        }

        // Divide
        List<Integer> left = preorderTraversal(root.left);
        List<Integer> right = preorderTraversal(root.right);

        // Conquer
        result.add(root.val);
        result.addAll(left);
        result.addAll(right);
        return result;
    }
}
```

<hr />
## Validate Binary Search Tree
https://leetcode.com/problems/validate-binary-search-tree/

思路：

对于每一个子树，限制它的最大，最小值，如果超过则返回false

对于根节点，最大最小都不限制

每一层节点，左子树最大值小于根，右子树最小值大于根

``` java
public class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Double.POSITIVE_INFINITY, Double.NEGATIVE_INFINITY);
    }

    private boolean isValidBST(TreeNode root, double max, double min) {
        if (root == null) {
            return true;
        }
        if (root.val > min && root.val < max
                && isValidBST(root.left, root.val, min)
                && isValidBST(root.right, max, root.val)) {
            return true;
        } else {
            return false;
        }
    }
}
```


<hr />
## Binary Tree Maximum Path Sum
https://leetcode.com/problems/binary-tree-maximum-path-sum/

思路：

结点有可能是负值，所以一个以`root`为根的树的最大路径和`maxPathSum(root)`有以下四种情况

1. `root.val`
2. `root.val` + `getMax(left)`
3. `root.val` + `getMax(right)`
4. `root.val` + `getMax(left)` + `getMax(right)`（跨越左右子树）

其中，`getMax(root)`是当前结点不包含第四种情况的最大路径和

``` java
public class Solution {
    private int max = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        getMax(root);
        return max;
    }

    public int getMax(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = getMax(root.left);
        int right = getMax(root.right);
        int current = Math.max(root.val, Math.max(root.val + left, root.val + right));
        max = Math.max(max, Math.max(current, root.val + left + right));
        return current;
    }
}
```


<hr />
## Balanced Binary Tree
https://leetcode.com/problems/balanced-binary-tree/

思路一：

自顶向下，有很多重复计算

``` java
public class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (Math.abs(getDepth(root.left) - getDepth(root.right)) > 1) {
            return false;
        }

        return isBalanced(root.left) && isBalanced(root.right);
    }

    private int getDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return Math.max(getDepth(root.left) + 1, getDepth(root.right) + 1);
    }
}
```

思路二：

后序遍历，自底向上计算

``` java
public class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        if (getBalance(root) == -1) {
            return false;
        }
        return true;
    }

    private int getBalance(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = getBalance(root.left);
        if (left == -1) {
            return -1;
        }

        int right = getBalance(root.right);
        if (right == -1) {
            return -1;
        }

        if (Math.abs(left - right) > 1) {
            return -1;
        }

        return Math.max(left, right) + 1;
    }
}
```


<hr />
## Lowest Common Ancestor of a Binary Tree
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

``` java
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 如果有一个match，则说明当前结点就是要找的最低公共祖先
        if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 如果一个在左子树找到，一个在右子树找到，则说明root是最低公共祖先
        if (left != null && right != null) {
            return root;
        }

        // 其他情况
        if (left != null) {
            return left;
        }
        if (right != null) {
            return right;
        }
        return null;
    }
}
```


<hr />
## Binary Tree Level Order Traversal
https://leetcode.com/problems/binary-tree-level-order-traversal/

``` java
public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        LinkedList<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            List<Integer> res = new ArrayList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                res.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            result.add(res);
        }
        
        return result;
    }
}
```


<hr />
## Search Range in Binary Search Tree
http://www.lintcode.com/en/problem/search-range-in-binary-search-tree/

``` java

```


<hr />
## Binary Search Tree Iterator
https://leetcode.com/problems/binary-search-tree-iterator/

``` java

```
