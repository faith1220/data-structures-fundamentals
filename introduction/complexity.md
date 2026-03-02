# 算法分析基础

## 什么是算法

算法（Algorithm）是解决特定问题的一系列明确步骤。可以把它理解为一份"操作说明书"：给定输入，按照规定的步骤执行，最终得到预期的输出。

一个合格的算法需要满足以下五个特性：

1. **有穷性** — 算法必须在有限步骤内结束，不能无限循环
2. **确定性** — 每一步操作都有明确的含义，不存在歧义
3. **可行性** — 每一步都能在有限时间内完成
4. **输入** — 有零个或多个输入
5. **输出** — 有一个或多个输出

同一个问题往往有多种算法可以解决。比如对一组数据排序，可以用冒泡排序、快速排序、归并排序等不同方法。这些方法都能得到正确结果，但效率可能天差地别。如何衡量算法的效率，就是本章要解决的问题。

## 为什么需要复杂度分析

最直接的想法是：把两个算法都跑一遍，比较谁用的时间更短。这种方法叫做事后统计法，但它有明显的缺陷：

- 结果依赖于硬件环境，同一个算法在不同机器上的运行时间不同
- 结果依赖于数据规模和数据特征，测试数据不同，结论可能不同
- 必须把算法完整实现出来才能比较，成本太高

因此，我们需要一种**与具体机器和数据无关**的方法，从理论层面评估算法的效率。这就是复杂度分析。

## 时间复杂度

### 基本思路

时间复杂度衡量的不是算法运行的绝对时间，而是**随着输入规模增长，算法执行的操作次数的增长趋势**。

我们用 n 表示输入数据的规模。考察以下代码：

{% tabs %}
{% tab title="Python" %}
```python
def sum_of_list(arr):
    total = 0            # 执行 1 次
    for x in arr:        # 循环 n 次
        total += x       # 每次循环执行 1 次
    return total         # 执行 1 次
```
{% endtab %}
{% tab title="Java" %}
```java
public static int sumOfList(int[] arr) {
    int total = 0;               // 执行 1 次
    for (int x : arr) {          // 循环 n 次
        total += x;              // 每次循环执行 1 次
    }
    return total;                // 执行 1 次
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
int sumOfList(const vector<int>& arr) {
    int total = 0;               // 执行 1 次
    for (int x : arr) {          // 循环 n 次
        total += x;              // 每次循环执行 1 次
    }
    return total;                // 执行 1 次
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static int SumOfList(int[] arr) {
    int total = 0;               // 执行 1 次
    foreach (int x in arr) {     // 循环 n 次
        total += x;              // 每次循环执行 1 次
    }
    return total;                // 执行 1 次
}
```
{% endtab %}
{% endtabs %}

总执行次数为 1 + n + 1 = n + 2。当 n 足够大时，常数 2 的影响可以忽略不计，我们只关心增长最快的那一项。这就引出了大 O 记号。

### 大 O 记号

大 O 记号（Big-O Notation）描述的是算法执行时间的**上界**，即最坏情况下的增长趋势。它的规则很简单：

1. 只保留最高阶项
2. 去掉最高阶项的系数

例如：

| 执行次数表达式 | 大 O 记号 |
| --- | --- |
| 3 | O(1) |
| 2n + 5 | O(n) |
| 3n² + 2n + 1 | O(n²) |
| 5·2ⁿ + n³ | O(2ⁿ) |

### 常见时间复杂度

以下按照从快到慢的顺序，列出最常见的时间复杂度等级：

| 复杂度 | 名称 | 示例场景 |
| --- | --- | --- |
| O(1) | 常数阶 | 数组按下标访问 |
| O(log n) | 对数阶 | 二分查找 |
| O(n) | 线性阶 | 遍历数组 |
| O(n log n) | 线性对数阶 | 归并排序、快速排序 |
| O(n²) | 平方阶 | 冒泡排序、选择排序 |
| O(2ⁿ) | 指数阶 | 穷举所有子集 |
| O(n!) | 阶乘阶 | 穷举所有排列 |

当 n = 100000 时，O(n) 的算法执行约十万次操作，而 O(n²) 的算法执行约一百亿次操作。这就是为什么复杂度分析如此重要。

### 分析时间复杂度的方法

掌握以下三条规则，就能应对大部分场景：

**规则一：循环次数决定复杂度**

