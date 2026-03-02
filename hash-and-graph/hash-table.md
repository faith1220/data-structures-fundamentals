# 哈希表

如果有人问你：在计算机所有的基础数据结构里，**哪一种结构查找数据的速度最快？** 答案毫无争议，那就是**哈希表（Hash Table，也叫散列表）**。

在前面的章节中：

* 在**数组**或**链表**中找东西，你需要从头到尾挨个看一遍，最坏情况时间复杂度是 **O(n)**。
* 在**二叉搜索树（BST）**&#x4E2D;找东西，你可以像“猜数字”一样每次排除一半，时间复杂度是 **O(\log n)**。

但是，**哈希表**打破了这个极限，它能跨越数据量的限制，无论数据有 10 个还是 10 亿个，它都能在那堆数据中做到**指哪打哪，瞬间定位**！它的平均查找时间复杂度是极其恐怖的 **O(1)**（常数时间）。

它是怎么做到的？让我们来揭开哈希表的神秘面纱。

***

## 现实生活中的“哈希思想”

为了理解哈希表，想象一下你去大学图书馆找书。 图书馆里有上百万册书。如果你没有任何线索，只能一本一本地翻看书名（这叫“线性遍历 O(n)”），可能得找好几个月。

但是现实中你不需要这么做，因为每一本书都有一个独一无二的编号（比如 `TP312/C11`）。你拿着这个编号，走到计算机类书架区，直接就能抽出那本书。 在这个过程中：

1. **索书号（把复杂书名转换成的简短编码）** -> 就等同于计算机里的 **哈希函数（Hash Function）**。
2. **书架上的具体位置** -> 就等同于内存里的 **数组下标**。

**哈希表的核心原理就是：通过一个“魔法函数”，把你要找的“名字（Key）”，直接算成一个“数组的下标（Index）”，然后直接去那个下标的格子里拿东西！**

***

## 哈希表的三大核心组件

### 1. 键值对（Key-Value Pair）

哈希表中存储的数据，永远是成对出现的。

* **Key（键）**：用来查询的唯一标识，比如学生的学号、人的身份证号、单词本身。
* **Value（值）**：你真正想要存储的具体信息，比如学生的成绩、人的档案资料、单词的释义。

哈希表的目的是：**给我 Key，我瞬间把对应的 Value 扔给你。**

### 2. 底层数组

很多人不知道，**哈希表的底层，其实就是一个彻头彻尾的普通数组！** 为什么用数组？因为数组有一个非常逆天的特性：只要你知道了下标 `i`，访问 `array[i]` 的速度就是 O(1)。哈希表就是利用了这个特性。

### 3. 哈希函数（Hash Function）

这是哈希表的灵魂。它是一台“加工机器”。 无论你丢进去多么长、多么奇怪的 Key（比如一个字符串 `"apple"`），哈希函数都会经过一顿复杂的数学运算，吐出一个**固定范围内的整数**。这个整数，就被直接用作**底层数组的下标**。

```
 用户的 Key      哈希函数           算出的数组下标    底层数组
"apple"   --->  hash("apple")  --->    3    --->  array[3] = "苹果"
"banana"  --->  hash("banana") --->    0    --->  array[0] = "香蕉"
"cat"     --->  hash("cat")    --->    7    --->  array[7] = "猫"
```

当你要查询 `"apple"` 的中文意思时，程序不会去遍历数组。它会再次调用 `hash("apple")` 算出 `3`，然后直接拉开 `array[3]` 的抽屉，取出“苹果”。**一步到位！**

***

## 无法避免的麻烦：哈希冲突（Hash Collision）

理想很丰满，现实很骨感。 假设我们的底层数组只有 10 个格子（下标从 0 到 9）。但是世界上的单词有上百万个。把你算出的数字挤压到 0-9 这 10 个格子里，**必然会出现两个不同的单词，算出了相同的下标！**

比如：

* `hash("dog")` 算出来是 `5`。
* `hash("god")` 算出来恰好也是 `5`。

完蛋了，第 5 号抽屉里已经被 `"dog"` 占了，`"god"` 该放哪？这种“不同 Key 映射到同一个位置”的现象，被称为**哈希冲突（Collision）**。 它是哈希表面临的核心难题，解决它有两种最经典的门派：

### 解决冲突门派一：拉链法（Separate Chaining）

这是目前工业界（如 Java 的 HashMap）最主流的做法。 它的思路非常简单粗暴：既然抽屉里只能放一个东西，那我就把抽屉改造成**一条链表**！

如果 `dog` 和 `god` 都争抢 5 号抽屉，那就让 5 号抽屉伸出一条链表，把它们串起来：

```
数组抽屉
 [0] -> null
 ...
 [5] -> [Key:"dog", Val:"狗"] -> [Key:"god", Val:"上帝"] -> null
 ...
```

**查询过程**：当我们要查 `"god"` 时，算出来是抽屉 5，拉开抽屉发现是一根链子。那就顺着链子挨个对比 Key：第一个是 "dog" 不是我要找的，第二个是 "god" 找到了，取出对应的 Value "上帝"。只要链表不要太长，这依然非常快。

### 解决冲突门派二：开放寻址法（Open Addressing）

这个门派的规矩是：**绝不另外开辟链表，所有数据必须在这个大数组里解决。**

如果 `dog` 占了 5 号包厢，这时候 `god` 也拿了 5 号包厢的票。`god` 发现门推不开，它该怎么办？

