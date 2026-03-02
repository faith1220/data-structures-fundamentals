# 树形结构基础

## 从“一条线”到“分叉路”

在前面的章节中，我们学习了**数组、链表、栈、队列**，它们都被称为**线性数据结构（Linear Data Structures）**。 什么是线性结构？你可以把它想象成一条长长的单行道，每个元素只有一个“前驱（前面的元素）”和一个“后继（后面的元素）”，大家排成一条直线。

但是，现实世界中的数据往往不是纯粹线性排列的。想象一下：

1. **公司的组织架构**：CEO 下面有多个部门经理，部门经理下面有多个员工。
2. **电脑的文件夹**：C盘里有多个文件夹，每个文件夹里又包含着文件或子文件夹。
3. **人类的族谱**：爷爷奶奶有几个孩子，每个孩子又组建了自己的家庭。

为了表示这&#x79CD;**“一对多”的层级结构**，计算机科学家发明了与自然界大树非常相似的数据结构——**树（Tree）**。

和自然界中往下扎根的树不同，计算机里的树是**倒过来的**：根在最上面，叶子在最下面。

```
       [根节点/CEO] -> 最顶层的老大，没有上级
         /      \
       /          \
   [部门A]      [部门B] -> 中间的管理者（有上级也有下级）
    /   \           \
  /       \           \
[员工1] [员工2]     [员工3] -> 叶子节点，位于最底层，没有下级
```

***

## 树的核心“行话”（术语）

一棵树由许多**节点（Node）**&#x548C;连接它们的小棍子——**边（Edge）**&#x7EC4;成。为了能顺畅地学习后面的算法，我们需要先搞懂下面几个“行话”。

请结合下面这棵具体的树来看：

```
          A
        /   \
       B     C
      / \   / \
     D   E F   G
        /
       H
```

1. **根节点（Root）**：树的最顶端的起始节点。上图中的 **A** 就是根。一棵树只能有一个根。
2. **父节点（Parent）与子节点（Child）**：相连的上下两个节点。比如 **B 是 D 的父节点**，**D 是 B 的子节点**。
3. **兄弟节点（Sibling）**：有同一个父节点的节点们。比如 **D 和 E** 是亲兄弟（同一个爹 B）。
4. **叶子节点（Leaf）**：没有子节点的节点，也就是光杆司令、树的末梢。上图中的 **D、H、F、G** 都是叶子。
5. **深度（Depth）**：从根节点往下数，走到某个节点需要经过几条边。
   * 比如从 A 到 E 需要走 A->B, B->E 这 2 条边，所以 **E 的深度是 2**。
   * 根节点 A 的深度是 0。
6. **高度（Height）**：从某个节点往下走，走到最远的叶子节点需要经过几条边。
   * 比如从 B 走到最远的 H 需要走 2 条边（B->E, E->H），所以 **B 的高度是 2**。
   * 叶子节点的高度是 0。
   * “整个树的高度”就是根节点 A 的高度（这棵树高度为 3）。

> **💡 新手速记口诀：**
>
> * **深度**是从上往下看（就像站在井口看水有多深）。
> * **高度**是从下往上看（就像站在地面量楼有多高）。

***

## 什么是“二叉树”（Binary Tree）

在所有的树结构中，有一种最简单、最基础、也最重要的变体，叫做**二叉树（Binary Tree）**。

**定义**：在二叉树中，**每个节点最多只能有两个子节点**，分别被称为**左子节点（Left Child）和右子节点（Right Child）**。

为什么我们要把复杂的树限制成“最多分两个叉”呢？ 因为大部分复杂的多叉树都可以转化为简单的二叉树来处理。二叉树的结构极其清晰，只有“左”和“右”两个固定的方向，这让我们的算法编写（尤其是递归算法）变得非常容易。

***

## 二叉树的节点代码实现

还记得我们在链表章节学过的链表节点（Node）吗？链表的一个节点包含“它自己的数据”和一个指向“下一个节点”的指针。

二叉树的节点非常类似，只不过它分叉了，所以有了**两个指针**：一个指向左边的孩子，一个指向右边的孩子。

让我们看看这在各个编程语言中是如何用代码定义的：

{% tabs %}
{% tab title="Python" %}
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val      # 节点里装的数据（比如一个数字）
        self.left = left    # 指向左子节点的“小棍子/绳子”
        self.right = right  # 指向右子节点的“小棍子/绳子”
```
{% endtab %}

{% tab title="Java" %}
```java
public class TreeNode {
    public int val;          // 节点数据
    public TreeNode left;    // 左子节点指针
    public TreeNode right;   // 右子节点指针