{% tabs %}
{% tab title="Python" %}
```python
# O(n)：单层循环
for i in range(n):
    print(i)

# O(n²)：双层嵌套循环
for i in range(n):
    for j in range(n):
        print(i, j)

# O(n³)：三层嵌套循环
for i in range(n):
    for j in range(n):
        for k in range(n):
            print(i, j, k)
```
{% endtab %}
{% tab title="Java" %}
```java
// O(n)：单层循环
for (int i = 0; i < n; i++) {
    System.out.println(i);
}

// O(n²)：双层嵌套循环
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        System.out.println(i + " " + j);
    }
}

// O(n³)：三层嵌套循环
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        for (int k = 0; k < n; k++) {
            System.out.println(i + " " + j + " " + k);
        }
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
// O(n)：单层循环
for (int i = 0; i < n; i++) {
    cout << i << endl;
}

// O(n²)：双层嵌套循环
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        cout << i << " " << j << endl;
    }
}

// O(n³)：三层嵌套循环
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        for (int k = 0; k < n; k++) {
            cout << i << " " << j << " " << k << endl;
        }
    }
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
// O(n)：单层循环
for (int i = 0; i < n; i++) {
    Console.WriteLine(i);
}

// O(n²)：双层嵌套循环
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        Console.WriteLine($"{i} {j}");
    }
}

// O(n³)：三层嵌套循环
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        for (int k = 0; k < n; k++) {
            Console.WriteLine($"{i} {j} {k}");
        }
    }
}
```
{% endtab %}
{% endtabs %}

**规则二：顺序执行取最大值**

如果代码有多个独立的部分，总复杂度取其中最大的那个。

{% tabs %}
{% tab title="Python" %}
```python
# 第一部分：O(n)
for i in range(n):
    print(i)

# 第二部分：O(n²)
for i in range(n):
    for j in range(n):
        print(i, j)

# 总复杂度：O(n²)
```
{% endtab %}
{% tab title="Java" %}
```java
// 第一部分：O(n)
for (int i = 0; i < n; i++) {
    System.out.println(i);
}

// 第二部分：O(n²)
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        System.out.println(i + " " + j);
    }
}

// 总复杂度：O(n²)
```
{% endtab %}
{% tab title="C++" %}
```cpp
// 第一部分：O(n)
for (int i = 0; i < n; i++) {
    cout << i << endl;
}

// 第二部分：O(n²)
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        cout << i << " " << j << endl;
    }
}

// 总复杂度：O(n²)
```
{% endtab %}
{% tab title="C#" %}
```csharp
// 第一部分：O(n)
for (int i = 0; i < n; i++) {
    Console.WriteLine(i);
}

// 第二部分：O(n²)
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        Console.WriteLine($"{i} {j}");
    }
}

// 总复杂度：O(n²)
```
{% endtab %}
{% endtabs %}

**规则三：每次规模减半是 O(log n)**

如果每次循环都将问题规模缩小一半，复杂度就是 O(log n)。

{% tabs %}
{% tab title="Python" %}
```python
# O(log n)
i = n
while i > 1:
    i = i // 2
```
{% endtab %}
{% tab title="Java" %}
```java
// O(log n)
int i = n;
while (i > 1) {
    i = i / 2;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
// O(log n)
int i = n;
while (i > 1) {
    i = i / 2;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
// O(log n)
int i = n;
while (i > 1) {
    i = i / 2;
}
```
{% endtab %}
{% endtabs %}

n 需要除以多少次 2 才能变为 1？答案是 log₂n 次。

### 最好、最坏与平均情况

同一个算法在不同输入下，执行次数可能不同。以在数组中查找某个值为例：

{% tabs %}
{% tab title="Python" %}
```python
def find(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```
{% endtab %}
{% tab title="Java" %}
```java
public static int find(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
int find(const vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static int Find(int[] arr, int target) {
    for (int i = 0; i < arr.Length; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}
```
{% endtab %}
{% endtabs %}

- **最好情况**（Best Case）：目标恰好在第一个位置，O(1)
- **最坏情况**（Worst Case）：目标在最后一个位置或不存在，O(n)
- **平均情况**（Average Case）：平均需要查找 n/2 次，O(n)

在实际分析中，我们通常关注**最坏情况**，因为它给出了性能的保底承诺。

## 空间复杂度

空间复杂度衡量的是算法在运行过程中**额外占用的内存空间**随输入规模的增长趋势。注意，输入数据本身占用的空间不计入。

### 常见空间复杂度

**O(1) — 常数空间**

只使用固定数量的变量，与输入规模无关。

{% tabs %}
{% tab title="Python" %}
```python
def sum_of_list(arr):
    total = 0
    for x in arr:
        total += x
    return total
# 只用了一个额外变量 total，空间复杂度 O(1)
```
{% endtab %}
{% tab title="Java" %}
```java
public static int sumOfList(int[] arr) {
    int total = 0;
    for (int x : arr) {
        total += x;
    }
    return total;
    // 只用了一个额外变量 total，空间复杂度 O(1)
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
int sumOfList(const vector<int>& arr) {
    int total = 0;
    for (int x : arr) {
        total += x;
    }
    return total;
    // 只用了一个额外变量 total，空间复杂度 O(1)
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static int SumOfList(int[] arr) {
    int total = 0;
    foreach (int x in arr) {
        total += x;
    }
    return total;
    // 只用了一个额外变量 total，空间复杂度 O(1)
}
```
{% endtab %}
{% endtabs %}

**O(n) — 线性空间**

额外使用了一个与输入规模同量级的数据结构。

