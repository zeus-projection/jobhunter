# Validate Binary Search Tree

[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

> 判断一棵树，是不是二叉查找树

```java
public boolean isValidBST(TreeNode root) {
  return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

public boolean isValidBST(TreeNode root, long minVal, long maxVal) {
  if(root == null) return true;
  if(root.val >= maxVal || root.val <= minVal) return false;
  return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
}
```

##### 延伸问题 树的前序遍历

1. Binary Tree Inorder Traversal

   ```java
   public List<Integer> inorderTraversal(TreeNode root) {
     List<Integer> list = new ArrayList<>();
     if(root == null) return list;
     Stack<TreeNode> stack = new Stack<>();
     while(root != null || !stack.empty()) {
       while(root != null) {
         stack.push(root);
         root = root.left;
       }
       root = stack.pop();
       list.add(root.val);
       root = root.right;
     }
     return list;
   }
   ```

2. Kth Smallest Element in BST

   ```java
   public int kthSmallest(TreeNode root, int k) {
     Stack<TreeNode> stack = new Stack<>();
     while(root != null || !stack.isEmpty()) {
       while(root != null) {
         stack.push(root);
         root = root.left;
       }
       root = stack.pop();
       if(--k == 0) break;
       root = root.right;
     }
     return root.val;
   }
   ```

3. Validate Binary Search Tree

   ```java
   public boolean isValidBST(TreeNode root) {
     if(root == null) return true;
     Stack<TreeNode> stack = new Stack<>();
     TreeNode pre = null;
     while(root != null || !stack.isEmpty()) {
       while(root != null) {
         stack.push(root);
         root = root.left;
       }
       root = stack.pop();
       if(pre != null && root.val <= pre.val) return false;
       pre = root;
       root = root.right;
     }
     return true;
   }
   ```