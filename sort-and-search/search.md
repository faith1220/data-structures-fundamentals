# 查找算法 (Search Algorithms)

在计算机世界里，我们把数据存起来（数据结构），又费尽心思把它们排好序（排序算法），最终的目的其实只有一个：**为了能够更快速地找回它们（查找）**。

如果你面前有一堆**乱序**的数字，你想找其中一个，你唯一的办法就是从头到尾挨个看一遍。这叫**线性查找 (Linear Search)**，时间复杂度是 $O(n)$。如果数据有 1 亿条，最倒霉的情况下你得找 1 亿次。

但是！如果你面前的数据是**已经排好序**的呢？我们就拥有了计算机科学中最经典、最伟大的查找魔法 —— **二分查找 (Binary Search)**。

---

## 现实生活中的二分查找

**场景一：猜数字游戏**
朋友让你猜一个 1 到 100 之间的数字。
你会从 1 开始猜：1？2？3？4？（这就是傻傻的线性查找）。
聪明人会直接猜：**50！**
朋友说：“大了。”
那你立刻就知道，1 到 50 都不可能了！目标一定在 1 到 49 之间。于是你接着猜：**25！**
朋友说：“小了。”
你瞬间又把范围缩小了一半，目标在 26 到 49 之间。
这样每次把范围劈成两半的猜法，就是二分查找。

**场景二：翻字典查单词**
你想在英汉词典里查 `Monkey` 这个单词。
你绝对不会从第 1 页的 `A` 开始一页页往后翻。你通常是一把翻到字典的中间找 `M`。如果翻到了 `P` 开头，你就知道 `Monkey` 一定在左半本；如果翻到了 `F` 开头，就知道它在右半本。
之所以你能这么爽快地查字典，**也就是因为字典里的单词全都是按字母顺序“排好序”的！**

---

## 二分查找的核心逻辑

**硬性前提**：数组**必须**是已经排好序的（通常是从小到大）。

我们只需要准备三个指针：
1. `left`：指向当前搜索范围的最左边。
2. `right`：指向当前搜索范围的最右边。
3. `mid`：指向 `left` 和 `right` 的正中间。

**规则**：
- 看一下 `mid` 指向的数字是不是你要找的。
- 如果是，太棒了，直接下班！
- 如果你要找的数字比 `mid` **大**，说明目标在右半边去。那么让 `left = mid + 1`。
- 如果你要找的数字比 `mid` **小**，说明目标在左半边去。那么让 `right = mid - 1`。
- 重复以上步骤，直到 `left` 跑到了 `right` 的右边（说明全找遍了还没找到，目标不存在）。

---

## 满分代码实现

我们来看看四种主流语言是如何优雅地写出二分查找的。

{% tabs %}
{% tab title="Python" %}
```python
def binary_search(arr, target):
    left = 0
    right = len(arr) - 1

    # 只要搜索区间有效，就继续劈两半
    while left <= right:
        # 找到中间位置
        mid = (left + right) // 2

        if arr[mid] == target:
            return mid       # 运气爆棚，刚好命中中间的，返回下标

        elif arr[mid] < target:
            left = mid + 1   # 目标比中间大，去右边找

        else:
            right = mid - 1  # 目标比中间小，去左边找

    # 如果 while 循环结束了还没 return，说明字典里根本没这个词
    return -1
```
{% endtab %}
{% tab title="Java" %}
```java
public class BinarySearch {
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;

        while (left <= right) {
            // 注意：(left + right) / 2 有时会因为数字太大导致内存溢出
            // 工业界极为严谨的标准写法是： left + (right - left) / 2
            int mid = left + (right - left) / 2;

            if (arr[mid] == target) {
                return mid;       // 找到了，返回下标
            } else if (arr[mid] < target) {
                left = mid + 1;   // 去右半边找
            } else {
                right = mid - 1;  // 去左半边找
            }
        }

        return -1; // 查无此人
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>

int binarySearch(const std::vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return -1;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public class SearchAlgo {
    public static int BinarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.Length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```
{% endtab %}
{% endtabs %}

---

## 降维打击的威力：$O(\log n)$ 有多可怕？

每次查找，我们都能把剩下的数据“扔掉一半”。这就是数学上著名的对数级别时间复杂度：**$O(\log n)$**。

它到底有多强？
- 如果你在 1,000 个数字里找东西，最多只需要找 **10 次**（因为 $2^{10} = 1024$）。
- 如果你在 1,000,000 (一百万) 个数字里找东西，最多只需要找 **20 次**。
- 如果你在 4,000,000,000 (四十亿，全地球一半人口) 个人里精准找出某个人，最多只需要找 **32 次**！

**这就是为什么所有数据库系统（如 MySQL）、搜索引擎，底层都要拼命维持数据的“有序性”（或是构建有序的树结构）。只要数据有序，找起东西来简直犹如神助！**

---

# 全书结语 🏆

恭喜你！！当你阅读到这里并完全理解了上述代码时，你已经打通了这本《数据结构与算法基础》的全部关卡！

让我们回望一下你走过的漫漫长路：
1. **线性结构**：你学会了像排队一样的**数组**、互相牵手的**链表**，还有排队买票的**队列**和叠盘子的**栈**。
2. **树形结构**：你见识了枝繁叶茂的**二叉树**，学会了防止倾斜的**AVL树/红黑树**，掌握了能瞬间拿到最大值的**堆（优先队列）**，还用“递归”这种逆向思维征服了树的遍历。
3. **散列与图**：你领教了天下第一快的神奇**哈希表**，还在错综复杂的**无向/有向图**迷宫里，靠着 Visited 护身符全身而退。
4. **排序与查找**：你理解了怎么用冒泡、插入梳理乱序数据，更见识了分治思想下的**快排与归并**，最后以一招跨越亿万数据的**二分查找**完美收官。

**这些知识不再是死记硬背的代码，而是人类数十年来为了让世界运转得更高效，所沉淀下来的思想结晶。**
从今往后，别人看着代码，看到的是冰冷的英文字母；而你再看代码，看到的是内存里精密的结构、数据的跳动和算法的脉络。

**祝你在充满无限可能的计算机科学世界里，一路披荆斩棘，乘风破浪！**