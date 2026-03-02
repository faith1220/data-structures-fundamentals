# 二叉搜索树

在前面的章节中，我们学习了二叉树的长相和遍历方式。但是，普通的二叉树如果只是随便乱挂节点，查找一个数字就只能把整棵树翻一遍，效率等同于在无序数组里找东西。

为了让查找变得极快，计算机科学家给二叉树定下了一条**非常严格的“家规”**。遵守这条家规的二叉树，就叫做——**二叉搜索树（Binary Search Tree，简称 BST）**。

## BST 的那条铁血“家规”

一棵二叉树要成为二叉搜索树，它的每一个节点都必须绝对服从以下规则：

1. **左边的都比我小**：也就是当前节点的所有左子树节点，值都比当前节点小。
2. **右边的都比我大**：也就是当前节点的所有右子树节点，值都比当前节点大。
3. **不能有等值的节点**（在最基础的定义中通常不允许重复元素，如果允许，可以规定放在左边或右边）。

这就是一棵标准的二叉搜索树：

```
          8 (根节点)
        /   \
       3     10
      / \      \
     1   6      14
        / \     /
       4   7   13
```

你随便挑一个节点检查，比如 `3`。它的左边 `1` 比它小，它的右边 `6, 4, 7` 全都比它大。这是极其严格的！

### 为什么这个“家规”极其牛逼？

回忆一下大家平时&#x73A9;**“猜数字”游戏**：我说一个 1-100 的数字，你猜 50，我告诉你“猜小了”。下一步你会去哪里猜？肯定是在 51-100 里猜对吧！你绝对不会再去 1-49 里找了。

BST 就是长在树上的“猜数字”游戏（二分查找）！ 假设我们要在这棵树里找数字 `7`：

1. 从根节点 `8` 开始，`7 < 8`，所以我们直接抛弃右边，**去左边找**。
2. 来到 `3`，`7 > 3`，所以抛弃左边，**去右边找**。
3. 来到 `6`，`7 > 6`，继续**去右边找**。
4. 来到 `7`，找到了！

每往下走一步，我们就砍掉了一半的错误选项。这种查找速度是极快的，时间复杂度为 **O(log n)**。

***

## 极其神奇的现象：中序遍历出奇迹

上一章我们学了**中序遍历（In-order Traversal：左 -> 根 -> 右）**。 如果你对一棵 BST 进行中序遍历，你会发现一个震惊的事实：**打印出来的数字是完全从小到大排好序的！**

对上面那棵树进行中序遍历的结果一定是：`1, 3, 4, 6, 7, 8, 10, 13, 14`。 原因很简单：因为 BST 规定了“左 < 根 < 右”，而中序遍历的顺序恰好也是“左边走完，再走根，最后走右边”。这完美契合了从小到大的顺序。

***

## 操作一：在 BST 中查找（Search）

代码写起来就跟用嘴巴说话一样简单。如果比我小，就去左孩子家找；如果比我大，就去右孩子家找；如果一样大，就是找到了；如果走到空房子（null），说明这个人不存在。

{% tabs %}
{% tab title="Python" %}
```python
def search_bst(root, target):
    # 如果走到空（没找到），或者刚好找到了，就停止
    if root is None or root.val == target:
        return root

    # 如果目标数字比我小，去左边找
    if target < root.val:
        return search_bst(root.left, target)

    # 如果目标数字比我大，去右边找
    return search_bst(root.right, target)
```
{% endtab %}

{% tab title="Java" %}
```java
public TreeNode searchBST(TreeNode root, int target) {
    if (root == null || root.val == target) {
        return root;
    }
    if (target < root.val) {
        return searchBST(root.left, target); // 去左子树找
    } else {
        return searchBST(root.right, target); // 去右子树找
    }
}
```
{% endtab %}

{% tab title="C++" %}
```cpp
TreeNode* searchBST(TreeNode* root, int target) {
    if (root == nullptr || root->val == target) {
        return root;
    }
    if (target < root->val) {
        return searchBST(root->left, target);
    } else {
        return searchBST(root->right, target);
    }
}
```
{% endtab %}

{% tab title="C#" %}
```csharp
public TreeNode SearchBST(TreeNode root, int target) {
    if (root == null || root.val == target) {
        return root;
    }
    if (target < root.val) {
        return SearchBST(root.left, target);
    } else {
        return SearchBST(root.right, target);
    }
}
```
{% endtab %}
{% endtabs %}

***

## 操作二：在 BST 中插入新节点（Insert）

插入也是一样的道理。新来的数字顺着根节点一路比大小，小去左，大去右，一直走到发现某个方向没有孩子了（遇到了 `null`），就把新节点挂在那里。

