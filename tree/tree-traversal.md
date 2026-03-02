# 二叉树的遍历

在上一章中，我们学会了如何用代码把一棵树“搭”出来。现在，假设这棵树上挂满了苹果（每个苹果上写着一个数字），我们要去把所有的苹果都摘下来（或者说看一遍），这就引出了树形结构中最经典的操作——**遍历（Traversal）**。

## 什么是“遍历”？

**遍历**的意思就是**按照某种规则，把树上的每一个节点都访问一遍，且只访问一次**。

如果是数组或链表（线性结构），遍历太简单了！闭着眼睛写个 `for` 循环或者 `while` 循环，从头走到尾就完事了。
但是，面对一棵树这样的“分叉路口”，我们走到一个节点时，会面临一个选择：**接下来是往左走，还是往右走？**

为了不迷路，计算机科学家们总结了两大类遍历策略：
1. **深度优先搜索（DFS - Depth-First Search）**：一条路走到黑，撞到南墙再回头。
2. **广度优先搜索（BFS - Breadth-First Search）**：一层一层地往下扫，就像水波纹一层层扩散。

---

## 策略一：深度优先搜索（DFS）与“老板分发任务”

在讲代码之前，我们先来学习一个超级强大的思想：**递归（Recursion）**。
树这种结构简直就是为递归而生的。你可以把遍历一棵树的过程，想象成**“公司老板向下分发任务”**的过程。

假设你要遍历下面这棵树：
```text
      A (大老板)
     / \
    B   C (两个部门经理)
   / \   \
  D   E   F (三个基层员工)
```

**递归思维**是这样运转的：
1. 大老板 A 想要统计全公司的人头。但他很懒，他不想亲自去数。
2. 他对经理 B 说：“你去把你手底下的人都统计一遍汇报给我。”
3. 他对经理 C 说：“你去把你手底下的人都统计一遍汇报给我。”
4. 他自己只需要把自己（A）加上，任务就完成了！

你看，**“遍历整棵树”**的大任务，被巧妙地拆解成了**“遍历左子树”**和**“遍历右子树”**两个完全一样的小任务！

根据“大老板（根节点）”在什么时候做自己的工作，我们将 DFS 细分为三种：
- **前序遍历（Pre-order）**：老板先干活，再让左边下属干，最后让右边下属干。**(根 -> 左 -> 右)**
- **中序遍历（In-order）**：先让左边下属干完，老板再干活，最后让右边下属干。**(左 -> 根 -> 右)**
- **后序遍历（Post-order）**：先让左右下属都干完活，老板最后再来收尾干活。**(左 -> 右 -> 根)**

### 1. 前、中、后序遍历的代码实现

仔细观察下面 4 种语言的代码，你会发现一个极其神奇的现象：**这三种遍历的代码几乎一模一样，唯一的区别仅仅是 `打印节点的值` 这行代码放在了哪里！**

{% tabs %}
{% tab title="Python" %}
```python
# 1. 前序遍历 (根 -> 左 -> 右)
def pre_order(root):
    if root is None: # 如果当前节点是空的（下属不存在），直接返回
        return
    print(root.val, end=" ") # 老板先干活（访问根节点）
    pre_order(root.left)     # 安排左部门干活
    pre_order(root.right)    # 安排右部门干活

# 2. 中序遍历 (左 -> 根 -> 右)
def in_order(root):
    if root is None:
        return
    in_order(root.left)      # 安排左部门先干活
    print(root.val, end=" ") # 左边干完了，老板干活（访问根节点）
    in_order(root.right)     # 最后安排右部门干活

# 3. 后序遍历 (左 -> 右 -> 根)
def post_order(root):
    if root is None:
        return
    post_order(root.left)    # 安排左部门干活
    post_order(root.right)   # 安排右部门干活
    print(root.val, end=" ") # 大家都干完了，老板最后来收尾（访问根节点）
```
{% endtab %}
{% tab title="Java" %}
```java
// 1. 前序遍历 (根 -> 左 -> 右)
public void preOrder(TreeNode root) {
    if (root == null) return;
    System.out.print(root.val + " "); // 根
    preOrder(root.left);              // 左
    preOrder(root.right);             // 右
}

// 2. 中序遍历 (左 -> 根 -> 右)
public void inOrder(TreeNode root) {
    if (root == null) return;
    inOrder(root.left);               // 左
    System.out.print(root.val + " "); // 根
    inOrder(root.right);              // 右
}

// 3. 后序遍历 (左 -> 右 -> 根)
public void postOrder(TreeNode root) {
    if (root == null) return;
    postOrder(root.left);             // 左
    postOrder(root.right);            // 右
    System.out.print(root.val + " "); // 根
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <iostream>

// 1. 前序遍历 (根 -> 左 -> 右)
void preOrder(TreeNode* root) {
    if (root == nullptr) return;
    std::cout << root->val << " "; // 根
    preOrder(root->left);          // 左
    preOrder(root->right);         // 右
}

// 2. 中序遍历 (左 -> 根 -> 右)
void inOrder(TreeNode* root) {
    if (root == nullptr) return;
    inOrder(root->left);           // 左
    std::cout << root->val << " "; // 根
    inOrder(root->right);          // 右
}

// 3. 后序遍历 (左 -> 右 -> 根)
void postOrder(TreeNode* root) {
    if (root == nullptr) return;
    postOrder(root->left);         // 左
    postOrder(root->right);        // 右
    std::cout << root->val << " "; // 根
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
// 1. 前序遍历 (根 -> 左 -> 右)
public void PreOrder(TreeNode root) {
    if (root == null) return;
    Console.Write(root.val + " "); // 根
    PreOrder(root.left);           // 左
    PreOrder(root.right);          // 右
}

// 2. 中序遍历 (左 -> 根 -> 右)
public void InOrder(TreeNode root) {
    if (root == null) return;
    InOrder(root.left);            // 左
    Console.Write(root.val + " "); // 根
    InOrder(root.right);           // 右
}

// 3. 后序遍历 (左 -> 右 -> 根)
public void PostOrder(TreeNode root) {
    if (root == null) return;
    PostOrder(root.left);          // 左
    PostOrder(root.right);         // 右
    Console.Write(root.val + " "); // 根
}
```
{% endtab %}
{% endtabs %}

