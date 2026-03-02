# 栈

## 什么是栈

栈（Stack）是一种操作受限的线性数据结构，它只允许在一端进行插入和删除操作。这一端被称为栈顶（Top），另一端称为栈底（Bottom）。

栈遵循**后进先出**（Last In, First Out，简称 LIFO）的原则：最后放入的元素最先被取出，最先放入的元素最后才能被取出。

生活中最直观的例子是一叠盘子：你只能从最上面放盘子，也只能从最上面取盘子。中间或底部的盘子无法直接操作。

```
栈顶 →  | 30 |  ← 最后放入，最先取出
         | 20 |
栈底 →  | 10 |  ← 最先放入，最后取出
         +----+
```

## 栈的基本操作

栈只支持以下几种核心操作：

| 操作 | 说明 | 时间复杂度 |
| --- | --- | --- |
| push(x) | 将元素 x 压入栈顶 | O(1) |
| pop() | 弹出并返回栈顶元素 | O(1) |
| peek() / top() | 查看栈顶元素但不弹出 | O(1) |
| is_empty() | 判断栈是否为空 | O(1) |
| size() | 返回栈中元素个数 | O(1) |

所有操作的时间复杂度都是 O(1)，这是栈的一大优势。

```
push(10):  | 10 |
           +----+

push(20):  | 20 |
           | 10 |
           +----+

push(30):  | 30 |
           | 20 |
           | 10 |
           +----+

pop():     | 20 |    返回 30
           | 10 |
           +----+

peek():    | 20 |    返回 20（不移除）
           | 10 |
           +----+
```

## 栈的实现

栈可以基于数组或链表来实现。两种方式各有优劣。

### 基于数组的实现

用数组的末尾作为栈顶，`append` 和 `pop` 操作天然就是 O(1) 的。

{% tabs %}
{% tab title="Python" %}
```python
class ArrayStack:
    def __init__(self):
        self._data = []

    def push(self, value):
        self._data.append(value)

    def pop(self):
        if self.is_empty():
            raise IndexError("栈为空")
        return self._data.pop()

    def peek(self):
        if self.is_empty():
            raise IndexError("栈为空")
        return self._data[-1]

    def is_empty(self):
        return len(self._data) == 0

    def size(self):
        return len(self._data)
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.ArrayList;
import java.util.List;

public class ArrayStack<T> {
    private List<T> data = new ArrayList<>();

    public void push(T value) {
        data.add(value);
    }

    public T pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空");
        }
        return data.remove(data.size() - 1);
    }

    public T peek() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空");
        }
        return data.get(data.size() - 1);
    }

    public boolean isEmpty() {
        return data.isEmpty();
    }

    public int size() {
        return data.size();
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>
#include <stdexcept>

class ArrayStack {
private:
    std::vector<int> data;

public:
    void push(int value) {
        data.push_back(value);
    }

    int pop() {
        if (isEmpty()) {
            throw std::runtime_error("栈为空");
        }
        int value = data.back();
        data.pop_back();
        return value;
    }

    int peek() {
        if (isEmpty()) {
            throw std::runtime_error("栈为空");
        }
        return data.back();
    }

    bool isEmpty() {
        return data.empty();
    }

    int size() {
        return data.size();
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;
using System.Collections.Generic;

public class ArrayStack<T> {
    private List<T> _data = new List<T>();

    public void Push(T value) {
        _data.Add(value);
    }

    public T Pop() {
        if (IsEmpty()) {
            throw new InvalidOperationException("栈为空");
        }
        T value = _data[_data.Count - 1];
        _data.RemoveAt(_data.Count - 1);
        return value;
    }

    public T Peek() {
        if (IsEmpty()) {
            throw new InvalidOperationException("栈为空");
        }
        return _data[_data.Count - 1];
    }

    public bool IsEmpty() {
        return _data.Count == 0;
    }

    public int Size() {
        return _data.Count;
    }
}
```
{% endtab %}
{% endtabs %}

### 基于链表的实现

用链表的头部作为栈顶，头部插入和删除都是 O(1)。

