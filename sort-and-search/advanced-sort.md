# 进阶排序算法 (Advanced Sort)

在上一章中，我们学习了冒泡、选择和插入这三个基础排序算法。它们的平均时间复杂度都是 **$O(n^2)$**。
$n^2$ 是个什么概念呢？如果只有 100 个人排队，那需要操作 $100 \times 100 = 1万$ 次，电脑眨眼间就能跑完。
但如果淘宝要给 1 亿件商品按价格排序呢？$1亿 \times 1亿 = 1亿亿$ 次计算！哪怕是如今配置顶级的服务器，也会算到冒烟直接卡死。

为了解决海量数据的排序灾难，计算机科学家祭出了算法设计中最伟大的思想之一：**分而治之（Divide and Conquer）**。
我们不再把 1 亿件商品混在一起排，而是把它们劈成两半，各排各的；如果一半还是太多，那就再劈成两半……一直劈到只剩一两个数字，排好了再把它们合拼起来。

利用这种思想，科学家们把排序的时间复杂度**从暴力的 $O(n^2)$ 强行降维打击到了极速的 $O(n \log n)$**。
这一章，我们就来学习统治着现代工业底层的两大王牌排序法：**归并排序** 与 **快速排序**。

---

## 一、归并排序 (Merge Sort)

**生活场景比喻**：
想象你是一名老师，正在批改全班 64 份卷子，你想把它们按分数排好。如果你一个人看，会被累瞎。
你可以这么做：
1. 把 64 份卷子**撕成两半（分）**，丢给你手下的左右两个班长：“你们俩各去排好手中的 32 份！”
2. 左右班长觉得 32 份也太多了，继续**撕成两半（分）**分给下面的小组长。
3. 这样一路分发下去，直到每个组长手里**只有 1 份卷子**。1 份卷子还需要排吗？天然就是排好的。
4. 然后，各个组长开始两两拿着自己排好的那一叠卷子，把两张小桌子拼在一起，**抽出两叠卷子中最上面（最小）的那一张（治 / 合并）**，慢慢合成一叠排好序的大卷子，一路向上**合并**，最终到你手里，64 份卷子就全排好了！

**算法核心**：**拆到底，然后再合并两个“已经各自排好序”的数组（利用双指针法）。**

{% tabs %}
{% tab title="Python" %}
```python
def merge_sort(arr):
    # 递归的终点：如果被劈到只剩不到2个元素，自然是有序的，直接返回
    if len(arr) < 2:
        return arr

    # 1. 分 (Divide)：找到中间位置，把数组劈成两半
    mid = len(arr) // 2
    left_half = arr[:mid]
    right_half = arr[mid:]

    # 2. 也是分 (Divide)：让左右两半各自去排好序（相信递归的魔力）
    left_sorted = merge_sort(left_half)
    right_sorted = merge_sort(right_half)

    # 3. 治 (Merge)：把两叠已经排好序的卷子，合并成一叠
    return merge(left_sorted, right_sorted)

def merge(left, right):
    result = []
    i = 0 # 指向左边那叠卷子最顶上的指针
    j = 0 # 指向右边那叠卷子最顶上的指针

    # 只要两边都有卷子，就比较最顶上的，谁小抽谁
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    # 如果有一叠抽完了，直接把另一叠剩下的全搬过来
    result.extend(left[i:])
    result.extend(right[j:])

    return result
```
{% endtab %}
{% tab title="Java" %}
```java
public class MergeSort {
    public static void mergeSort(int[] arr) {
        if (arr == null || arr.length < 2) return;
        int[] temp = new int[arr.length]; // 开辟一个辅助教室（内存）用来合并数据
        sort(arr, 0, arr.length - 1, temp);
    }

    private static void sort(int[] arr, int left, int right, int[] temp) {
        if (left >= right) return;

        int mid = left + (right - left) / 2;

        // 1. 各自从中间劈开去排
        sort(arr, left, mid, temp);
        sort(arr, mid + 1, right, temp);

        // 2. 合并两叠卷子
        merge(arr, left, mid, right, temp);
    }

    private static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left;      // 左卷子堆的指针
        int j = mid + 1;   // 右卷子堆的指针
        int t = 0;         // 辅助临时数组的指针

        // 比较两堆卷子最上面那个，谁小抽谁
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[t++] = arr[i++];
            } else {
                temp[t++] = arr[j++];
            }
        }

        // 把没抽完的那一堆直接全部摞进去
        while (i <= mid) temp[t++] = arr[i++];
        while (j <= right) temp[t++] = arr[j++];

        // 把排好序的拿回给原教室（原数组）
        t = 0;
        while (left <= right) {
            arr[left++] = temp[t++];
        }
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>

void merge(std::vector<int>& arr, int left, int mid, int right) {
    std::vector<int> temp(right - left + 1);
    int i = left;
    int j = mid + 1;
    int k = 0;

    // 两边都有人排队时，谁小谁先进 temp
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
        }
    }

    // 肯定有一边提前走空了，把剩下的全拉进去
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];

    // 覆盖回原数组
    for (int p = 0; p < temp.size(); p++) {
        arr[left + p] = temp[p];
    }
}

void mergeSortHelper(std::vector<int>& arr, int left, int right) {
    if (left >= right) return;

    int mid = left + (right - left) / 2;
    mergeSortHelper(arr, left, mid);
    mergeSortHelper(arr, mid + 1, right);

    // 合并
    merge(arr, left, mid, right);
}

void mergeSort(std::vector<int>& arr) {
    if (arr.size() < 2) return;
    mergeSortHelper(arr, 0, arr.size() - 1);
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public class MergeSortAlgo {
    public static void MergeSort(int[] arr) {
        if (arr == null || arr.Length < 2) return;
        int[] temp = new int[arr.Length];
        Sort(arr, 0, arr.Length - 1, temp);
    }

    private static void Sort(int[] arr, int left, int right, int[] temp) {
        if (left >= right) return;

        int mid = left + (right - left) / 2;
        Sort(arr, left, mid, temp);
        Sort(arr, mid + 1, right, temp);

        Merge(arr, left, mid, right, temp);
    }

    private static void Merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left;
        int j = mid + 1;
        int t = 0;

        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[t++] = arr[i++];
            } else {
                temp[t++] = arr[j++];
            }
        }

        while (i <= mid) temp[t++] = arr[i++];
        while (j <= right) temp[t++] = arr[j++];

        t = 0;
        while (left <= right) {
            arr[left++] = temp[t++];
        }
    }
}
```
{% endtab %}
{% endtabs %}

