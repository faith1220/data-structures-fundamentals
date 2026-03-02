# 数组与动态数组

## 什么是数组

数组（Array）是最基本、最常用的数据结构。它将一组**相同类型**的元素存储在一段**连续的内存空间**中，通过下标（Index）来访问每个元素。

可以把数组想象成一排编了号的储物柜。每个柜子大小相同，柜子上的编号从 0 开始递增。只要知道编号，就能直接打开对应的柜子，不需要从头逐个查找。

```
下标:   0    1    2    3    4
      +----+----+----+----+----+
值:   | 10 | 20 | 30 | 40 | 50 |
      +----+----+----+----+----+
```

## 数组的内存模型

数组之所以能够通过下标直接访问元素，关键在于它的内存是连续分配的。假设数组的起始地址为 `base`，每个元素占用 `size` 个字节，那么第 `i` 个元素的地址为：

```
address(i) = base + i * size
```

这个计算是常数时间的，因此数组的随机访问时间复杂度为 O(1)。这是数组最大的优势。

## 数组的基本操作

### 访问元素

通过下标直接访问，时间复杂度 O(1)。

{% tabs %}
{% tab title="Python" %}
```python
arr = [10, 20, 30, 40, 50]
print(arr[2])  # 输出 30
```
{% endtab %}
{% tab title="Java" %}
```java
int[] arr = {10, 20, 30, 40, 50};
System.out.println(arr[2]);  // 输出 30
```
{% endtab %}
{% tab title="C++" %}
```cpp
int arr[] = {10, 20, 30, 40, 50};
std::cout << arr[2] << std::endl;  // 输出 30
```
{% endtab %}
{% tab title="C#" %}
```csharp
int[] arr = {10, 20, 30, 40, 50};
Console.WriteLine(arr[2]);  // 输出 30
```
{% endtab %}
{% endtabs %}

### 查找元素

如果不知道元素的下标，只能从头到尾逐个比较，时间复杂度 O(n)。

