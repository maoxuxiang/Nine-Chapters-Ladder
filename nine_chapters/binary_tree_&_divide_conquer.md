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

``` java

```


<hr />
## Binary Tree Maximum Path Sum
https://leetcode.com/problems/binary-tree-maximum-path-sum/

``` java

```


<hr />
## Balanced Binary Tree
https://leetcode.com/problems/balanced-binary-tree/

``` java

```


<hr />
## Lowest Common Ancestor of a Binary Tree
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

``` java

```


<hr />
## Binary Tree Level Order Traversal
https://leetcode.com/problems/binary-tree-level-order-traversal/

``` java

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
