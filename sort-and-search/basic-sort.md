# 基础排序算法 (Basic Sort)

在前面的三大篇章里，我们学习了如何把各式各样的数据“存”进电脑里（线性、树形、图状）。
但是，把数据存起来仅仅是第一步。人类最常做的事情是：**“在一堆乱七八糟的数据里，把它们按从大到小（或从小到大）排好队。”**

比如：淘宝按价格从低到高排序、按销量从高到低排序；学校按总分给学生排名。
这就是计算机科学中最古老、最经典的研究领域：**排序算法（Sorting Algorithms）**。

成百上千位天才科学家发明了几十种排序算法。我们会把它们分为两大类：
1. **基础排序 $O(n^2)$**：思路极其简单、像人类的本能，但处理海量数据时比较慢。
2. **进阶排序 $O(n \log n)$**：思路极其精妙，速度快如闪电，是工业界的绝对主力。

本章，我们先来学习最符合人类直觉的三个“基础排序”：**冒泡排序、选择排序、插入排序**。

---

## 一、冒泡排序 (Bubble Sort)

**思想来源**：水底下的气泡，越往上浮越大。
**具体做法**：从左到右，**专门比较相邻的两个数字**。如果左边比右边大（站错位置了），就把他俩**交换**。这样一路交换到队伍的最后面，全场**最大的那个胖子**，就一定会被挤到队伍的最末尾！第一轮结束，最大的搞定了；第二轮去搞定第二大的，以此类推。

### 冒泡排序的动画演示
假设我们要把 `[5, 1, 4, 2]` 从小到大排好：

**第一轮（定海神针，把最大的排到最后）：**
- `[5, 1, 4, 2]` -> 比较 5 和 1。5 比 1 大，**交换！** -> `[1, 5, 4, 2]`
- `[1, 5, 4, 2]` -> 比较 5 和 4。5 比 4 大，**交换！** -> `[1, 4, 5, 2]`
- `[1, 4, 5, 2]` -> 比较 5 和 2。5 比 2 大，**交换！** -> `[1, 4, 2, 5]`
第一轮结束！数字 `5` 就像最重的石头，已经沉到了队伍的最后面。

**第二轮（只需排前 3 个）：**
- `[1, 4, 2, 5]` -> 比较 1 和 4。正常，不交换。
- `[1, 4, 2, 5]` -> 比较 4 和 2。4 比 2 大，**交换！** -> `[1, 2, 4, 5]`
第二轮结束！次大的 `4` 排好了。后面以此类推。

{% tabs %}
{% tab title="Python" %}
```python
def bubble_sort(arr):
    n = len(arr)
    # 外层循环：表示需要走几轮。n个数只要排 n-1 轮即可。
    for i in range(n - 1):
        # 提前退出的优化：如果有一轮发现没有任何人交换位置，说明已经全排好了！
        swapped = False

        # 内层循环：专门负责相邻交换。
        # 为什么是 n-1-i？因为每一轮结束，后面就多了一个不需要看的大胖子。
        for j in range(n - 1 - i):
            if arr[j] > arr[j + 1]:
                # 交换左右两位
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True

        if not swapped:
            break # 优化提前结束

    return arr
```
{% endtab %}
{% tab title="Java" %}
```java
public void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        boolean swapped = false;

        // 每一轮把目前最大的数像冒泡一样挪到最后
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                // Java 中的交换大法
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        // 如果一整轮都没有人站错位置，说明大部队早就排好序了，光速下班！
        if (!swapped) break;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>
#include <algorithm> // 为了使用 std::swap

void bubbleSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]); // C++ 自带的优美交换函数
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public void BubbleSort(int[] arr) {
    int n = arr.Length;
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                // 交换
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```
{% endtab %}
{% endtabs %}

---

## 二、选择排序 (Selection Sort)

**思想来源**：农民伯伯挑苹果。
**具体做法**：如果你有一筐大小不一的苹果，你要怎么按从小到大排好？
很简单，你先把一整筐看一遍，**挑出那个绝对最小的苹果**，拿出来放在队伍的最左边第一个位置。
然后，你在剩下的筐里，再次看一遍，挑出剩下的苹果里最小的，放在队伍第二个位置。以此类推。

选择排序，就是每次都在“还没排好的乱军”之中，**死磕出一个最小的值**，然后把它和乱军中的头把交椅进行交换。