{% tabs %}
{% tab title="Python" %}
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```
{% endtab %}
{% tab title="Java" %}
```java
public static int linearSearch(int[] arr, int target) {
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
int linearSearch(int arr[], int size, int target) {
    for (int i = 0; i < size; i++) {
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
public static int LinearSearch(int[] arr, int target) {
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

如果数组是有序的，可以使用二分查找将时间复杂度降低到 O(log n)，这部分内容会在后续的[查找算法](../sort-and-search/search.md)章节中详细讨论。

### 插入元素

在数组末尾插入元素很简单，时间复杂度 O(1)。但如果要在中间某个位置插入，就需要把该位置及之后的所有元素向后移动一位，为新元素腾出空间。

{% tabs %}
{% tab title="Python" %}
```python
# 在下标 2 的位置插入 25
arr = [10, 20, 30, 40, 50]

# 需要将 30, 40, 50 依次后移
# 移动前: [10, 20, 30, 40, 50, __]
# 移动后: [10, 20, __, 30, 40, 50]
# 插入后: [10, 20, 25, 30, 40, 50]
```
{% endtab %}
{% tab title="Java" %}
```java
// 在下标 2 的位置插入 25
int[] arr = {10, 20, 30, 40, 50};

// 需要将 30, 40, 50 依次后移
// 移动前: [10, 20, 30, 40, 50, __]
// 移动后: [10, 20, __, 30, 40, 50]
// 插入后: [10, 20, 25, 30, 40, 50]
```
{% endtab %}
{% tab title="C++" %}
```cpp
// 在下标 2 的位置插入 25
int arr[] = {10, 20, 30, 40, 50};

// 需要将 30, 40, 50 依次后移
// 移动前: [10, 20, 30, 40, 50, __]
// 移动后: [10, 20, __, 30, 40, 50]
// 插入后: [10, 20, 25, 30, 40, 50]
```
{% endtab %}
{% tab title="C#" %}
```csharp
// 在下标 2 的位置插入 25
int[] arr = {10, 20, 30, 40, 50};

// 需要将 30, 40, 50 依次后移
// 移动前: [10, 20, 30, 40, 50, __]
// 移动后: [10, 20, __, 30, 40, 50]
// 插入后: [10, 20, 25, 30, 40, 50]
```
{% endtab %}
{% endtabs %}

最坏情况下（在头部插入），需要移动全部 n 个元素，时间复杂度 O(n)。

### 删除元素

与插入类似，删除某个位置的元素后，需要把后续元素依次前移来填补空缺。

{% tabs %}
{% tab title="Python" %}
```python
# 删除下标 1 的元素
arr = [10, 20, 30, 40, 50]

# 删除前: [10, 20, 30, 40, 50]
# 前移后: [10, 30, 40, 50, __]
```
{% endtab %}
{% tab title="Java" %}
```java
// 删除下标 1 的元素
int[] arr = {10, 20, 30, 40, 50};

// 删除前: [10, 20, 30, 40, 50]
// 前移后: [10, 30, 40, 50, __]
```
{% endtab %}
{% tab title="C++" %}
```cpp
// 删除下标 1 的元素
int arr[] = {10, 20, 30, 40, 50};

// 删除前: [10, 20, 30, 40, 50]
// 前移后: [10, 30, 40, 50, __]
```
{% endtab %}
{% tab title="C#" %}
```csharp
// 删除下标 1 的元素
int[] arr = {10, 20, 30, 40, 50};

// 删除前: [10, 20, 30, 40, 50]
// 前移后: [10, 30, 40, 50, __]
```
{% endtab %}
{% endtabs %}

最坏情况下（删除头部元素），时间复杂度 O(n)。

### 操作复杂度汇总

| 操作 | 时间复杂度 | 说明 |
| --- | --- | --- |
| 按下标访问 | O(1) | 直接计算地址 |
| 查找（无序） | O(n) | 需要逐个比较 |
| 查找（有序） | O(log n) | 二分查找 |
| 尾部插入 | O(1) | 直接追加 |
| 中间/头部插入 | O(n) | 需要移动元素 |
| 尾部删除 | O(1) | 直接移除 |
| 中间/头部删除 | O(n) | 需要移动元素 |

## 静态数组与动态数组

### 静态数组

在 C、Java 等语言中，数组在创建时就必须指定大小，且大小在之后不能改变。这种数组称为静态数组（Static Array）。

{% tabs %}
{% tab title="Python" %}
```python
# Python 没有真正的静态数组，列表本身就是动态的
# 可以用固定长度的列表来模拟静态数组
arr = [10, 20, 30, 40, 50]
```
{% endtab %}
{% tab title="Java" %}
```java
// Java：创建一个大小为 5 的整型数组
int[] arr = {10, 20, 30, 40, 50};
```
{% endtab %}
{% tab title="C++" %}
```cpp
// C++：创建一个大小为 5 的整型数组
int arr[5] = {10, 20, 30, 40, 50};
```
{% endtab %}
{% tab title="C#" %}
```csharp
// C#：创建一个大小为 5 的整型数组
int[] arr = {10, 20, 30, 40, 50};
```
{% endtab %}
{% endtabs %}

静态数组的问题很明显：如果事先不知道要存多少数据，分配少了不够用，分配多了浪费空间。

### 动态数组

动态数组（Dynamic Array）解决了这个问题。它在内部维护一个静态数组，当元素数量超过当前容量时，自动分配一块更大的内存空间，将原有数据复制过去，然后释放旧空间。

Python 的 `list`、Java 的 `ArrayList`、C++ 的 `std::vector` 都是动态数组的实现。

{% tabs %}
{% tab title="Python" %}
```python
# Python 的 list 就是动态数组
arr = []
arr.append(10)  # [10]
arr.append(20)  # [10, 20]
arr.append(30)  # [10, 20, 30]
# 不需要预先指定大小，按需增长
```
{% endtab %}
{% tab title="Java" %}
```java
// Java 的 ArrayList 就是动态数组
ArrayList<Integer> arr = new ArrayList<>();
arr.add(10);  // [10]
arr.add(20);  // [10, 20]
arr.add(30);  // [10, 20, 30]
// 不需要预先指定大小，按需增长
```
{% endtab %}
{% tab title="C++" %}
```cpp
// C++ 的 std::vector 就是动态数组
std::vector<int> arr;
arr.push_back(10);  // [10]
arr.push_back(20);  // [10, 20]
arr.push_back(30);  // [10, 20, 30]
// 不需要预先指定大小，按需增长
```
{% endtab %}
{% tab title="C#" %}
```csharp
// C# 的 List<T> 就是动态数组
List<int> arr = new List<int>();
arr.Add(10);  // [10]
arr.Add(20);  // [10, 20]
arr.Add(30);  // [10, 20, 30]
// 不需要预先指定大小，按需增长
```
{% endtab %}
{% endtabs %}

### 扩容机制

动态数组的扩容过程如下：

1. 当前数组已满，无法继续插入
2. 分配一块新的、更大的连续内存（通常是原来的 2 倍）
3. 将旧数组中的所有元素复制到新数组
4. 释放旧数组的内存
5. 将新元素插入到新数组中

```
扩容前（容量 4，已满）:
+----+----+----+----+
| 10 | 20 | 30 | 40 |
+----+----+----+----+

扩容后（容量 8）:
+----+----+----+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |    |    |    |
+----+----+----+----+----+----+----+----+
```

### 均摊分析

单次扩容需要复制 n 个元素，时间复杂度为 O(n)。但扩容并不是每次插入都会发生。以 2 倍扩容为例：

- 容量从 1 扩到 2：复制 1 个元素
- 容量从 2 扩到 4：复制 2 个元素
- 容量从 4 扩到 8：复制 4 个元素
- 容量从 k 扩到 2k：复制 k 个元素

插入 n 个元素的总复制次数为 1 + 2 + 4 + ... + n/2 + n，约等于 2n。分摊到每次插入操作上，平均复制次数为常数。因此，动态数组尾部插入的**均摊时间复杂度**（Amortized Time Complexity）为 O(1)。

## 手动实现一个动态数组

理解动态数组的最好方式是自己实现一个。以下是一个简化版本：

{% tabs %}
{% tab title="Python" %}
```python
class DynamicArray:
    def __init__(self):
        self._capacity = 4          # 初始容量
        self._size = 0              # 当前元素个数
        self._data = [None] * self._capacity

    def size(self):
        return self._size

    def get(self, index):
        if index < 0 or index >= self._size:
            raise IndexError("下标越界")
        return self._data[index]

    def set(self, index, value):
        if index < 0 or index >= self._size:
            raise IndexError("下标越界")
        self._data[index] = value

    def append(self, value):
        if self._size == self._capacity:
            self._resize(self._capacity * 2)
        self._data[self._size] = value
        self._size += 1

    def insert(self, index, value):
        if index < 0 or index > self._size:
            raise IndexError("下标越界")
        if self._size == self._capacity:
            self._resize(self._capacity * 2)
        # 将 index 及之后的元素后移一位
        for i in range(self._size, index, -1):
            self._data[i] = self._data[i - 1]
        self._data[index] = value
        self._size += 1

    def remove(self, index):
        if index < 0 or index >= self._size:
            raise IndexError("下标越界")
        # 将 index 之后的元素前移一位
        for i in range(index, self._size - 1):
            self._data[i] = self._data[i + 1]
        self._data[self._size - 1] = None
        self._size -= 1

    def _resize(self, new_capacity):
        new_data = [None] * new_capacity
        for i in range(self._size):
            new_data[i] = self._data[i]
        self._data = new_data
        self._capacity = new_capacity
```
{% endtab %}
{% tab title="Java" %}
```java
public class DynamicArray {
    private int[] data;
    private int size;
    private int capacity;

    public DynamicArray() {
        capacity = 4;              // 初始容量
        size = 0;                  // 当前元素个数
        data = new int[capacity];
    }

    public int size() {
        return size;
    }

    public int get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("下标越界");
        }
        return data[index];
    }

    public void set(int index, int value) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("下标越界");
        }
        data[index] = value;
    }

    public void append(int value) {
        if (size == capacity) {
            resize(capacity * 2);
        }
        data[size] = value;
        size++;
    }

    public void insert(int index, int value) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("下标越界");
        }
        if (size == capacity) {
            resize(capacity * 2);
        }
        // 将 index 及之后的元素后移一位
        for (int i = size; i > index; i--) {
            data[i] = data[i - 1];
        }
        data[index] = value;
        size++;
    }

    public void remove(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("下标越界");
        }
        // 将 index 之后的元素前移一位
        for (int i = index; i < size - 1; i++) {
            data[i] = data[i + 1];
        }
        data[size - 1] = 0;
        size--;
    }

    private void resize(int newCapacity) {
        int[] newData = new int[newCapacity];
        for (int i = 0; i < size; i++) {
            newData[i] = data[i];
        }
        data = newData;
        capacity = newCapacity;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
class DynamicArray {
private:
    int* data;
    int _size;
    int _capacity;

    void resize(int newCapacity) {
        int* newData = new int[newCapacity];
        for (int i = 0; i < _size; i++) {
            newData[i] = data[i];
        }
        delete[] data;
        data = newData;
        _capacity = newCapacity;
    }

public:
    DynamicArray() : _capacity(4), _size(0) {   // 初始容量 4
        data = new int[_capacity];
    }

    ~DynamicArray() {
        delete[] data;
    }

    int size() const {
        return _size;
    }

    int get(int index) const {
        if (index < 0 || index >= _size) {
            throw std::out_of_range("下标越界");
        }
        return data[index];
    }

    void set(int index, int value) {
        if (index < 0 || index >= _size) {
            throw std::out_of_range("下标越界");
        }
        data[index] = value;
    }

    void append(int value) {
        if (_size == _capacity) {
            resize(_capacity * 2);
        }
        data[_size] = value;
        _size++;
    }

    void insert(int index, int value) {
        if (index < 0 || index > _size) {
            throw std::out_of_range("下标越界");
        }
        if (_size == _capacity) {
            resize(_capacity * 2);
        }
        // 将 index 及之后的元素后移一位
        for (int i = _size; i > index; i--) {
            data[i] = data[i - 1];
        }
        data[index] = value;
        _size++;
    }

    void remove(int index) {
        if (index < 0 || index >= _size) {
            throw std::out_of_range("下标越界");
        }
        // 将 index 之后的元素前移一位
        for (int i = index; i < _size - 1; i++) {
            data[i] = data[i + 1];
        }
        data[_size - 1] = 0;
        _size--;
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
public class DynamicArray {
    private int[] data;
    private int size;
    private int capacity;

    public DynamicArray() {
        capacity = 4;              // 初始容量
        size = 0;                  // 当前元素个数
        data = new int[capacity];
    }

    public int Size() {
        return size;
    }

    public int Get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfRangeException("下标越界");
        }
        return data[index];
    }

    public void Set(int index, int value) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfRangeException("下标越界");
        }
        data[index] = value;
    }

    public void Append(int value) {
        if (size == capacity) {
            Resize(capacity * 2);
        }
        data[size] = value;
        size++;
    }

    public void Insert(int index, int value) {
        if (index < 0 || index > size) {
            throw new IndexOutOfRangeException("下标越界");
        }
        if (size == capacity) {
            Resize(capacity * 2);
        }
        // 将 index 及之后的元素后移一位
        for (int i = size; i > index; i--) {
            data[i] = data[i - 1];
        }
        data[index] = value;
        size++;
    }

    public void Remove(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfRangeException("下标越界");
        }
        // 将 index 之后的元素前移一位
        for (int i = index; i < size - 1; i++) {
            data[i] = data[i + 1];
        }
        data[size - 1] = 0;
        size--;
    }

    private void Resize(int newCapacity) {
        int[] newData = new int[newCapacity];
        for (int i = 0; i < size; i++) {
            newData[i] = data[i];
        }
        data = newData;
        capacity = newCapacity;
    }
}
```
{% endtab %}
{% endtabs %}

使用示例：

{% tabs %}
{% tab title="Python" %}
```python
arr = DynamicArray()
arr.append(10)
arr.append(20)
arr.append(30)
print(arr.get(1))    # 输出 20

arr.insert(1, 15)    # 在下标 1 插入 15
# 数组变为 [10, 15, 20, 30]

arr.remove(2)        # 删除下标 2 的元素
# 数组变为 [10, 15, 30]
```
{% endtab %}
{% tab title="Java" %}
```java
DynamicArray arr = new DynamicArray();
arr.append(10);
arr.append(20);
arr.append(30);
System.out.println(arr.get(1));    // 输出 20

arr.insert(1, 15);    // 在下标 1 插入 15
// 数组变为 [10, 15, 20, 30]

arr.remove(2);        // 删除下标 2 的元素
// 数组变为 [10, 15, 30]
```
{% endtab %}
{% tab title="C++" %}
```cpp
DynamicArray arr;
arr.append(10);
arr.append(20);
arr.append(30);
std::cout << arr.get(1) << std::endl;    // 输出 20

arr.insert(1, 15);    // 在下标 1 插入 15
// 数组变为 [10, 15, 20, 30]

arr.remove(2);        // 删除下标 2 的元素
// 数组变为 [10, 15, 30]
```
{% endtab %}
{% tab title="C#" %}
```csharp
DynamicArray arr = new DynamicArray();
arr.Append(10);
arr.Append(20);
arr.Append(30);
Console.WriteLine(arr.Get(1));    // 输出 20

arr.Insert(1, 15);    // 在下标 1 插入 15
// 数组变为 [10, 15, 20, 30]

arr.Remove(2);        // 删除下标 2 的元素
// 数组变为 [10, 15, 30]
```
{% endtab %}
{% endtabs %}

## 数组的典型应用

- **存储一组有序数据**，如学生的成绩列表
- **作为其他数据结构的底层实现**，如栈、队列、堆都可以基于数组实现
- **矩阵运算**，二维数组用于表示矩阵
- **缓冲区**，在文件读写、网络通信中用作临时存储

## 数组的优缺点

**优点：**

- 随机访问速度快，O(1)
- 内存连续，缓存命中率高，实际运行效率好
- 实现简单，语言原生支持

**缺点：**

- 插入和删除效率低，O(n)
- 静态数组大小固定，动态数组扩容有额外开销
- 要求连续内存，大数组可能分配失败

当你的使用场景以**读取和遍历为主、很少在中间插入删除**时，数组是最合适的选择。如果需要频繁在任意位置插入或删除，应考虑使用[链表](linked-list.md)。

## 本章小结

- 数组将相同类型的元素存储在连续内存中，通过下标实现 O(1) 的随机访问
- 插入和删除操作需要移动元素，最坏情况为 O(n)
- 动态数组通过自动扩容解决了静态数组大小固定的问题
- 动态数组采用倍增策略扩容，尾部插入的均摊时间复杂度为 O(1)
- 数组适合读多写少的场景，是许多其他数据结构的底层基础