{% tabs %}
{% tab title="Python" %}
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class LinkedStack:
    def __init__(self):
        self._top = None
        self._size = 0

    def push(self, value):
        new_node = Node(value)
        new_node.next = self._top
        self._top = new_node
        self._size += 1

    def pop(self):
        if self.is_empty():
            raise IndexError("栈为空")
        value = self._top.value
        self._top = self._top.next
        self._size -= 1
        return value

    def peek(self):
        if self.is_empty():
            raise IndexError("栈为空")
        return self._top.value

    def is_empty(self):
        return self._top is None

    def size(self):
        return self._size
```
{% endtab %}
{% tab title="Java" %}
```java
class Node<T> {
    T value;
    Node<T> next;

    Node(T value) {
        this.value = value;
        this.next = null;
    }
}

public class LinkedStack<T> {
    private Node<T> top = null;
    private int size = 0;

    public void push(T value) {
        Node<T> newNode = new Node<>(value);
        newNode.next = top;
        top = newNode;
        size++;
    }

    public T pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空");
        }
        T value = top.value;
        top = top.next;
        size--;
        return value;
    }

    public T peek() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空");
        }
        return top.value;
    }

    public boolean isEmpty() {
        return top == null;
    }

    public int size() {
        return size;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <stdexcept>

struct Node {
    int value;
    Node* next;
    Node(int val) : value(val), next(nullptr) {}
};

class LinkedStack {
private:
    Node* _top = nullptr;
    int _size = 0;

public:
    ~LinkedStack() {
        while (_top != nullptr) {
            Node* temp = _top;
            _top = _top->next;
            delete temp;
        }
    }

    void push(int value) {
        Node* newNode = new Node(value);
        newNode->next = _top;
        _top = newNode;
        _size++;
    }

    int pop() {
        if (isEmpty()) {
            throw std::runtime_error("栈为空");
        }
        int value = _top->value;
        Node* temp = _top;
        _top = _top->next;
        delete temp;
        _size--;
        return value;
    }

    int peek() {
        if (isEmpty()) {
            throw std::runtime_error("栈为空");
        }
        return _top->value;
    }

    bool isEmpty() {
        return _top == nullptr;
    }

    int size() {
        return _size;
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;

public class Node<T> {
    public T Value { get; set; }
    public Node<T> Next { get; set; }

    public Node(T value) {
        Value = value;
        Next = null;
    }
}

public class LinkedStack<T> {
    private Node<T> _top = null;
    private int _size = 0;

    public void Push(T value) {
        Node<T> newNode = new Node<T>(value);
        newNode.Next = _top;
        _top = newNode;
        _size++;
    }

    public T Pop() {
        if (IsEmpty()) {
            throw new InvalidOperationException("栈为空");
        }
        T value = _top.Value;
        _top = _top.Next;
        _size--;
        return value;
    }

    public T Peek() {
        if (IsEmpty()) {
            throw new InvalidOperationException("栈为空");
        }
        return _top.Value;
    }

    public bool IsEmpty() {
        return _top == null;
    }

    public int Size() {
        return _size;
    }
}
```
{% endtab %}
{% endtabs %}

### 两种实现的对比

| 比较维度 | 数组实现 | 链表实现 |
| --- | --- | --- |
| push/pop 时间 | O(1)（均摊） | O(1)（严格） |
| 空间利用 | 可能有预分配浪费 | 按需分配，无浪费 |
| 额外开销 | 无 | 每个节点存储一个指针 |
| 缓存友好性 | 好 | 差 |

在大多数场景下，基于数组的实现更常用，因为它实现更简单、缓存性能更好。

## 栈的典型应用

栈的 LIFO 特性使它在许多场景中都是天然的选择。

### 应用一：括号匹配

判断一个表达式中的括号是否正确配对，是栈最经典的应用之一。

规则：遇到左括号入栈，遇到右括号时弹出栈顶并检查是否匹配。最终栈为空则匹配成功。

{% tabs %}
{% tab title="Python" %}
```python
def is_valid_parentheses(s):
    stack = []
    pairs = {')': '(', ']': '[', '}': '{'}

    for char in s:
        if char in '([{':
            stack.append(char)
        elif char in ')]}':
            if not stack or stack[-1] != pairs[char]:
                return False
            stack.pop()

    return len(stack) == 0
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Map;

public static boolean isValidParentheses(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    Map<Character, Character> pairs = Map.of(')', '(', ']', '[', '}', '{');

    for (char ch : s.toCharArray()) {
        if (ch == '(' || ch == '[' || ch == '{') {
            stack.push(ch);
        } else if (ch == ')' || ch == ']' || ch == '}') {
            if (stack.isEmpty() || stack.peek() != pairs.get(ch)) {
                return false;
            }
            stack.pop();
        }
    }

    return stack.isEmpty();
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <stack>
#include <string>
#include <unordered_map>

bool isValidParentheses(const std::string& s) {
    std::stack<char> stk;
    std::unordered_map<char, char> pairs = {
        {')', '('}, {']', '['}, {'}', '{'}
    };

    for (char ch : s) {
        if (ch == '(' || ch == '[' || ch == '{') {
            stk.push(ch);
        } else if (ch == ')' || ch == ']' || ch == '}') {
            if (stk.empty() || stk.top() != pairs[ch]) {
                return false;
            }
            stk.pop();
        }
    }

    return stk.empty();
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System.Collections.Generic;

public static bool IsValidParentheses(string s) {
    var stack = new Stack<char>();
    var pairs = new Dictionary<char, char> {
        {')', '('}, {']', '['}, {'}', '{'}
    };

    foreach (char ch in s) {
        if (ch == '(' || ch == '[' || ch == '{') {
            stack.Push(ch);
        } else if (ch == ')' || ch == ']' || ch == '}') {
            if (stack.Count == 0 || stack.Peek() != pairs[ch]) {
                return false;
            }
            stack.Pop();
        }
    }

    return stack.Count == 0;
}
```
{% endtab %}
{% endtabs %}

测试示例：

{% tabs %}
{% tab title="Python" %}
```python
print(is_valid_parentheses("({[]})"))   # True
print(is_valid_parentheses("([)]"))     # False
print(is_valid_parentheses("(("))       # False
```
{% endtab %}
{% tab title="Java" %}
```java
System.out.println(isValidParentheses("({[]})"));   // true
System.out.println(isValidParentheses("([)]"));     // false
System.out.println(isValidParentheses("(("));       // false
```
{% endtab %}
{% tab title="C++" %}
```cpp
std::cout << std::boolalpha;
std::cout << isValidParentheses("({[]})") << std::endl;   // true
std::cout << isValidParentheses("([)]") << std::endl;     // false
std::cout << isValidParentheses("((") << std::endl;       // false
```
{% endtab %}
{% tab title="C#" %}
```csharp
Console.WriteLine(IsValidParentheses("({[]})"));   // True
Console.WriteLine(IsValidParentheses("([)]"));     // False
Console.WriteLine(IsValidParentheses("(("));       // False
```
{% endtab %}
{% endtabs %}

### 应用二：函数调用栈

程序在执行函数调用时，操作系统使用一个栈来管理调用过程。每调用一个函数，就将该函数的上下文（局部变量、返回地址等）压入栈中；函数执行完毕后，从栈顶弹出，恢复上一层函数的执行。

{% tabs %}
{% tab title="Python" %}
```python
def func_a():
    print("进入 func_a")
    func_b()
    print("返回 func_a")

def func_b():
    print("进入 func_b")
    func_c()
    print("返回 func_b")

def func_c():
    print("进入 func_c")
    print("返回 func_c")

func_a()
```
{% endtab %}
{% tab title="Java" %}
```java
public class CallStackDemo {
    static void funcA() {
        System.out.println("进入 funcA");
        funcB();
        System.out.println("返回 funcA");
    }

    static void funcB() {
        System.out.println("进入 funcB");
        funcC();
        System.out.println("返回 funcB");
    }

    static void funcC() {
        System.out.println("进入 funcC");
        System.out.println("返回 funcC");
    }

    public static void main(String[] args) {
        funcA();
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <iostream>

void funcC() {
    std::cout << "进入 funcC" << std::endl;
    std::cout << "返回 funcC" << std::endl;
}

void funcB() {
    std::cout << "进入 funcB" << std::endl;
    funcC();
    std::cout << "返回 funcB" << std::endl;
}

void funcA() {
    std::cout << "进入 funcA" << std::endl;
    funcB();
    std::cout << "返回 funcA" << std::endl;
}

int main() {
    funcA();
    return 0;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;

class CallStackDemo {
    static void FuncA() {
        Console.WriteLine("进入 FuncA");
        FuncB();
        Console.WriteLine("返回 FuncA");
    }

    static void FuncB() {
        Console.WriteLine("进入 FuncB");
        FuncC();
        Console.WriteLine("返回 FuncB");
    }

    static void FuncC() {
        Console.WriteLine("进入 FuncC");
        Console.WriteLine("返回 FuncC");
    }

    static void Main() {
        FuncA();
    }
}
```
{% endtab %}
{% endtabs %}

执行过程中调用栈的变化：

```
调用 func_a:  | func_a |
调用 func_b:  | func_b |
              | func_a |
调用 func_c:  | func_c |
              | func_b |
              | func_a |
func_c 返回:  | func_b |
              | func_a |
func_b 返回:  | func_a |
func_a 返回:  （空栈）
```

递归本质上就是函数不断调用自身，导致调用栈不断增长。如果递归深度过大，栈空间耗尽，就会出现栈溢出（Stack Overflow）错误。

### 应用三：表达式求值

栈可以用于计算后缀表达式（Reverse Polish Notation，逆波兰表达式）。后缀表达式将运算符写在操作数之后，不需要括号就能表达运算优先级。

例如，中缀表达式 `(3 + 4) * 5` 对应的后缀表达式是 `3 4 + 5 *`。

求值规则：从左到右扫描，遇到数字就入栈，遇到运算符就弹出两个数进行计算，将结果压回栈中。

{% tabs %}
{% tab title="Python" %}
```python
def eval_postfix(expression):
    stack = []
    tokens = expression.split()

    for token in tokens:
        if token in '+-*/':
            b = stack.pop()  # 注意：先弹出的是右操作数
            a = stack.pop()
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            elif token == '/':
                stack.append(int(a / b))
        else:
            stack.append(int(token))

    return stack[0]
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.ArrayDeque;
import java.util.Deque;

public static int evalPostfix(String expression) {
    Deque<Integer> stack = new ArrayDeque<>();
    String[] tokens = expression.split(" ");

    for (String token : tokens) {
        if ("+-*/".contains(token)) {
            int b = stack.pop();  // 注意：先弹出的是右操作数
            int a = stack.pop();
            switch (token) {
                case "+": stack.push(a + b); break;
                case "-": stack.push(a - b); break;
                case "*": stack.push(a * b); break;
                case "/": stack.push(a / b); break;
            }
        } else {
            stack.push(Integer.parseInt(token));
        }
    }

    return stack.peek();
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <stack>
#include <string>
#include <sstream>

int evalPostfix(const std::string& expression) {
    std::stack<int> stk;
    std::istringstream iss(expression);
    std::string token;

    while (iss >> token) {
        if (token == "+" || token == "-" || token == "*" || token == "/") {
            int b = stk.top(); stk.pop();  // 注意：先弹出的是右操作数
            int a = stk.top(); stk.pop();
            if (token == "+") stk.push(a + b);
            else if (token == "-") stk.push(a - b);
            else if (token == "*") stk.push(a * b);
            else if (token == "/") stk.push(a / b);
        } else {
            stk.push(std::stoi(token));
        }
    }

    return stk.top();
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;
using System.Collections.Generic;

public static int EvalPostfix(string expression) {
    var stack = new Stack<int>();
    string[] tokens = expression.Split(' ');

    foreach (string token in tokens) {
        if ("+-*/".Contains(token)) {
            int b = stack.Pop();  // 注意：先弹出的是右操作数
            int a = stack.Pop();
            switch (token) {
                case "+": stack.Push(a + b); break;
                case "-": stack.Push(a - b); break;
                case "*": stack.Push(a * b); break;
                case "/": stack.Push(a / b); break;
            }
        } else {
            stack.Push(int.Parse(token));
        }
    }

    return stack.Peek();
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Python" %}
```python
print(eval_postfix("3 4 + 5 *"))    # (3+4)*5 = 35
print(eval_postfix("5 1 2 + 4 * + 3 -"))  # 5+(1+2)*4-3 = 14
```
{% endtab %}
{% tab title="Java" %}
```java
System.out.println(evalPostfix("3 4 + 5 *"));    // (3+4)*5 = 35
System.out.println(evalPostfix("5 1 2 + 4 * + 3 -"));  // 5+(1+2)*4-3 = 14
```
{% endtab %}
{% tab title="C++" %}
```cpp
std::cout << evalPostfix("3 4 + 5 *") << std::endl;    // (3+4)*5 = 35
std::cout << evalPostfix("5 1 2 + 4 * + 3 -") << std::endl;  // 5+(1+2)*4-3 = 14
```
{% endtab %}
{% tab title="C#" %}
```csharp
Console.WriteLine(EvalPostfix("3 4 + 5 *"));    // (3+4)*5 = 35
Console.WriteLine(EvalPostfix("5 1 2 + 4 * + 3 -"));  // 5+(1+2)*4-3 = 14
```
{% endtab %}
{% endtabs %}

### 应用四：浏览器前进后退

浏览器的前进和后退功能可以用两个栈来实现：

- **后退栈**：存储已浏览的页面历史
- **前进栈**：存储后退操作后可以前进的页面

{% tabs %}
{% tab title="Python" %}
```python
class Browser:
    def __init__(self):
        self.back_stack = []
        self.forward_stack = []
        self.current = None

    def visit(self, url):
        if self.current is not None:
            self.back_stack.append(self.current)
        self.current = url
        self.forward_stack.clear()  # 访问新页面后清空前进栈

    def back(self):
        if not self.back_stack:
            return
        self.forward_stack.append(self.current)
        self.current = self.back_stack.pop()

    def forward(self):
        if not self.forward_stack:
            return
        self.back_stack.append(self.current)
        self.current = self.forward_stack.pop()
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Browser {
    private Deque<String> backStack = new ArrayDeque<>();
    private Deque<String> forwardStack = new ArrayDeque<>();
    private String current = null;

    public void visit(String url) {
        if (current != null) {
            backStack.push(current);
        }
        current = url;
        forwardStack.clear();  // 访问新页面后清空前进栈
    }

    public void back() {
        if (backStack.isEmpty()) {
            return;
        }
        forwardStack.push(current);
        current = backStack.pop();
    }

    public void forward() {
        if (forwardStack.isEmpty()) {
            return;
        }
        backStack.push(current);
        current = forwardStack.pop();
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <stack>
#include <string>

class Browser {
private:
    std::stack<std::string> backStack;
    std::stack<std::string> forwardStack;
    std::string current;
    bool hasCurrent = false;

public:
    void visit(const std::string& url) {
        if (hasCurrent) {
            backStack.push(current);
        }
        current = url;
        hasCurrent = true;
        while (!forwardStack.empty()) {
            forwardStack.pop();  // 访问新页面后清空前进栈
        }
    }

    void back() {
        if (backStack.empty()) {
            return;
        }
        forwardStack.push(current);
        current = backStack.top();
        backStack.pop();
    }

    void forward() {
        if (forwardStack.empty()) {
            return;
        }
        backStack.push(current);
        current = forwardStack.top();
        forwardStack.pop();
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System.Collections.Generic;

public class Browser {
    private Stack<string> _backStack = new Stack<string>();
    private Stack<string> _forwardStack = new Stack<string>();
    private string _current = null;

    public void Visit(string url) {
        if (_current != null) {
            _backStack.Push(_current);
        }
        _current = url;
        _forwardStack.Clear();  // 访问新页面后清空前进栈
    }

    public void Back() {
        if (_backStack.Count == 0) {
            return;
        }
        _forwardStack.Push(_current);
        _current = _backStack.Pop();
    }

    public void Forward() {
        if (_forwardStack.Count == 0) {
            return;
        }
        _backStack.Push(_current);
        _current = _forwardStack.Pop();
    }
}
```
{% endtab %}
{% endtabs %}

### 应用五：撤销操作

文本编辑器的撤销（Undo）功能同样基于栈。每次编辑操作被记录并压入栈中，撤销时弹出最近的操作并执行其逆操作。

## 本章小结

- 栈是一种后进先出（LIFO）的线性数据结构，只允许在栈顶进行操作
- 核心操作 push、pop、peek 的时间复杂度均为 O(1)
- 栈可以基于数组或链表实现，数组实现更为常用
- 栈的典型应用包括括号匹配、函数调用栈、表达式求值、浏览器导航等
- 递归的底层机制就是调用栈，理解栈有助于理解递归
