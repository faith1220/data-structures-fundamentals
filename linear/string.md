# 字符串

## 什么是字符串

字符串（String）是由零个或多个字符组成的有序序列。从数据结构的角度看，字符串本质上是一个**字符数组**（一排装满字符的格子），但由于其在日常编程中的使用频率极高，并且具备许多专属的操作，因此通常被当作一种独立的数据结构来讨论。

你可以把字符串想象成一串由字母、数字和标点符号组成的珠子手链。

```text
字符串: "hello"
在内存中的逻辑结构类似于字符数组:
+---+---+---+---+---+
| h | e | l | l | o |
+---+---+---+---+---+
  0   1   2   3   4
```

几乎所有编程语言都将字符串作为内置类型提供。但是在不同的语言中，字符串的底层实现有所不同，最主要的区别在于**可变性（Mutability）**：
- **不可变（Immutable）**：像 Python 和 Java、C# 中的字符串，一旦创建，就不能再去修改它里面的某一个字符。如果想修改，系统会在内存中创建一个全新的字符串。
- **可变（Mutable）**：像 C 语言的字符数组，或者 C++ 中的 `std::string`，你可以随时修改其中的某一个字符。

## 字符串的存储方式

### 顺序存储概念

大多数语言采用顺序存储的方式，也就是将字符连续地放在内存中。C 语言中最经典的实现方式是使用末尾的空字符 `\0` 来标记字符串的结束，这种方式被称为**零终止字符串（Null-terminated String）**。

```c
// C 语言中的经典字符串
char s[] = "hello";
// 实际在内存中的存储多了一个 '\0' 作为结束标志:
// | h | e | l | l | o | \0 |
```

而在 Python、Java、C# 和 C++ (`std::string`) 中，底层系统通常会额外记录一个字段叫做“长度（Length/Size）”，这样就不需要依赖特殊的结束符了。

### 字符串不可变性带来的影响

在 Python、Java 和 C# 中，对字符串进行频繁的拼接修改时要特别注意。因为每次拼接实际上都会申请一块新的内存，把新旧内容拷贝进去。如果在一个巨大的循环里一直通过 `+` 号拼接字符串，会导致程序运行变得非常缓慢（时间复杂度退化为 O(n²)）。

因此，在需要频繁拼接字符串时，我们通常会使用专门的工具（比如 Java 的 `StringBuilder`，C# 的 `StringBuilder`，Python 的 `"".join()` 列表）。

## 字符串的基本操作

### 1. 访问字符与子串提取

访问单个字符通常通过下标索引进行（时间复杂度 O(1)）。
子串提取是从原字符串中截取连续的一段（时间复杂度 O(k)，k 是截取的长度）。

{% tabs %}
{% tab title="Python" %}
```python
s = "hello world"

# 访问字符
print(s[0])   # 输出 'h'
print(s[-1])  # 输出 'd'（Python 独有特性，支持负数下标记倒数）

# 提取子串 [起始索引:结束索引]，左闭右开
print(s[0:5]) # 输出 "hello"
print(s[6:])  # 输出 "world"
```
{% endtab %}
{% tab title="Java" %}
```java
String s = "hello world";

// 访问单个字符使用 charAt 方法
System.out.println(s.charAt(0)); // 输出 'h'
System.out.println(s.charAt(s.length() - 1)); // 输出 'd'

// 提取子串 substring(startIndex, endIndex)，左闭右开
System.out.println(s.substring(0, 5)); // 输出 "hello"
System.out.println(s.substring(6));    // 输出 "world"
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <iostream>
#include <string>

std::string s = "hello world";

// 访问单个字符使用 [] 或 at()
std::cout << s[0] << std::endl; // 输出 'h'
std::cout << s.back() << std::endl; // 输出 'd'

// 提取子串 substr(startIndex, length) 注意第二个参数是截取长度
std::cout << s.substr(0, 5) << std::endl; // 输出 "hello"
std::cout << s.substr(6) << std::endl;    // 输出 "world"
```
{% endtab %}
{% tab title="C#" %}
```csharp
string s = "hello world";

// 访问单个字符使用 []
Console.WriteLine(s[0]); // 输出 'h'
Console.WriteLine(s[s.Length - 1]); // 输出 'd'

// 提取子串 Substring(startIndex, length) 注意第二个参数是截取的长度
Console.WriteLine(s.Substring(0, 5)); // 输出 "hello"
Console.WriteLine(s.Substring(6));    // 输出 "world"
```
{% endtab %}
{% endtabs %}

