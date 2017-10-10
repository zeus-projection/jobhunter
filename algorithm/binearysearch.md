# 二叉树遍历

### 二叉树层次遍历

不重复地访问某种树的所有节点的过程。

二叉树遍历分两种

1. 深度优先遍历
   先序遍历，中序遍历，后序遍历；
2. 广度优先遍历




**深度优先遍历之 先序遍历（Pre-ORder Traversal）**

​	先访问根，然后，访问子树的遍历方式

```c
void pre_order_traversal(TreeNode* root) {
  //Do something with root
  if(root-> lchild != null) 
    pre_order_traversal(root->lchild);
  if(root-> rchild != null)
    pre_order_traversal(root->rchild);
}
```

**深度优先遍历之 中序遍历（In-ORder Traversal）**

​	先访问左（右）子树，然后访问根，最后访问右（左）子树的遍历方式

```c
void in_order_traversal(TreeNode* root) {
  if(root-> lchild != null
    in_order_traversal(root->lchild);
  //Do something with root
  if(root-> rchild != null)
    in_order_traversal(root->rchild);
}
```

**深度优先遍历之 后序遍历（Post-ORder Traversal）**

​	先访问子树，然后访问根的遍历方式

```c
void post_order_traversal(TreeNode* root) {
  if(root-> lchild != null) 
    post_order_traversal(root->lchild);
  if(root-> rchild != null)
    post_order_traversal(root->rchild
  //Do something with root
}
```



**广度优先遍历：**

​	和深度优先遍历不同，广度优先遍历会先访问离根节点最近的节点。二叉树的广度优先遍历又称按层次遍历。借助队列实现

```c
void Layer_Traver(tree* root) {
  int head = 0, tail = 0;
  tree* p[SIZE] = {NULL};
  tree* tmp;
  if(root != NULL) {
    p[head] = root;
    tail++;
    //Do something with p[head]
  } else {
    return;
  }
  while (head < tail) {
    tmp = p[head];
    if(tmp->left != NULL) {
      p[tail] = tmp->left;
      tail++
      //Do something with p[head]
    } 
    if(tmp->right != NULL) {
      p[tail] = tmp->right;
      tail++;
      //Do something with p[head]
    }
    head++;
  }
  return;
}
```

分层遍历二叉树

```java
public boolean printNodeAtLevel(TreeNode root, int level, List<Integer> list) {
  if(root == null || level < 0) {
    return false;
  }
  if(level == 0 && root != null) {
    list.add(root.val);
    return true;
  }
  boolean left = printNodeAtLevel(root.left, level - 1, list);
  boolean right = printNodeAtLevel(root.right, level - 1, list);
  return left || right;
}
```

```java
public int maxDepth(TreeNode root) {
  if(root == null) {
    return 0;
  }
  int left = 1 + maxDepth(root.left);
  int right = 1 + maxDepth(root.right);
  return Math.max(left, right);
}
```

```java
public List<List<Integer>> levelOrder(TreeNode root) {
  List<List<Integer>> lists = new ArrayList<List<Integer>>();
  if(root == null)
    return lists;
  for (int level =0;;level++) {
    List<Integer> list = new ArrayList<>();
   	if(!printNodeAtLevel(root, level, list))
      break;
    lists.add(list);
  }
  return lists;
}
```

```java
public List<List<Integer>> levelOrder2(TreeNode root) {
  List<List<Integer>> lists = new ArrayList<Lsit<Integer>>();
  if(root == null) {
    return lists;
  }
  int cur = 0, last = 1;
  List<TreeNode> queue = new ArrayList<>();
  queue.add(root);
  while(cur < queue.size()) {
    last = queue.size();
    List<Integer> list = new ArrayList<>();
    while (cur < last) {
      TreeNode node = queue.get(cur);
      list.add(node.val);
      if(node.left != null) {
        queue.add(node.left);
      }
      if(node.right != right) {
        queue.add(node.right);
      }
      cur++;
    }
    lists.add(list);
  }
  return lists;
}
```
### 二叉树

```java
public List<Integer> inorderTraversal(TreeNode root) {
  List<Integer> list = new ArrayList<Integer>();
  
  Stack<TreeNode> stack = new Stack<TreeNode>();
  
  TreeNode cur = root;
  while(cur != null || !stack.empty()) {
    while(cur != null) {
      stack.add(cur);
      cur = cur.left;
    }
    cur = stck.pop();
    list.add(cur.val);
    cur = cur.right;
  }
  return lsit;
}
```