例如，我们要把 `5` 插入进树里：`5 < 8`去左边，`5 > 3`去右边，`5 < 6`去左边走到 `4`，`5 > 4`去右边。`4`的右边是空的，那就把 `5` 当作 `4` 的右孩子！

{% tabs %}
{% tab title="Python" %}
```python
def insert_bst(root, val):
    # 如果树本来是空的，新节点直接当根老板
    if root is None:
        return TreeNode(val)

    # 按照规矩往下找空位
    if val < root.val:
        root.left = insert_bst(root.left, val)
    elif val > root.val:
        root.right = insert_bst(root.right, val)

    # 返回当前的根节点给上一层
    return root
```
{% endtab %}

{% tab title="Java" %}
```java
public TreeNode insertBST(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val); // 找到空位，造个新家
    }

    if (val < root.val) {
        root.left = insertBST(root.left, val);
    } else if (val > root.val) {
        root.right = insertBST(root.right, val);
    }
    return root;
}
```
{% endtab %}

{% tab title="C++" %}
```cpp
TreeNode* insertBST(TreeNode* root, int val) {
    if (root == nullptr) {
        return new TreeNode(val);
    }
    if (val < root->val) {
        root->left = insertBST(root->left, val);
    } else if (val > root->val) {
        root->right = insertBST(root->right, val);
    }
    return root;
}
```
{% endtab %}

{% tab title="C#" %}
```csharp
public TreeNode InsertBST(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    if (val < root.val) {
        root.left = InsertBST(root.left, val);
    } else if (val > root.val) {
        root.right = InsertBST(root.right, val);
    }
    return root;
}
```
{% endtab %}
{% endtabs %}

***

## 操作三：BST 的删除操作（最头疼的一集）

在 BST 里删人非常麻烦，因为这棵树有森严的“大小家规”。如果你随便把一个中间的管理层（节点）给删了，那他下面的小弟们该怎么重新站队安排？

删除操作需要分为 **3 种情况**来对症下药：

1. **情况A：要删的人是光杆司令（叶子节点）。** 这是最爽的。比如要删 `1` 或 `4`。他下面没小弟了，直接把他老板指向他的指针设定为 `null` 即可，走得很干脆。
2. **情况B：要删的人只有一个孩子（单亲家庭）。** 这也挺好办。比如要删 `10`（他只有右孩子 `14`）。你把 `10` 删掉，然后让他唯一的孩子 `14` 直接顶替他的位置去向 `8`（原来的爷爷）汇报就可以了。爷孙直接牵手，跨过中间人。
3. **情况C：要删的人有儿有女（左右都有孩子）。** 比如我们要直接干掉根节点 `8`。完蛋，它左边一大家子，右边一大家子，它没了，谁来接班当最大的老板？ **解决思路**：必须去他的小弟里找一个**最门当户对**的人来顶包。 谁最合适？答案是：**它的左子树里最大的那个（也就是中序遍历里原来在 8 前面的那个），或者是右子树里最小的那个（中序遍历里排 8 后面的那个）。** 比如在前面的图里，我们去 `8` 的右边找最小的，那是 `10`。我们就把 `10` 提拔到原来 `8` 的位置，然后把原本底下的 `10` 按照情况A或者情况B给删掉。

> **因为删除代码对纯新手极其复杂，这里就不放出大段的四语言枯燥代码了。只要你在脑子里牢牢记住了“找接班人顶包”的原理，将来应对面试和读源码就会非常从容。**

## BST 的致命缺陷

二叉搜索树看起来很完美？并不是的。

如果你插入数据的顺序非常倒霉，比如按顺序插入 `1, 2, 3, 4, 5`：

```
  1
   \
    2
     \
      3
       \
        4
         \
          5
```

这棵树完全偏向了一边，**退化成了一条链表！** 此时你再去查数字 5，你得一个一个往下走，时间复杂度直接退化回了可怕的 **O(n)**。

为了解决这个随时可能瘸腿的问题，计算机科学家们发明了**带自平衡功能的**二叉搜索树（比如大名鼎鼎的**红黑树 Red-Black Tree** 和 **AVL树**），它们在发现树有些歪的时候，会自动“旋转”来保持左右两边差不多高！

## 本章小结

* **BST 家规**：左下属比我小，右下属比我大。
* **查找极快**：就像猜数字一样，每次排除一半错误答案，时间复杂度 `O(log n)`。
* **中序遍历排好序**：对 BST 做中序遍历，结果就是从小到大排好序的数组。
* **删除很磨人**：如果有两个孩子，必须找“左子树最大”或“右子树最小”来顶包接班。
* **极端退化**：如果不加节制，BST 可能退化成一条链表。由此引出了未来会学到的自平衡树。