{% tabs %}
{% tab title="Python" %}
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        # 假设当前位置 i 上的数字就是最小的
        min_index = i

        # 让侦察兵 j 去后面的乱军里视察一圈
        for j in range(i + 1, n):
            if arr[j] < arr[min_index]:
                min_index = j # 卧槽，发现了一个比我还小的，记住他的坐标！

        # 视察完毕，让真命天子（最小的）和当前位置 i 的人交换
        arr[i], arr[min_index] = arr[min_index], arr[i]

    return arr
```
{% endtab %}
{% tab title="Java" %}
```java
public void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i; // 假设自己是最小的

        // 去后面找真正最小的
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j; // 记录真实最小值的索引
            }
        }

        // 把真实的最小值，换到他该待的位置 i 上
        int temp = arr[minIndex];
        arr[minIndex] = arr[i];
        arr[i] = temp;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>
#include <algorithm>

void selectionSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        std::swap(arr[i], arr[minIndex]);
    }
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public void SelectionSort(int[] arr) {
    int n = arr.Length;
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }

        int temp = arr[minIndex];
        arr[minIndex] = arr[i];
        arr[i] = temp;
    }
}
```
{% endtab %}
{% endtabs %}

---

## 三、插入排序 (Insertion Sort)

**思想来源**：平时斗地主抓牌时候的本能操作。
**具体做法**：假设你左手拿的牌已经是从小到大排好序的了，这时候你右手又抓起一张新牌，你会怎么做？
你肯定是从右往左对比左手里的牌，**一旦发现左手的牌比新牌大，就把左手的牌往右挪一挪腾位置**，直到找到一个适合新牌的位置，然后稳稳插进去！

> **这是基础排序里最有用的算法。在遇到“几乎已经排好序”的数组时，插入排序的速度无敌快，甚至快过那些大名鼎鼎的高级排序！**（很多高级编程语言底层，在处理短条数据时，都会偷偷切回插入排序）。

{% tabs %}
{% tab title="Python" %}
```python
def insertion_sort(arr):
    n = len(arr)
    # 假设手里第一张牌(i=0)是排好序的，我们从抓第二张牌(i=1)开始
    for i in range(1, n):
        key = arr[i] # 右手抓起的新牌
        j = i - 1    # j 指向左手里最右边的那张牌

        # 只要左手里的牌比新牌大，就把它往右推一格，腾位置
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        # 腾位完毕，把新牌插入坑中
        arr[j + 1] = key

    return arr
```
{% endtab %}
{% tab title="Java" %}
```java
public void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int key = arr[i]; // 右手里拿到的新牌
        int j = i - 1;    // 左手里已经排好序的牌的尽头

        // 往前扫描：如果是比我大的牌，统统往右移一步！
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        // 找到了合适的位置，落座
        arr[j + 1] = key;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>

void insertionSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public void InsertionSort(int[] arr) {
    int n = arr.Length;
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```
{% endtab %}
{% endtabs %}

## 本章小结：基础排序三兄弟的绝代风华

| 算法名称 | 现实生活比喻 | 平均时间复杂度 | 核心特性 |
| :--- | :--- | :---: | :--- |
| **冒泡排序** | 水里泡泡上浮 | $O(n^2)$ | 太慢了，主要用于教学和演示“交换”思维。 |
| **选择排序** | 挑一筐苹果中最小的 | $O(n^2)$ | 表现最死板！即使数组早就排好序了，它还是得傻傻地遍历寻找。由于它的交换次数是最少的，有时候在极其特殊的“交换成本极大（比如挪动大型物理设备）”的极端场景有点用。 |
| **插入排序** | 斗地主抓牌插牌 | $O(n^2)$ | **王者级 O(n²) 算法**！如果你给它一个“近乎完全有序”的数组，它的内层循环几乎不触发，速度疯狂飙升至梦幻般的 $O(n)$。它是大厂工业界排序底层的**核心兜底策略**。 |

学完了这三个靠“人类直觉”就能写出来的排序算法，你可能迫不及待想知道：**那互联网大厂真正处理几千万条海量数据时，用的是什么神仙算法呢？**

下一章，我们将介绍排序算法史上的巅峰之作、占据几乎所有高级编程语言底层框架的**“进阶排序”**：靠着将全军劈成两半大杀四方的 **快速排序（Quick Sort）** 和 **归并排序（Merge Sort）**！