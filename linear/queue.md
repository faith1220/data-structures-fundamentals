# 队列与双端队列

## 什么是队列

队列（Queue）是另一种操作受限的线性数据结构。与栈不同，队列的插入和删除分别在两端进行：新元素从队尾（Rear）加入，已有元素从队头（Front）移出。

队列遵循**先进先出**（First In, First Out，简称 FIFO）的原则：最先加入的元素最先被取出，就像现实中的排队一样——先到的人先服务。

```
入队方向 →
              队头                        队尾
              +----+----+----+----+----+
              | 10 | 20 | 30 | 40 | 50 |
              +----+----+----+----+----+
              ↓ 出队方向
```

## 队列的基本操作

| 操作 | 说明 | 时间复杂度 |
| --- | --- | --- |
| enqueue(x) | 将元素 x 加入队尾 | O(1) |
| dequeue() | 移除并返回队头元素 | O(1) |
| peek() / front() | 查看队头元素但不移除 | O(1) |
| is_empty() | 判断队列是否为空 | O(1) |
| size() | 返回队列中元素个数 | O(1) |

```
初始:      空队列

enqueue(10):  | 10 |

enqueue(20):  | 10 | 20 |

enqueue(30):  | 10 | 20 | 30 |

dequeue():    | 20 | 30 |       返回 10

dequeue():    | 30 |            返回 20
```

## 队列的实现

### 基于链表的实现

使用链表实现队列是最直接的方式。用链表头部作为队头（出队端），尾部作为队尾（入队端），配合 tail 指针，入队和出队都是 O(1)。

{% tabs %}
{% tab title="Python" %}
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class LinkedQueue:
    def __init__(self):
        self.front = None
        self.rear = None
        self._size = 0

    def enqueue(self, value):
        new_node = Node(value)
        if self.rear is None:
            self.front = new_node
            self.rear = new_node
        else:
            self.rear.next = new_node
            self.rear = new_node
        self._size += 1

    def dequeue(self):
        if self.is_empty():
            raise IndexError("队列为空")
        value = self.front.value
        self.front = self.front.next
        if self.front is None:
            self.rear = None
        self._size -= 1
        return value

    def peek(self):
        if self.is_empty():
            raise IndexError("队列为空")
        return self.front.value

    def is_empty(self):
        return self.front is None

    def size(self):
        return self._size
```
{% endtab %}
{% tab title="Java" %}
```java
public class LinkedQueue<T> {
    private static class Node<T> {
        T value;
        Node<T> next;

        Node(T value) {
            this.value = value;
            this.next = null;
        }
    }

    private Node<T> front;
    private Node<T> rear;
    private int size;

    public LinkedQueue() {
        this.front = null;
        this.rear = null;
        this.size = 0;
    }

    public void enqueue(T value) {
        Node<T> newNode = new Node<>(value);
        if (rear == null) {
            front = newNode;
            rear = newNode;
        } else {
            rear.next = newNode;
            rear = newNode;
        }
        size++;
    }