{% tabs %}
{% tab title="Python" %}
```python
def copy_list(arr):
    new_arr = []
    for x in arr:
        new_arr.append(x)
    return new_arr
# 创建了一个同样大小的新数组，空间复杂度 O(n)
```
{% endtab %}
{% tab title="Java" %}
```java
public static int[] copyList(int[] arr) {
    int[] newArr = new int[arr.length];
    for (int i = 0; i < arr.length; i++) {
        newArr[i] = arr[i];
    }
    return newArr;
    // 创建了一个同样大小的新数组，空间复杂度 O(n)
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
vector<int> copyList(const vector<int>& arr) {
    vector<int> newArr;
    for (int x : arr) {
        newArr.push_back(x);
    }
    return newArr;
    // 创建了一个同样大小的新数组，空间复杂度 O(n)
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static int[] CopyList(int[] arr) {
    int[] newArr = new int[arr.Length];
    for (int i = 0; i < arr.Length; i++) {
        newArr[i] = arr[i];
    }
    return newArr;
    // 创建了一个同样大小的新数组，空间复杂度 O(n)
}
```
{% endtab %}
{% endtabs %}

**O(n²) — 平方空间**

例如创建一个 n×n 的二维矩阵。

{% tabs %}
{% tab title="Python" %}
```python
def create_matrix(n):
    matrix = []
    for i in range(n):
        row = [0] * n
        matrix.append(row)
    return matrix
# 空间复杂度 O(n²)
```
{% endtab %}
{% tab title="Java" %}
```java
public static int[][] createMatrix(int n) {
    int[][] matrix = new int[n][n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = 0;
        }
    }
    return matrix;
    // 空间复杂度 O(n²)
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
vector<vector<int>> createMatrix(int n) {
    vector<vector<int>> matrix(n, vector<int>(n, 0));
    return matrix;
    // 空间复杂度 O(n²)
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static int[,] CreateMatrix(int n) {
    int[,] matrix = new int[n, n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i, j] = 0;
        }
    }
    return matrix;
    // 空间复杂度 O(n²)
}
```
{% endtab %}
{% endtabs %}

### 时间与空间的权衡

在实际开发中，时间复杂度和空间复杂度往往不能同时达到最优，需要根据场景做取舍。这就是所谓的"以空间换时间"或"以时间换空间"。

一个典型的例子：判断数组中是否有重复元素。

{% tabs %}
{% tab title="Python" %}
```python
# 方案一：双重循环，时间 O(n²)，空间 O(1)
def has_duplicate_v1(arr):
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            if arr[i] == arr[j]:
                return True
    return False

# 方案二：用集合记录，时间 O(n)，空间 O(n)
def has_duplicate_v2(arr):
    seen = set()
    for x in arr:
        if x in seen:
            return True
        seen.add(x)
    return False
```
{% endtab %}
{% tab title="Java" %}
```java
// 方案一：双重循环，时间 O(n²)，空间 O(1)
public static boolean hasDuplicateV1(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        for (int j = i + 1; j < arr.length; j++) {
            if (arr[i] == arr[j]) {
                return true;
            }
        }
    }
    return false;
}

// 方案二：用集合记录，时间 O(n)，空间 O(n)
public static boolean hasDuplicateV2(int[] arr) {
    HashSet<Integer> seen = new HashSet<>();
    for (int x : arr) {
        if (seen.contains(x)) {
            return true;
        }
        seen.add(x);
    }
    return false;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
// 方案一：双重循环，时间 O(n²)，空间 O(1)
bool hasDuplicateV1(const vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        for (int j = i + 1; j < arr.size(); j++) {
            if (arr[i] == arr[j]) {
                return true;
            }
        }
    }
    return false;
}

// 方案二：用集合记录，时间 O(n)，空间 O(n)
bool hasDuplicateV2(const vector<int>& arr) {
    unordered_set<int> seen;
    for (int x : arr) {
        if (seen.count(x)) {
            return true;
        }
        seen.insert(x);
    }
    return false;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
// 方案一：双重循环，时间 O(n²)，空间 O(1)
public static bool HasDuplicateV1(int[] arr) {
    for (int i = 0; i < arr.Length; i++) {
        for (int j = i + 1; j < arr.Length; j++) {
            if (arr[i] == arr[j]) {
                return true;
            }
        }
    }
    return false;
}

// 方案二：用集合记录，时间 O(n)，空间 O(n)
public static bool HasDuplicateV2(int[] arr) {
    HashSet<int> seen = new HashSet<int>();
    foreach (int x in arr) {
        if (seen.Contains(x)) {
            return true;
        }
        seen.Add(x);
    }
    return false;
}
```
{% endtab %}
{% endtabs %}

方案二用 O(n) 的额外空间换取了从 O(n²) 到 O(n) 的时间提升。在数据量较大时，这种权衡通常是值得的。

## 本章小结

- 算法是解决问题的有限步骤序列，需要满足有穷性、确定性等基本特性
- 时间复杂度用大 O 记号描述算法执行时间随输入规模的增长趋势
- 常见复杂度从快到慢依次为：O(1)、O(log n)、O(n)、O(n log n)、O(n²)、O(2ⁿ)
- 空间复杂度衡量算法额外占用内存的增长趋势
- 时间和空间往往需要权衡取舍，具体选择取决于实际场景