* **线性探测（Linear Probing）**：它会非常有耐心地去敲 6 号包厢的门。看 6 号空不空，空就住进去；不空就继续敲 7 号，直到找到空房间为止。

这种方法在数据快装满时会引起严重的“拥堵（聚集现象）”，因此需要经常给大数组“扩容（搬到更大的新酒店）”。

***

## 各种编程语言中的哈希表大保健

作为天下第一的数据结构，每天的开发都离不开它。所有主流语言都把哈希表作为最核心的内置组件（甚至就是语言本身的一部分）。

在实战中，我们几乎把它们简称为**字典（Dictionary）或映射（Map）**。

{% tabs %}
{% tab title="Python" %}
```python
# Python 中最常用的 dict 字典，底层就是用 C 语言写的一套极度优化的哈希表！
# Python 使用的是变种的“开放寻址法”来解决冲突。

# 1. 创建哈希表
student_scores = {
    "Alice": 95,
    "Bob": 82
}

# 2. 插入或更新 (O(1))
student_scores["Charlie"] = 88 # 瞬间在底层数组找到空位塞入

# 3. 查找 (O(1))
print(student_scores["Alice"]) # 输出 95

# 4. 检查 Key 是否存在 (O(1))
if "Bob" in student_scores:
    print("Bob 考了", student_scores["Bob"])

# 5. 删除
del student_scores["Bob"]
```
{% endtab %}

{% tab title="Java" %}
```java
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        // Java 中最著名的集合类 HashMap
        // 底层使用“拉链法”，并且在链表过长时，会自动把链表升级成前面学的“红黑树”！
        Map<String, Integer> studentScores = new HashMap<>();

        // 1. 插入
        studentScores.put("Alice", 95);
        studentScores.put("Bob", 82);

        // 2. 查找
        System.out.println(studentScores.get("Alice")); // 输出 95

        // 3. 检查是否存在
        if (studentScores.containsKey("Bob")) {
            System.out.println("Bob exists!");
        }

        // 4. 删除
        studentScores.remove("Bob");
    }
}
```
{% endtab %}

{% tab title="C++" %}
```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    // 特别注意：C++ 里的 std::map 底层是红黑树（O(log n)），自带按 Key 排序功能。
    // 如果你纯粹要追求 O(1) 的速度（即真正的哈希表），必须用 std::unordered_map
    std::unordered_map<std::string, int> student_scores;

    // 1. 插入
    student_scores["Alice"] = 95;
    student_scores["Bob"] = 82;

    // 2. 查找
    std::cout << student_scores["Alice"] << std::endl; // 输出 95

    // 3. 检查是否存在
    if (student_scores.find("Bob") != student_scores.end()) {
        std::cout << "Bob exists!" << std::endl;
    }

    // 4. 删除
    student_scores.erase("Bob");

    return 0;
}
```
{% endtab %}

{% tab title="C#" %}
```csharp
using System;
using System.Collections.Generic;

class Program {
    static void Main() {
        // C# 中的主流哈希表结构叫 Dictionary (字典)
        Dictionary<string, int> studentScores = new Dictionary<string, int>();

        // 1. 插入
        # studentScores["Alice"] = 95 会覆盖原有同名 Key。
        // 而 Add 不能重复添加同名 Key，会报错。
        studentScores["Alice"] = 95;
        studentScores.Add("Bob", 82);

        // 2. 查找
        Console.WriteLine(studentScores["Alice"]); // 输出 95

        // 3. 检查是否存在
        if (studentScores.ContainsKey("Bob")) {
            Console.WriteLine("Bob exists!");
        }

        // 4. 删除
        studentScores.Remove("Bob");
    }
}
```
{% endtab %}
{% endtabs %}

## 问题：为什么 HashMap 查询极快，但依然会有 O(n) 的风险？

面试官经常会挖坑问你：“你说哈希表的查询是 O(1)，难道它就绝对没有慢的时候吗？”

这时候你要用前面学过的“哈希冲突”来反击他： 如果有一个极其恶意的骇客，故意构造了 10,000 个能算出同一个哈希值的不同单词，然后硬塞进你的哈希表。 如果你的底层用的&#x662F;**“拉链法”**，这 10,000 个单词会在同一个抽屉里串成一条 10,000 个节点的**单边链表**。 当你再去这个抽屉里找某个单词时，程序不得不在长长的链表里**从头遍历到底**。你的查询速度毫无防备地，从光速的 O(1) 跌落回了灾难级的 O(n)。

(_小拓展：为了防范这种“哈希碰撞攻击”，Java 8 之后的 `HashMap` 做了非常天才的改进：一旦发现某个抽屉上的链表超过了 8 个节点，它会毫不犹豫地把这根单薄的链表直接改造成上一章讲的“**红黑树**”！从而把最差的速度拼死扛住在 O(\log n)。_)

## 本章小结

* **最快第一**：哈希表通过牺牲一定的内存空间，换取了天下第一的极速 $O(1)$ 查询能力，是典型的“以空间换时间”。
* **三大组件**：哈希表本质上是一个存放**键值对**的**底层数组**，通过**哈希函数**像魔法一样把 Key 转化成数组下标。
* **两大痛点**：哈希函数的运算速度，以及遇到**哈希冲突**时如何化解（拉链法 vs 开放寻址法），决定了一个哈希表底层的好坏。
* **现成工具**：务必高度熟练掌握 Python 的 `dict`、Java 的 `HashMap`、C++ 的 `unordered_map` 以及 C# 的 `Dictionary` 的基本用法，绝对没有任何一个大型软件的开发能离开哈希表。