### 这三种遍历有什么用？
- **前序遍历**：极度适合用来**复制一棵树**，或者在树中查找某个特定的值。因为老板最先开始干活，遇到事情最先处理。
- **中序遍历**：在后面我们要学的**二叉搜索树（BST）**中，中序遍历出来的结果恰好是一个**从小到大排序好的数组**！非常神奇！
- **后序遍历**：极度适合用来**删除一棵树**，或者计算树的高度。因为你要删除一个节点，必须先把它的左右孩子先删掉（下属都离职了，老板最后才走）。

---

## 策略二：广度优先搜索（BFS）层序遍历

如果你觉得把大树“一条路走到黑”太绕了，那**层序遍历（Level-order Traversal）**绝对符合人类最淳朴的直觉。

层序遍历顾名思义：**自顶向下，一层一层地看；在同一层里，从左到右一个个看。**

```text
按层序遍历这棵树：
      1          -> 第一层
     / \
    2   3        -> 第二层
   / \   \
  4   5   6      -> 第三层

输出结果就应该是：1, 2, 3, 4, 5, 6
```

### 怎么实现层序遍历？借助“队列”！

还记得我们在前面章节学过的**队列（Queue）**吗？“先进先出（FIFO）”的排队机制。
层序遍历的核心思想就是开一个队列，然后重复以下操作：
1. 先把根节点请进队列排队。
2. 只要队列不为空，就让队头的人出列（打印它）。
3. 队头这个人出列的同时，把他的左孩子和右孩子（如果有的话）强行拉进队列的尾部继续排队。
4. 周而复始，直到所有人都出列。

{% tabs %}
{% tab title="Python" %}
```python
from collections import deque

def level_order(root):
    if root is None:
        return

    # 初始化队列，先把根节点放进去
    queue = deque([root])

    while queue: # 只要队列里还有人
        # 1. 队头出列
        node = queue.popleft()
        print(node.val, end=" ")

        # 2. 把出来的这个人的左孩子拉进队列排队
        if node.left:
            queue.append(node.left)

        # 3. 把出来的这个人的右孩子拉进队列排队
        if node.right:
            queue.append(node.right)
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.LinkedList;
import java.util.Queue;

public void levelOrder(TreeNode root) {
    if (root == null) return;

    // 初始化队列
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root); // 根节点入队

    while (!queue.isEmpty()) {
        // 1. 队头出列
        TreeNode node = queue.poll();
        System.out.print(node.val + " ");

        // 2 & 3. 左右孩子依次入队排队
        if (node.left != null) {
            queue.offer(node.left);
        }
        if (node.right != null) {
            queue.offer(node.right);
        }
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <iostream>
#include <queue>

void levelOrder(TreeNode* root) {
    if (root == nullptr) return;

    std::queue<TreeNode*> queue;
    queue.push(root); // 根节点入队

    while (!queue.empty()) {
        // 1. 队头出列
        TreeNode* node = queue.front();
        queue.pop();
        std::cout << node->val << " ";

        // 2 & 3. 左右孩子依次入队排队
        if (node->left != nullptr) {
            queue.push(node->left);
        }
        if (node->right != nullptr) {
            queue.push(node->right);
        }
    }
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System.Collections.Generic;

public void LevelOrder(TreeNode root) {
    if (root == null) return;

    Queue<TreeNode> queue = new Queue<TreeNode>();
    queue.Enqueue(root); // 根节点入队

    while (queue.Count > 0) {
        // 1. 队头出列
        TreeNode node = queue.Dequeue();
        Console.Write(node.val + " ");

        // 2 & 3. 左右孩子依次入队排队
        if (node.left != null) {
            queue.Enqueue(node.left);
        }
        if (node.right != null) {
            queue.Enqueue(node.right);
        }
    }
}
```
{% endtab %}
{% endtabs %}

## 本章小结

- **遍历** 就是按照规则走遍树的所有节点。
- **深度优先搜索（DFS）** 依赖“递归”思维。根据“访问根节点”的时机不同，分为：
  - **前序遍历**：根 -> 左 -> 右
  - **中序遍历**：左 -> 根 -> 右
  - **后序遍历**：左 -> 右 -> 根
- **广度优先搜索（BFS/层序遍历）** 依赖“队列”这种数据结构来实现。一层层像切黄瓜一样把它剥离出来。

掌握了二叉树的遍历，你就拿到了开启高级树形算法结构大门的钥匙！在下一章，我们将利用这一章学习的“遍历”绝技，去认识一个在数据库和搜索引擎中极其重要的数据结构：**二叉搜索树（BST, Binary Search Tree）**。