    public T dequeue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        T value = front.value;
        front = front.next;
        if (front == null) {
            rear = null;
        }
        size--;
        return value;
    }

    public T peek() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        return front.value;
    }

    public boolean isEmpty() {
        return front == null;
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

template <typename T>
class LinkedQueue {
private:
    struct Node {
        T value;
        Node* next;
        Node(T val) : value(val), next(nullptr) {}
    };

    Node* front_;
    Node* rear_;
    int size_;

public:
    LinkedQueue() : front_(nullptr), rear_(nullptr), size_(0) {}

    ~LinkedQueue() {
        while (!isEmpty()) {
            dequeue();
        }
    }

    void enqueue(T value) {
        Node* newNode = new Node(value);
        if (rear_ == nullptr) {
            front_ = newNode;
            rear_ = newNode;
        } else {
            rear_->next = newNode;
            rear_ = newNode;
        }
        size_++;
    }

    T dequeue() {
        if (isEmpty()) {
            throw std::runtime_error("队列为空");
        }
        T value = front_->value;
        Node* temp = front_;
        front_ = front_->next;
        delete temp;
        if (front_ == nullptr) {
            rear_ = nullptr;
        }
        size_--;
        return value;
    }

    T peek() const {
        if (isEmpty()) {
            throw std::runtime_error("队列为空");
        }
        return front_->value;
    }

    bool isEmpty() const {
        return front_ == nullptr;
    }

    int size() const {
        return size_;
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;

public class LinkedQueue<T>
{
    private class Node
    {
        public T Value;
        public Node Next;

        public Node(T value)
        {
            Value = value;
            Next = null;
        }
    }

    private Node _front;
    private Node _rear;
    private int _size;

    public LinkedQueue()
    {
        _front = null;
        _rear = null;
        _size = 0;
    }

    public void Enqueue(T value)
    {
        Node newNode = new Node(value);
        if (_rear == null)
        {
            _front = newNode;
            _rear = newNode;
        }
        else
        {
            _rear.Next = newNode;
            _rear = newNode;
        }
        _size++;
    }

    public T Dequeue()
    {
        if (IsEmpty())
            throw new InvalidOperationException("队列为空");
        T value = _front.Value;
        _front = _front.Next;
        if (_front == null)
            _rear = null;
        _size--;
        return value;
    }

    public T Peek()
    {
        if (IsEmpty())
            throw new InvalidOperationException("队列为空");
        return _front.Value;
    }

    public bool IsEmpty()
    {
        return _front == null;
    }

    public int Size()
    {
        return _size;
    }
}
```
{% endtab %}
{% endtabs %}

### 基于数组的实现（朴素方式的问题）

如果直接用数组实现队列，每次出队后需要将所有元素前移一位，时间复杂度为 O(n)。另一种做法是用一个变量记录队头位置，出队时只移动队头指针，但这样会导致数组前端的空间无法复用，造成"假满"现象。

```
初始:       | 10 | 20 | 30 | 40 | 50 |
             ↑ front                ↑ rear

dequeue 两次后:
            |    |    | 30 | 40 | 50 |
                       ↑ front      ↑ rear
前面两个位置浪费了
```

解决这个问题的方案是循环队列。

### 循环队列

循环队列（Circular Queue）将数组的首尾相连，形成逻辑上的环。当 rear 到达数组末尾时，下一个位置回到数组开头，从而复用前面空出的空间。

```
逻辑视图（容量为 6）:

        +----+----+----+----+----+----+
        |    |    | 30 | 40 | 50 |    |
        +----+----+----+----+----+----+
                   ↑ front      ↑ rear

enqueue(60):
        +----+----+----+----+----+----+
        |    |    | 30 | 40 | 50 | 60 |
        +----+----+----+----+----+----+
                   ↑ front           ↑ rear

enqueue(70):  rear 回绕到位置 0
        +----+----+----+----+----+----+
        | 70 |    | 30 | 40 | 50 | 60 |
        +----+----+----+----+----+----+
         ↑ rear   ↑ front
```

关键公式：通过取模运算实现环形效果。

```
下一个位置 = (当前位置 + 1) % 容量
```

为了区分队列为空和队列已满（两种情况下 front 和 rear 可能相同），常见的做法是预留一个空位，即容量为 n 的数组最多存 n-1 个元素。

{% tabs %}
{% tab title="Python" %}
```python
class CircularQueue:
    def __init__(self, capacity):
        self._capacity = capacity + 1  # 多分配一个位置
        self._data = [None] * self._capacity
        self._front = 0
        self._rear = 0

    def enqueue(self, value):
        if self.is_full():
            raise IndexError("队列已满")
        self._data[self._rear] = value
        self._rear = (self._rear + 1) % self._capacity

    def dequeue(self):
        if self.is_empty():
            raise IndexError("队列为空")
        value = self._data[self._front]
        self._data[self._front] = None
        self._front = (self._front + 1) % self._capacity
        return value

    def peek(self):
        if self.is_empty():
            raise IndexError("队列为空")
        return self._data[self._front]

    def is_empty(self):
        return self._front == self._rear

    def is_full(self):
        return (self._rear + 1) % self._capacity == self._front

    def size(self):
        return (self._rear - self._front + self._capacity) % self._capacity
```
{% endtab %}
{% tab title="Java" %}
```java
public class CircularQueue<T> {
    private Object[] data;
    private int capacity;
    private int front;
    private int rear;

    public CircularQueue(int capacity) {
        this.capacity = capacity + 1; // 多分配一个位置
        this.data = new Object[this.capacity];
        this.front = 0;
        this.rear = 0;
    }

    public void enqueue(T value) {
        if (isFull()) {
            throw new RuntimeException("队列已满");
        }
        data[rear] = value;
        rear = (rear + 1) % capacity;
    }

    @SuppressWarnings("unchecked")
    public T dequeue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        T value = (T) data[front];
        data[front] = null;
        front = (front + 1) % capacity;
        return value;
    }

    @SuppressWarnings("unchecked")
    public T peek() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空");
        }
        return (T) data[front];
    }

    public boolean isEmpty() {
        return front == rear;
    }

    public boolean isFull() {
        return (rear + 1) % capacity == front;
    }

    public int size() {
        return (rear - front + capacity) % capacity;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>
#include <stdexcept>

template <typename T>
class CircularQueue {
private:
    std::vector<T> data_;
    int capacity_;
    int front_;
    int rear_;

public:
    CircularQueue(int capacity)
        : capacity_(capacity + 1),  // 多分配一个位置
          data_(capacity + 1),
          front_(0),
          rear_(0) {}

    void enqueue(T value) {
        if (isFull()) {
            throw std::runtime_error("队列已满");
        }
        data_[rear_] = value;
        rear_ = (rear_ + 1) % capacity_;
    }

    T dequeue() {
        if (isEmpty()) {
            throw std::runtime_error("队列为空");
        }
        T value = data_[front_];
        front_ = (front_ + 1) % capacity_;
        return value;
    }

    T peek() const {
        if (isEmpty()) {
            throw std::runtime_error("队列为空");
        }
        return data_[front_];
    }

    bool isEmpty() const {
        return front_ == rear_;
    }

    bool isFull() const {
        return (rear_ + 1) % capacity_ == front_;
    }

    int size() const {
        return (rear_ - front_ + capacity_) % capacity_;
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;

public class CircularQueue<T>
{
    private T[] _data;
    private int _capacity;
    private int _front;
    private int _rear;

    public CircularQueue(int capacity)
    {
        _capacity = capacity + 1; // 多分配一个位置
        _data = new T[_capacity];
        _front = 0;
        _rear = 0;
    }

    public void Enqueue(T value)
    {
        if (IsFull())
            throw new InvalidOperationException("队列已满");
        _data[_rear] = value;
        _rear = (_rear + 1) % _capacity;
    }

    public T Dequeue()
    {
        if (IsEmpty())
            throw new InvalidOperationException("队列为空");
        T value = _data[_front];
        _data[_front] = default;
        _front = (_front + 1) % _capacity;
        return value;
    }

    public T Peek()
    {
        if (IsEmpty())
            throw new InvalidOperationException("队列为空");
        return _data[_front];
    }

    public bool IsEmpty()
    {
        return _front == _rear;
    }

    public bool IsFull()
    {
        return (_rear + 1) % _capacity == _front;
    }

    public int Size()
    {
        return (_rear - _front + _capacity) % _capacity;
    }
}
```
{% endtab %}
{% endtabs %}

## 双端队列

双端队列（Double-Ended Queue，简称 Deque，发音 "deck"）是队列的扩展，它允许在**两端**进行插入和删除操作。

```
         队头                队尾
          ↕                   ↕
  出/入 ← | 10 | 20 | 30 | 40 | → 出/入
```

### 基本操作

| 操作 | 说明 | 时间复杂度 |
| --- | --- | --- |
| push_front(x) | 在队头插入元素 | O(1) |
| push_back(x) | 在队尾插入元素 | O(1) |
| pop_front() | 移除并返回队头元素 | O(1) |
| pop_back() | 移除并返回队尾元素 | O(1) |
| peek_front() | 查看队头元素 | O(1) |
| peek_back() | 查看队尾元素 | O(1) |

双端队列是一种更通用的数据结构：如果只使用一端操作，它就是栈；如果一端只入、另一端只出，它就是队列。

### 基于双向链表的实现

双向链表的头尾操作都是 O(1)，非常适合实现双端队列。

{% tabs %}
{% tab title="Python" %}
```python
class DoublyNode:
    def __init__(self, value):
        self.value = value
        self.prev = None
        self.next = None

class Deque:
    def __init__(self):
        self.head = None
        self.tail = None
        self._size = 0

    def push_front(self, value):
        new_node = DoublyNode(value)
        if self.head is None:
            self.head = new_node
            self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node
        self._size += 1

    def push_back(self, value):
        new_node = DoublyNode(value)
        if self.tail is None:
            self.head = new_node
            self.tail = new_node
        else:
            new_node.prev = self.tail
            self.tail.next = new_node
            self.tail = new_node
        self._size += 1

    def pop_front(self):
        if self.is_empty():
            raise IndexError("双端队列为空")
        value = self.head.value
        if self.head == self.tail:
            self.head = None
            self.tail = None
        else:
            self.head = self.head.next
            self.head.prev = None
        self._size -= 1
        return value

    def pop_back(self):
        if self.is_empty():
            raise IndexError("双端队列为空")
        value = self.tail.value
        if self.head == self.tail:
            self.head = None
            self.tail = None
        else:
            self.tail = self.tail.prev
            self.tail.next = None
        self._size -= 1
        return value

    def peek_front(self):
        if self.is_empty():
            raise IndexError("双端队列为空")
        return self.head.value

    def peek_back(self):
        if self.is_empty():
            raise IndexError("双端队列为空")
        return self.tail.value

    def is_empty(self):
        return self.head is None

    def size(self):
        return self._size
```
{% endtab %}
{% tab title="Java" %}
```java
public class Deque<T> {
    private static class DoublyNode<T> {
        T value;
        DoublyNode<T> prev;
        DoublyNode<T> next;

        DoublyNode(T value) {
            this.value = value;
            this.prev = null;
            this.next = null;
        }
    }

    private DoublyNode<T> head;
    private DoublyNode<T> tail;
    private int size;

    public Deque() {
        head = null;
        tail = null;
        size = 0;
    }

    public void pushFront(T value) {
        DoublyNode<T> newNode = new DoublyNode<>(value);
        if (head == null) {
            head = newNode;
            tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
    }

    public void pushBack(T value) {
        DoublyNode<T> newNode = new DoublyNode<>(value);
        if (tail == null) {
            head = newNode;
            tail = newNode;
        } else {
            newNode.prev = tail;
            tail.next = newNode;
            tail = newNode;
        }
        size++;
    }

    public T popFront() {
        if (isEmpty()) {
            throw new RuntimeException("双端队列为空");
        }
        T value = head.value;
        if (head == tail) {
            head = null;
            tail = null;
        } else {
            head = head.next;
            head.prev = null;
        }
        size--;
        return value;
    }

    public T popBack() {
        if (isEmpty()) {
            throw new RuntimeException("双端队列为空");
        }
        T value = tail.value;
        if (head == tail) {
            head = null;
            tail = null;
        } else {
            tail = tail.prev;
            tail.next = null;
        }
        size--;
        return value;
    }

    public T peekFront() {
        if (isEmpty()) {
            throw new RuntimeException("双端队列为空");
        }
        return head.value;
    }

    public T peekBack() {
        if (isEmpty()) {
            throw new RuntimeException("双端队列为空");
        }
        return tail.value;
    }

    public boolean isEmpty() {
        return head == null;
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

template <typename T>
class Deque {
private:
    struct DoublyNode {
        T value;
        DoublyNode* prev;
        DoublyNode* next;
        DoublyNode(T val) : value(val), prev(nullptr), next(nullptr) {}
    };

    DoublyNode* head_;
    DoublyNode* tail_;
    int size_;

public:
    Deque() : head_(nullptr), tail_(nullptr), size_(0) {}

    ~Deque() {
        while (!isEmpty()) {
            popFront();
        }
    }

    void pushFront(T value) {
        DoublyNode* newNode = new DoublyNode(value);
        if (head_ == nullptr) {
            head_ = newNode;
            tail_ = newNode;
        } else {
            newNode->next = head_;
            head_->prev = newNode;
            head_ = newNode;
        }
        size_++;
    }

    void pushBack(T value) {
        DoublyNode* newNode = new DoublyNode(value);
        if (tail_ == nullptr) {
            head_ = newNode;
            tail_ = newNode;
        } else {
            newNode->prev = tail_;
            tail_->next = newNode;
            tail_ = newNode;
        }
        size_++;
    }

    T popFront() {
        if (isEmpty()) {
            throw std::runtime_error("双端队列为空");
        }
        T value = head_->value;
        DoublyNode* temp = head_;
        if (head_ == tail_) {
            head_ = nullptr;
            tail_ = nullptr;
        } else {
            head_ = head_->next;
            head_->prev = nullptr;
        }
        delete temp;
        size_--;
        return value;
    }

    T popBack() {
        if (isEmpty()) {
            throw std::runtime_error("双端队列为空");
        }
        T value = tail_->value;
        DoublyNode* temp = tail_;
        if (head_ == tail_) {
            head_ = nullptr;
            tail_ = nullptr;
        } else {
            tail_ = tail_->prev;
            tail_->next = nullptr;
        }
        delete temp;
        size_--;
        return value;
    }

    T peekFront() const {
        if (isEmpty()) {
            throw std::runtime_error("双端队列为空");
        }
        return head_->value;
    }

    T peekBack() const {
        if (isEmpty()) {
            throw std::runtime_error("双端队列为空");
        }
        return tail_->value;
    }

    bool isEmpty() const {
        return head_ == nullptr;
    }

    int size() const {
        return size_;
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;

public class Deque<T>
{
    private class DoublyNode
    {
        public T Value;
        public DoublyNode Prev;
        public DoublyNode Next;

        public DoublyNode(T value)
        {
            Value = value;
            Prev = null;
            Next = null;
        }
    }

    private DoublyNode _head;
    private DoublyNode _tail;
    private int _size;

    public Deque()
    {
        _head = null;
        _tail = null;
        _size = 0;
    }

    public void PushFront(T value)
    {
        DoublyNode newNode = new DoublyNode(value);
        if (_head == null)
        {
            _head = newNode;
            _tail = newNode;
        }
        else
        {
            newNode.Next = _head;
            _head.Prev = newNode;
            _head = newNode;
        }
        _size++;
    }

    public void PushBack(T value)
    {
        DoublyNode newNode = new DoublyNode(value);
        if (_tail == null)
        {
            _head = newNode;
            _tail = newNode;
        }
        else
        {
            newNode.Prev = _tail;
            _tail.Next = newNode;
            _tail = newNode;
        }
        _size++;
    }

    public T PopFront()
    {
        if (IsEmpty())
            throw new InvalidOperationException("双端队列为空");
        T value = _head.Value;
        if (_head == _tail)
        {
            _head = null;
            _tail = null;
        }
        else
        {
            _head = _head.Next;
            _head.Prev = null;
        }
        _size--;
        return value;
    }

    public T PopBack()
    {
        if (IsEmpty())
            throw new InvalidOperationException("双端队列为空");
        T value = _tail.Value;
        if (_head == _tail)
        {
            _head = null;
            _tail = null;
        }
        else
        {
            _tail = _tail.Prev;
            _tail.Next = null;
        }
        _size--;
        return value;
    }

    public T PeekFront()
    {
        if (IsEmpty())
            throw new InvalidOperationException("双端队列为空");
        return _head.Value;
    }

    public T PeekBack()
    {
        if (IsEmpty())
            throw new InvalidOperationException("双端队列为空");
        return _tail.Value;
    }

    public bool IsEmpty()
    {
        return _head == null;
    }

    public int Size()
    {
        return _size;
    }
}
```
{% endtab %}
{% endtabs %}

Python 标准库中的 `collections.deque` 就是一个高性能的双端队列实现，底层使用分块的双向链表，在实际开发中建议直接使用。

{% tabs %}
{% tab title="Python" %}
```python
from collections import deque

dq = deque()
dq.append(10)        # 队尾插入
dq.appendleft(5)     # 队头插入
dq.pop()             # 队尾移除，返回 10
dq.popleft()         # 队头移除，返回 5
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.ArrayDeque;
import java.util.Deque;

Deque<Integer> dq = new ArrayDeque<>();
dq.addLast(10);      // 队尾插入
dq.addFirst(5);      // 队头插入
dq.removeLast();     // 队尾移除，返回 10
dq.removeFirst();    // 队头移除，返回 5
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <deque>

std::deque<int> dq;
dq.push_back(10);    // 队尾插入
dq.push_front(5);    // 队头插入
dq.pop_back();       // 队尾移除（值为 10）
dq.pop_front();      // 队头移除（值为 5）
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System.Collections.Generic;

// C# 没有内置 Deque，可用 LinkedList 替代
LinkedList<int> dq = new LinkedList<int>();
dq.AddLast(10);      // 队尾插入
dq.AddFirst(5);      // 队头插入
dq.RemoveLast();     // 队尾移除（值为 10）
dq.RemoveFirst();    // 队头移除（值为 5）
```
{% endtab %}
{% endtabs %}

## 队列的典型应用

### 应用一：广度优先搜索

广度优先搜索（Breadth-First Search，BFS）是图和树的一种遍历方式。它使用队列来保证按层次顺序访问节点。

{% tabs %}
{% tab title="Python" %}
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)

    while queue:
        node = queue.popleft()
        print(node, end=" ")
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.*;

public static void bfs(Map<String, List<String>> graph, String start) {
    Set<String> visited = new HashSet<>();
    Queue<String> queue = new LinkedList<>();
    queue.add(start);
    visited.add(start);

    while (!queue.isEmpty()) {
        String node = queue.poll();
        System.out.print(node + " ");
        for (String neighbor : graph.get(node)) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                queue.add(neighbor);
            }
        }
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <iostream>
#include <queue>
#include <unordered_set>
#include <unordered_map>
#include <vector>
#include <string>

void bfs(std::unordered_map<std::string, std::vector<std::string>>& graph,
         const std::string& start) {
    std::unordered_set<std::string> visited;
    std::queue<std::string> q;
    q.push(start);
    visited.insert(start);

    while (!q.empty()) {
        std::string node = q.front();
        q.pop();
        std::cout << node << " ";
        for (const auto& neighbor : graph[node]) {
            if (visited.find(neighbor) == visited.end()) {
                visited.insert(neighbor);
                q.push(neighbor);
            }
        }
    }
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;
using System.Collections.Generic;

public static void Bfs(Dictionary<string, List<string>> graph, string start)
{
    HashSet<string> visited = new HashSet<string>();
    Queue<string> queue = new Queue<string>();
    queue.Enqueue(start);
    visited.Add(start);

    while (queue.Count > 0)
    {
        string node = queue.Dequeue();
        Console.Write(node + " ");
        foreach (string neighbor in graph[node])
        {
            if (!visited.Contains(neighbor))
            {
                visited.Add(neighbor);
                queue.Enqueue(neighbor);
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

BFS 的详细内容将在[图的遍历](../hash-and-graph/graph-traversal.md)章节中展开。

### 应用二：任务调度

操作系统使用队列来管理待执行的任务。新任务加入队尾，CPU 从队头取出任务执行，保证每个任务按提交顺序得到处理。

打印机的打印队列、消息队列系统（如 RabbitMQ、Kafka）的底层逻辑也是如此。

### 应用三：滑动窗口最大值

给定一个数组和窗口大小 k，求每个窗口内的最大值。使用双端队列可以在 O(n) 时间内解决这个问题。

基本思路：维护一个双端队列，存储数组下标。队列中的下标对应的值保持递减顺序，这样队头始终是当前窗口的最大值。

{% tabs %}
{% tab title="Python" %}
```python
from collections import deque

def max_sliding_window(nums, k):
    result = []
    dq = deque()  # 存储下标

    for i in range(len(nums)):
        # 移除超出窗口范围的元素
        while dq and dq[0] < i - k + 1:
            dq.popleft()

        # 移除队尾所有比当前元素小的值
        while dq and nums[dq[-1]] < nums[i]:
            dq.pop()

        dq.append(i)

        # 窗口形成后，记录最大值
        if i >= k - 1:
            result.append(nums[dq[0]])

    return result
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.*;

public static int[] maxSlidingWindow(int[] nums, int k) {
    List<Integer> result = new ArrayList<>();
    Deque<Integer> dq = new ArrayDeque<>(); // 存储下标

    for (int i = 0; i < nums.length; i++) {
        // 移除超出窗口范围的元素
        while (!dq.isEmpty() && dq.peekFirst() < i - k + 1) {
            dq.pollFirst();
        }

        // 移除队尾所有比当前元素小的值
        while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) {
            dq.pollLast();
        }

        dq.addLast(i);

        // 窗口形成后，记录最大值
        if (i >= k - 1) {
            result.add(nums[dq.peekFirst()]);
        }
    }

    return result.stream().mapToInt(Integer::intValue).toArray();
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <vector>
#include <deque>

std::vector<int> maxSlidingWindow(std::vector<int>& nums, int k) {
    std::vector<int> result;
    std::deque<int> dq; // 存储下标

    for (int i = 0; i < (int)nums.size(); i++) {
        // 移除超出窗口范围的元素
        while (!dq.empty() && dq.front() < i - k + 1) {
            dq.pop_front();
        }

        // 移除队尾所有比当前元素小的值
        while (!dq.empty() && nums[dq.back()] < nums[i]) {
            dq.pop_back();
        }

        dq.push_back(i);

        // 窗口形成后，记录最大值
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }

    return result;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System.Collections.Generic;

public static int[] MaxSlidingWindow(int[] nums, int k)
{
    List<int> result = new List<int>();
    LinkedList<int> dq = new LinkedList<int>(); // 存储下标

    for (int i = 0; i < nums.Length; i++)
    {
        // 移除超出窗口范围的元素
        while (dq.Count > 0 && dq.First.Value < i - k + 1)
            dq.RemoveFirst();

        // 移除队尾所有比当前元素小的值
        while (dq.Count > 0 && nums[dq.Last.Value] < nums[i])
            dq.RemoveLast();

        dq.AddLast(i);

        // 窗口形成后，记录最大值
        if (i >= k - 1)
            result.Add(nums[dq.First.Value]);
    }

    return result.ToArray();
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Python" %}
```python
print(max_sliding_window([1, 3, -1, -3, 5, 3, 6, 7], 3))
# 输出: [3, 3, 5, 5, 6, 7]
```
{% endtab %}
{% tab title="Java" %}
```java
System.out.println(Arrays.toString(maxSlidingWindow(
    new int[]{1, 3, -1, -3, 5, 3, 6, 7}, 3)));
// 输出: [3, 3, 5, 5, 6, 7]
```
{% endtab %}
{% tab title="C++" %}
```cpp
std::vector<int> nums = {1, 3, -1, -3, 5, 3, 6, 7};
auto res = maxSlidingWindow(nums, 3);
for (int v : res) std::cout << v << " ";
// 输出: 3 3 5 5 6 7
```
{% endtab %}
{% tab title="C#" %}
```csharp
int[] res = MaxSlidingWindow(new int[]{1, 3, -1, -3, 5, 3, 6, 7}, 3);
Console.WriteLine(string.Join(", ", res));
// 输出: 3, 3, 5, 5, 6, 7
```
{% endtab %}
{% endtabs %}

## 栈与队列的对比

| 比较维度 | 栈 | 队列 |
| --- | --- | --- |
| 顺序原则 | 后进先出（LIFO） | 先进先出（FIFO） |
| 操作端 | 仅栈顶 | 队头出、队尾入 |
| 典型应用 | 括号匹配、函数调用 | BFS、任务调度 |
| 核心特征 | 回溯、递归 | 排队、层次遍历 |

一个简单的判断标准：如果问题具有"先处理最近的"特征，用栈；如果具有"先来先服务"特征，用队列。

## 本章小结

- 队列遵循先进先出（FIFO）原则，元素从队尾入、队头出
- 循环队列通过取模运算复用数组空间，避免"假满"现象
- 双端队列允许在两端进行插入和删除，是栈和队列的通用形式
- 队列的典型应用包括 BFS、任务调度、滑动窗口等
- Python 的 `collections.deque` 提供了高性能的双端队列实现