**归并排序的致命缺点**：因为你需要“拼桌子来合并两堆卷子”，所以你**必须开辟一块和原数据差不多一样大的新内存（叫做临时数组 temp）**。它的空间复杂度是 $O(n)$。在内存极度吃紧的嵌入式设备上，这点很致命。

有没有不用额外花这么多内存的极速神仙算法呢？有，那就是赫赫有名的……

---

## 二、快速排序 (Quick Sort)

发明人：Tony Hoare（图灵奖得主，他因为这个算法拿了奖）。
快速排序霸占了长达几十年“20世纪十大算法”的前十把交椅，几乎所有语言核心库内置的 `sort()` 排序（比如 C++ 的 `std::sort`，Java 和 C# 对基本数据类型的排序），底层都在使用它的进阶版。

**生活场景比喻**：
全校操场上有几百号人乱哄哄地站成一排，校长想要让他们从小到大排好。
1. 校长懒得一个个比，他**随便揪出一个人（比如抓了排在队尾的小明），当作“标杆 / 基准 (Pivot)”**。
2. 校长拿着大喇叭喊：“**所有个子比小明矮的，统统站到小明的左边去！所有个子比小明高的，统统站到小明右边去！**”
3. 哗啦啦一阵骚动，队伍分成了两派。小明站在正中间。
4. 校长说：“好了，小明现在就**永远定格在这个位置，他绝对是站对地方了。**”
5. 接着，左半边比小明矮的那堆人，选个新标杆继续上面这套喊话；右半边也选个新标杆喊话。
6. 喊着喊着，所有人都不可撼动地“定”在了属于自己的位置上，队伍就天然排好了！

**算法核心**：**找基准（Pivot），然后在原数组里原地交换（Partition 化块），确保左小右大。**
因为完全是在队伍里原地让大家换位置，不需要额外开辟空教室（除了递归调用自身产生的非常少量的结构内存 $O(\log n)$），所以极其节省内存！