### 2. 字符串比较

两个字符串的比较不仅可以判断是否相等，还可以判断谁大谁小。比较规则是按照**字典序（Lexicographic Order）**逐个字符进行的：从第一个字符开始，比较对应位置字符的编码值大小，一旦遇到不同的字符就能得出整体结论。

例如：
- `"apple" < "banana"` （因为第一个字母 'a' 的编码小于 'b'）
- `"abc" < "abcd"` （前缀完全相同，较短的字符串认为更小）

比较两个长度为 n 和 m 的字符串，最坏情况下需要逐一比较，时间复杂度为 O(min(n, m))。

## 字符串匹配算法

**字符串匹配（String Matching）** 是字符串领域中最核心也是最常考的问题之一：给定一段长文本（Text）和一个简短的模式串（Pattern），要求找出模式串在文本中第一次出现的位置。比如我们在文档中使用 `Ctrl + F` 查找某个词，背后运行的就是匹配算法。

### 暴力匹配（Brute Force）

对于初学者，最直观的想法就是“暴力匹配”：我们将模式串和文本串左对齐，如果字符相同，就继续对比下一个；如果出现不同，就把模式串向右平移一格，再从头开始一节一节比对。

{% tabs %}
{% tab title="Python" %}
```python
def brute_force_search(text, pattern):
    n = len(text)
    m = len(pattern)

    # i 指向文本串的起始尝试位置
    for i in range(n - m + 1):
        match = True
        # j 指向模式串的当前字符
        for j in range(m):
            if text[i + j] != pattern[j]:
                match = False # 发现不匹配，这轮尝试失败
                break
        if match:
            return i # 全部匹配成功，返回起始索引
    return -1 # 没找到
```
{% endtab %}
{% tab title="Java" %}
```java
public static int bruteForceSearch(String text, String pattern) {
    int n = text.length();
    int m = pattern.length();

    for (int i = 0; i <= n - m; i++) {
        boolean match = true;
        for (int j = 0; j < m; j++) {
            if (text.charAt(i + j) != pattern.charAt(j)) {
                match = false;
                break;
            }
        }
        if (match) {
            return i;
        }
    }
    return -1;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
int bruteForceSearch(const std::string& text, const std::string& pattern) {
    int n = text.length();
    int m = pattern.length();

    for (int i = 0; i <= n - m; i++) {
        bool match = true;
        for (int j = 0; j < m; j++) {
            if (text[i + j] != pattern[j]) {
                match = false;
                break;
            }
        }
        if (match) {
            return i;
        }
    }
    return -1;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static int BruteForceSearch(string text, string pattern) {
    int n = text.Length;
    int m = pattern.Length;

    for (int i = 0; i <= n - m; i++) {
        bool match = true;
        for (int j = 0; j < m; j++) {
            if (text[i + j] != pattern[j]) {
                match = false;
                break;
            }
        }
        if (match) {
            return i;
        }
    }
    return -1;
}
```
{% endtab %}
{% endtabs %}

**性能分析：**
在最坏的情况下（例如文本串是 `"aaaaab"`，模式串是 `"aaab"`），每次都要苦苦比较到最后一个字符才会发现失败，导致大量无意义的重复比较。暴力匹配的时间复杂度高达 **O(n × m)**。

### KMP 算法（进阶）

**KMP 算法（Knuth-Morris-Pratt）** 是一种极其巧妙的算法，能将时间复杂度优化到令人惊叹的 **O(n + m)**。
它的核心思想是：在匹配失败时，**绝不回退文本串的指针**。它通过提前分析模式串本身，利用“匹配失败前已经成功匹配的内容”，聪明地将模式串向右滑动尽可能远的距离。

其秘密武器是一个叫做 **前缀表（Prefix Table 或 next 数组）** 的工具：
前缀表记录了模式串中“最长相同前后缀的长度”。当我们匹配失败时，可以直接查表，让模式串跳到正确的位置继续比较，避免傻傻地从头再来。

*(注意：对于刚入门的新手来说，KMP 算法较为烧脑，可以作为一个进阶了解。)*