    // 构造函数：当别人用 new TreeNode(5) 时，帮忙把数据存进去
    public TreeNode(int val) {
        this.val = val;
        this.left = null;    // 刚出生时，左右孩子都是空的
        this.right = null;
    }
}
```
{% endtab %}

{% tab title="C++" %}
```cpp
struct TreeNode {
    int val;            // 节点数据
    TreeNode* left;     // 指向左子节点的指针
    TreeNode* right;    // 指向右子节点的指针

    // 构造函数
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```
{% endtab %}

{% tab title="C#" %}
```csharp
public class TreeNode {
    public int val;         // 节点数据
    public TreeNode left;   // 左子节点指针
    public TreeNode right;  // 右子节点指针

    // 构造函数
    public TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}
```
{% endtab %}
{% endtabs %}

***

## 手动拼装一棵二叉树（搭积木）

理解了节点的定义后，我们就像搭积木一样，在代码里把一棵树拼接出来吧。这能帮你直观地感受到树在内存中是如何存在的。

我们要拼接的树长这样：

```
      1 (Root 根节点)
     / \
    2   3
   / \
  4   5
```

代码拼接过程如下：

{% tabs %}
{% tab title="Python" %}
```python
# 1. 创建零散的节点（就像取出 5 块积木，每块上面写着不同的数字）
root = TreeNode(1)
node2 = TreeNode(2)
node3 = TreeNode(3)
node4 = TreeNode(4)
node5 = TreeNode(5)

# 2. 把它们连接起来（用左右指针把积木搭好）
root.left = node2
root.right = node3

node2.left = node4
node2.right = node5

# 现在，只要你手里拿着 root 这根绳子的头，你就可以顺藤摸瓜找到这棵树上的所有节点！
print("根节点的值:", root.val)
print("根的左孩子的值:", root.left.val)
print("根的左孩子的右孩子的值:", root.left.right.val) # 顺藤摸瓜，输出 5
```
{% endtab %}

{% tab title="Java" %}
```java
// 1. 创建零散的节点
TreeNode root = new TreeNode(1);
TreeNode node2 = new TreeNode(2);
TreeNode node3 = new TreeNode(3);
TreeNode node4 = new TreeNode(4);
TreeNode node5 = new TreeNode(5);

// 2. 连接节点：告诉它们谁是谁的左孩子，谁是谁的右孩子
root.left = node2;
root.right = node3;

node2.left = node4;
node2.right = node5;

// 测试这棵树是否拼接成功
System.out.println("根节点的值: " + root.val);
System.out.println("根的左孩子的值: " + root.left.val);
System.out.println("根的左孩子的右孩子的值: " + root.left.right.val); // 顺藤摸瓜，输出 5
```
{% endtab %}

{% tab title="C++" %}
```cpp
#include <iostream>

// 1. 创建零散的节点 (使用 new 在堆内存中分配)
TreeNode* root = new TreeNode(1);
TreeNode* node2 = new TreeNode(2);
TreeNode* node3 = new TreeNode(3);
TreeNode* node4 = new TreeNode(4);
TreeNode* node5 = new TreeNode(5);

// 2. 连接节点：用箭头操作符赋值指针
root->left = node2;
root->right = node3;

node2->left = node4;
node2->right = node5;

// 测试
std::cout << "根节点的值: " << root->val << std::endl;
std::cout << "根的左孩子的右孩子的值: " << root->left->right->val << std::endl; // 输出 5

// (注：C++ 在实际纯手写代码时要注意手动 delete 释放内存，这里为演示结构而省略)
```
{% endtab %}

{% tab title="C#" %}
```csharp
// 1. 创建零散的节点
TreeNode root = new TreeNode(1);
TreeNode node2 = new TreeNode(2);
TreeNode node3 = new TreeNode(3);
TreeNode node4 = new TreeNode(4);
TreeNode node5 = new TreeNode(5);

// 2. 连接节点
root.left = node2;
root.right = node3;

node2.left = node4;
node2.right = node5;

// 测试
Console.WriteLine("根节点的值: " + root.val);
Console.WriteLine("根的左孩子的右孩子的值: " + root.left.right.val); // 顺藤摸瓜，输出 5
```
{% endtab %}
{% endtabs %}

## 本章小结

* **树**是用来表&#x793A;**“一对多”层级关系**的数据结构，比如公司架构、电脑目录。
* 计算机里的树是**倒立的**：顶部叫**根（Root）**，向下生长，末梢叫**叶子（Leaf）**。记住**深度从上往下数，高度从下往上数**。
* **二叉树**是每个节点最多有两个分叉（左右孩子）的树，是最简单也是学习后续高级算法的基石。
* 二叉树在代码里本质上就是一种**稍微复杂一点的链表**：链表只有一个指向后面的 `next` 指针，而二叉树节点拥有**两个分叉的指针 `left` 和 `right`**。