{% tabs %}
{% tab title="Python" %}
```python
def quick_sort(arr):
    # 递归思想，但是为了能够"原地"操作省内存，通常写一个辅助函数携带左右边界
    quick_sort_helper(arr, 0, len(arr) - 1)
    return arr

def quick_sort_helper(arr, low, high):
    if low < high:
        # partition 功能：找出校长喊话后，"小明(基准)"确定定格的绝对位置
        pivot_index = partition(arr, low, high)

        # 让左半边的乱军继续玩这个游戏
        quick_sort_helper(arr, low, pivot_index - 1)
        # 让右半边的乱军继续玩这个游戏
        quick_sort_helper(arr, pivot_index + 1, high)

def partition(arr, low, high):
    # 我们选队伍最后一个同学作为"基准/标杆 (Pivot)"
    pivot = arr[high]

    # i 就像一道区分墙，它的左边全是比标杆矮的人
    i = low - 1

    for j in range(low, high):
        # 只要发现有人比标杆矮，就把这堵墙往右推一格，把他拉进矮子阵营
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i] # 两人交换位置

    # 视察完毕，把一开始躲在最后面的"标杆"，安插进这堵墙的正中间
    arr[i + 1], arr[high] = arr[high], arr[i + 1]

    # 返回标杆被安插的绝对位置
    return i + 1
```
{% endtab %}
{% tab title="Java" %}
```java
public class QuickSort {
    public static void quickSort(int[] arr) {
        if (arr == null || arr.length == 0) return;
        sort(arr, 0, arr.length - 1);
    }

    private static void sort(int[] arr, int low, int high) {
        if (low < high) {
            // 利用 partition 操作把队伍撕成比标杆大和小的两部分，拿到标杆定格的位置
            int pivotIndex = partition(arr, low, high);

            sort(arr, low, pivotIndex - 1);  // 左半边继续排
            sort(arr, pivotIndex + 1, high); // 右半边继续排
        }
    }

    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // 选用最后一个作为基准
        int i = low - 1;       // i 代表小于区的右边界

        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                // 交换 arr[i] 和 arr[j]
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }

        // 最后把基准自己安插到比他小的队伍后面
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;

        return i + 1; // 返回基准最后停留的安全屋位置
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>
#include <algorithm>

int partition(std::vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            std::swap(arr[i], arr[j]);
        }
    }
    std::swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSortHelper(std::vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSortHelper(arr, low, pi - 1);
        quickSortHelper(arr, pi + 1, high);
    }
}

void quickSort(std::vector<int>& arr) {
    if (arr.empty()) return;
    quickSortHelper(arr, 0, arr.size() - 1);
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public class QuickSortAlgo {
    public static void QuickSort(int[] arr) {
        if (arr == null || arr.Length == 0) return;
        Sort(arr, 0, arr.Length - 1);
    }

    private static void Sort(int[] arr, int low, int high) {
        if (low < high) {
            int pi = Partition(arr, low, high);
            Sort(arr, low, pi - 1);
            Sort(arr, pi + 1, high);
        }
    }

    private static int Partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;

        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                int t = arr[i];
                arr[i] = arr[j];
                arr[j] = t;
            }
        }

        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;

        return i + 1;
    }
}
```
{% endtab %}
{% endtabs %}

### 快排的阿喀琉斯之踵 (致命隐患)
快排几乎在各种语言的实测中都是全场最快的王。但是它有一个要命的缺陷：**如果你挑选的那个“标杆（Pivot）”每次恰好都是全校最高（或最矮）的傻大个！**
例如，数组本来就已经是排好序的，你每次都选最后一个人当标杆，那“校长喊话”起不到任何平分队伍的作用，快排的长项（分治劈把巨阵劈成两半）瞬间报废，它会立马退化成最烂的 **$O(n^2)$**。
为了防止这种情况，工业界的大神们通常不会每次都死板地选最后一个当标杆，而是使用**“随机从人群里抓一个”**或者**“抽头尾和中间的三个人，选身高在中间的那个作为标杆（三数取中）”**的招数来避坑。

## 本章小结：神仙斗法

| 算法名称 | 核心思想 | 平均速度 | 最坏速度 | 空间 / 内存 | 优点与缺点 |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **归并排序<br>(Merge Sort)** | 劈两半，排好后合二为一 | $O(n \log n)$ | $O(n \log n)$ | $O(n)$ | **优点**：稳如泰山，无论输入有多烂它都极快且坚不可摧。<br>**缺点**：需要额外开辟大量内存装辅助数组 temp。 |
| **快速排序<br>(Quick Sort)** | 选标杆，小的站左大的站右 | $O(n \log n)$ | $O(n^2)$ | $O(\log n)$ | **优点**：常数极小，绝大部分情况下的“地球最快飞人”，且极其节省内存空间 (原地 In-place)。<br>**缺点**：如果数据极为倒霉且故意针对，可能退化拖慢速度。 |

当你读到这里时，恭喜你已经征服了计算机算法领域最美妙的**分治递归思想**！这也意味着这本以《数据结构与算法基础》为核心的 GitBook 已经迎来了尾声。

接下来，我们将用非常轻松的心情，了解一下整本指南的最后一个防线：**《查找算法（著名的二分查找）》**。