{% tabs %}
{% tab title="Python" %}
```python
def build_prefix_table(pattern):
    m = len(pattern)
    prefix = [0] * m
    length = 0  # 当前最长相同前后缀的长度
    i = 1

    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            prefix[i] = length
            i += 1
        else:
            if length > 0:
                length = prefix[length - 1] # 回退
            else:
                prefix[i] = 0
                i += 1
    return prefix

def kmp_search(text, pattern):
    n = len(text)
    m = len(pattern)
    if m == 0: return 0

    prefix = build_prefix_table(pattern)
    j = 0  # 模式串指针

    for i in range(n):
        while j > 0 and text[i] != pattern[j]:
            j = prefix[j - 1] # 查表进行智力跳跃，避免文本串指针 i 回退
        if text[i] == pattern[j]:
            j += 1
        if j == m:
            return i - m + 1 # 找到了
    return -1
```
{% endtab %}
{% tab title="Java" %}
```java
public static int[] buildPrefixTable(String pattern) {
    int m = pattern.length();
    int[] prefix = new int[m];
    int length = 0;
    int i = 1;

    while (i < m) {
        if (pattern.charAt(i) == pattern.charAt(length)) {
            length++;
            prefix[i] = length;
            i++;
        } else {
            if (length > 0) {
                length = prefix[length - 1];
            } else {
                prefix[i] = 0;
                i++;
            }
        }
    }
    return prefix;
}

public static int kmpSearch(String text, String pattern) {
    int n = text.length(), m = pattern.length();
    if (m == 0) return 0;

    int[] prefix = buildPrefixTable(pattern);
    int j = 0;

    for (int i = 0; i < n; i++) {
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
            j = prefix[j - 1];
        }
        if (text.charAt(i) == pattern.charAt(j)) {
            j++;
        }
        if (j == m) {
            return i - m + 1;
        }
    }
    return -1;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>
#include <string>

std::vector<int> buildPrefixTable(const std::string& pattern) {
    int m = pattern.length();
    std::vector<int> prefix(m, 0);
    int length = 0;
    int i = 1;

    while (i < m) {
        if (pattern[i] == pattern[length]) {
            length++;
            prefix[i] = length;
            i++;
        } else {
            if (length > 0) {
                length = prefix[length - 1];
            } else {
                prefix[i] = 0;
                i++;
            }
        }
    }
    return prefix;
}

int kmpSearch(const std::string& text, const std::string& pattern) {
    int n = text.length(), m = pattern.length();
    if (m == 0) return 0;

    std::vector<int> prefix = buildPrefixTable(pattern);
    int j = 0;

    for (int i = 0; i < n; i++) {
        while (j > 0 && text[i] != pattern[j]) {
            j = prefix[j - 1];
        }
        if (text[i] == pattern[j]) {
            j++;
        }
        if (j == m) {
            return i - m + 1;
        }
    }
    return -1;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static int[] BuildPrefixTable(string pattern) {
    int m = pattern.Length;
    int[] prefix = new int[m];
    int length = 0;
    int i = 1;

    while (i < m) {
        if (pattern[i] == pattern[length]) {
            length++;
            prefix[i] = length;
            i++;
        } else {
            if (length > 0) {
                length = prefix[length - 1];
            } else {
                prefix[i] = 0;
                i++;
            }
        }
    }
    return prefix;
}

public static int KmpSearch(string text, string pattern) {
    int n = text.Length, m = pattern.Length;
    if (m == 0) return 0;

    int[] prefix = BuildPrefixTable(pattern);
    int j = 0;

    for (int i = 0; i < n; i++) {
        while (j > 0 && text[i] != pattern[j]) {
            j = prefix[j - 1];
        }
        if (text[i] == pattern[j]) {
            j++;
        }
        if (j == m) {
            return i - m + 1;
        }
    }
    return -1;
}
```
{% endtab %}
{% endtabs %}

## 常见字符串操作技巧

由于在算法面试和日常开发中，处理字符串的需求极高，掌握一些基础的套路技巧非常重要。

### 1. 回文判断（双指针法）

**回文（Palindrome）** 指的是正着读和反着读都完全一样的字符串。比如 `"racecar"` 或者 `"aba"`。
判断回文的最优方法是使用左右双指针，分别从字符串的两头开始，往中间收缩，一旦发现不对等的字符就判错。

{% tabs %}
{% tab title="Python" %}
```python
def is_palindrome(s):
    left = 0
    right = len(s) - 1
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True
```
{% endtab %}
{% tab title="Java" %}
```java
public static boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
bool isPalindrome(const std::string& s) {
    int left = 0;
    int right = s.length() - 1;
    while (left < right) {
        if (s[left] != s[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static bool IsPalindrome(string s) {
    int left = 0;
    int right = s.Length - 1;
    while (left < right) {
        if (s[left] != s[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```
{% endtab %}
{% endtabs %}
此算法时间复杂度为 O(n)，且不需要额外的空间（空间复杂度 O(1)），非常高效。

### 2. 统计字符与变位词判断（哈希表/数组记录）

**变位词（Anagram）** 是指由相同数量的相同字母组成的单词，只是字母顺序被打乱了。比如 `"listen"` 和 `"silent"`。
如何判断两个词是否是变位词呢？最快的方法是用哈希表（或大小为 256 的数组）来“数数”，统计第一个词里的每个字母各出现了几次，然后再遍历第二个词，从统计表里相减，最后如果每种字母的数量都刚刚好抵消为 0，就是变位词。

{% tabs %}
{% tab title="Python" %}
```python
def is_anagram(s1, s2):
    if len(s1) != len(s2):
        return False

    count = {} # 使用字典记录出现次数
    for c in s1:
        count[c] = count.get(c, 0) + 1

    for c in s2:
        if count.get(c, 0) <= 0:
            return False
        count[c] -= 1

    return True
```
{% endtab %}
{% tab title="Java" %}
```java
public static boolean isAnagram(String s1, String s2) {
    if (s1.length() != s2.length()) return false;

    // 假设都是小写英文字母，可以使用大小为 26 的数组作为简易哈希表
    int[] count = new int[26];
    for (int i = 0; i < s1.length(); i++) {
        count[s1.charAt(i) - 'a']++;
    }

    for (int i = 0; i < s2.length(); i++) {
        int index = s2.charAt(i) - 'a';
        if (count[index] <= 0) {
            return false;
        }
        count[index]--;
    }
    return true;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
bool isAnagram(const std::string& s1, const std::string& s2) {
    if (s1.length() != s2.length()) return false;

    int count[26] = {0}; // 假设都是字母
    for (char c : s1) {
        count[c - 'a']++;
    }

    for (char c : s2) {
        if (count[c - 'a'] <= 0) {
            return false;
        }
        count[c - 'a']--;
    }
    return true;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public static bool IsAnagram(string s1, string s2) {
    if (s1.Length != s2.Length) return false;

    int[] count = new int[26];
    foreach (char c in s1) {
        count[c - 'a']++;
    }

    foreach (char c in s2) {
        if (count[c - 'a'] <= 0) {
            return false;
        }
        count[c - 'a']--;
    }
    return true;
}
```
{% endtab %}
{% endtabs %}

## 再了解一点：字符编码

计算机是不能直接看懂“字母”的，它只能处理“0 和 1”组成的数字。为了让计算机处理文字，人类发明了**字符编码（Encoding）**，就是给每个字符分配一个唯一的数字身份证。

- **ASCII** — 用一个字节的数字表示了 128 个基本字符，包括英文字母、数字和常见的标点。比如大写字母 `'A'` 的身份证号是 65，小写 `'a'` 是 97。
- **Unicode** — 由于世界上语言太多了，汉字、日文、阿拉伯文等等，后来就搞了一个全球通用的巨大身份证登记表，为所有文字分配了编号。
- **UTF-8** — 它是一种非常聪明的 Unicode 存储格式，它会根据字符的不同，自动调整使用的空间。英文字母省着用 1 字节，复杂的中文就用 3 字节，既兼容又省内存。

在许多语言中，字符可以转换成对应的整数编码用于加减运算，这就解释了前面为什么写 `s1.charAt(i) - 'a'` 就能神奇地将 `a~z` 映射为 `0~25` 的数组下标了。

## 本章小结

- **字符串结构**：可以看作是一排连续存储的字符数组，但不同语言对其是否“可变（Mutable）”的定义不同，要小心频繁拼接字符串带来的性能拖累。
- **匹配算法**：字符串检索是非常常见的需求，暴力算法 O(n × m) 简单好写但遇上刁钻数据会慢，KMP 算法利用前缀表将性能优化到了 O(n + m)。
- **常见技巧**：要熟练掌握应对回文判断的“头尾双指针法”，以及寻找变位词所依赖的“数组（哈希表）记录法”。
- **底层原理**：理解计算机底层是如何通过编码记录字符的。