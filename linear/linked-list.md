# 链表

## 什么是链表

在上一章中我们了解到，数组将元素存储在连续的内存空间中。这带来了随机访问的优势，但也导致插入和删除操作需要移动大量元素。

链表（Linked List）采用了完全不同的思路：每个元素可以存储在内存的**任意位置**，元素之间通过**指针**相互连接。每个元素被封装成一个节点（Node），节点中除了存储数据本身，还存储指向下一个节点的地址。

```
节点结构:
+------+------+
| 数据 | 指针 |
+------+------+

链表示意:
+---+---+    +---+---+    +---+---+    +---+------+
| 10| -----→ | 20| -----→ | 30| -----→ | 40| None |
+---+---+    +---+---+    +---+---+    +---+------+
  head
```

与数组相比，链表不需要连续内存，插入和删除只需要修改指针，不需要移动任何数据。代价是失去了随机访问能力——要访问第 i 个元素，必须从头节点开始逐个遍历。

## 节点的定义

链表的基本组成单元是节点。以下是最简单的节点定义：

{% tabs %}
{% tab title="Python" %}
```python
class Node:
    def __init__(self, value):
        self.value = value  # 存储的数据
        self.next = None    # 指向下一个节点的指针
```
{% endtab %}
{% tab title="Java" %}
```java
class Node {
    int value;   // 存储的数据
    Node next;   // 指向下一个节点的指针

    Node(int value) {
        this.value = value;
        this.next = null;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
struct Node {
    int value;     // 存储的数据
    Node* next;    // 指向下一个节点的指针

    Node(int value) : value(value), next(nullptr) {}
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
class Node {
    public int Value;   // 存储的数据
    public Node Next;   // 指向下一个节点的指针

    public Node(int value) {
        Value = value;
        Next = null;
    }
}
```
{% endtab %}
{% endtabs %}

多个节点通过 `next` 指针串联起来，就形成了链表。第一个节点称为头节点（Head），最后一个节点的 `next` 指向 `None`，表示链表结束。

## 单链表

单链表（Singly Linked List）是最基础的链表形式，每个节点只有一个指向后继节点的指针。

### 基本操作实现

{% tabs %}
{% tab title="Python" %}
```python
class SinglyLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0
```
{% endtab %}
{% tab title="Java" %}
```java
class SinglyLinkedList {
    Node head;
    int size;

    SinglyLinkedList() {
        head = null;
        size = 0;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
class SinglyLinkedList {
public:
    Node* head;
    int size;

    SinglyLinkedList() : head(nullptr), size(0) {}
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
class SinglyLinkedList {
    public Node Head;
    public int Size;

    public SinglyLinkedList() {
        Head = null;
        Size = 0;
    }
}
```
{% endtab %}
{% endtabs %}

**遍历链表**

从头节点出发，沿着 `next` 指针逐个访问，直到遇到 `None`。

{% tabs %}
{% tab title="Python" %}
```python
def traverse(self):
    current = self.head
    while current is not None:
        print(current.value, end=" -> ")
        current = current.next
    print("None")
```
{% endtab %}
{% tab title="Java" %}
```java
void traverse() {
    Node current = head;
    while (current != null) {
        System.out.print(current.value + " -> ");
        current = current.next;
    }
    System.out.println("None");
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
void traverse() {
    Node* current = head;
    while (current != nullptr) {
        std::cout << current->value << " -> ";
        current = current->next;
    }
    std::cout << "None" << std::endl;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public void Traverse() {
    Node current = Head;
    while (current != null) {
        Console.Write(current.Value + " -> ");
        current = current.Next;
    }
    Console.WriteLine("None");
}
```
{% endtab %}
{% endtabs %}

**头部插入**

在链表头部插入新节点只需要两步：让新节点的 `next` 指向当前头节点，然后更新头节点为新节点。时间复杂度 O(1)。

{% tabs %}
{% tab title="Python" %}
```python
def insert_at_head(self, value):
    new_node = Node(value)
    new_node.next = self.head
    self.head = new_node
    self.size += 1
```
{% endtab %}
{% tab title="Java" %}
```java
void insertAtHead(int value) {
    Node newNode = new Node(value);
    newNode.next = head;
    head = newNode;
    size++;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
void insertAtHead(int value) {
    Node* newNode = new Node(value);
    newNode->next = head;
    head = newNode;
    size++;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public void InsertAtHead(int value) {
    Node newNode = new Node(value);
    newNode.Next = Head;
    Head = newNode;
    Size++;
}
```
{% endtab %}
{% endtabs %}

```
插入前:  head → [20] → [30] → None
插入 10: head → [10] → [20] → [30] → None
```

**尾部插入**

需要先遍历到最后一个节点，再将其 `next` 指向新节点。时间复杂度 O(n)。

{% tabs %}
{% tab title="Python" %}
```python
def insert_at_tail(self, value):
    new_node = Node(value)
    if self.head is None:
        self.head = new_node
    else:
        current = self.head
        while current.next is not None:
            current = current.next
        current.next = new_node
    self.size += 1
```
{% endtab %}
{% tab title="Java" %}
```java
void insertAtTail(int value) {
    Node newNode = new Node(value);
    if (head == null) {
        head = newNode;
    } else {
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        current.next = newNode;
    }
    size++;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
void insertAtTail(int value) {
    Node* newNode = new Node(value);
    if (head == nullptr) {
        head = newNode;
    } else {
        Node* current = head;
        while (current->next != nullptr) {
            current = current->next;
        }
        current->next = newNode;
    }
    size++;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public void InsertAtTail(int value) {
    Node newNode = new Node(value);
    if (Head == null) {
        Head = newNode;
    } else {
        Node current = Head;
        while (current.Next != null) {
            current = current.Next;
        }
        current.Next = newNode;
    }
    Size++;
}
```
{% endtab %}
{% endtabs %}

**在指定位置插入**

先遍历到目标位置的前一个节点，然后调整指针。

{% tabs %}
{% tab title="Python" %}
```python
def insert_at(self, index, value):
    if index < 0 or index > self.size:
        raise IndexError("下标越界")
    if index == 0:
        self.insert_at_head(value)
        return
    new_node = Node(value)
    current = self.head
    for _ in range(index - 1):
        current = current.next
    new_node.next = current.next
    current.next = new_node
    self.size += 1
```
{% endtab %}
{% tab title="Java" %}
```java
void insertAt(int index, int value) {
    if (index < 0 || index > size) {
        throw new IndexOutOfBoundsException("下标越界");
    }
    if (index == 0) {
        insertAtHead(value);
        return;
    }
    Node newNode = new Node(value);
    Node current = head;
    for (int i = 0; i < index - 1; i++) {
        current = current.next;
    }
    newNode.next = current.next;
    current.next = newNode;
    size++;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
void insertAt(int index, int value) {
    if (index < 0 || index > size) {
        throw std::out_of_range("下标越界");
    }
    if (index == 0) {
        insertAtHead(value);
        return;
    }
    Node* newNode = new Node(value);
    Node* current = head;
    for (int i = 0; i < index - 1; i++) {
        current = current->next;
    }
    newNode->next = current->next;
    current->next = newNode;
    size++;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public void InsertAt(int index, int value) {
    if (index < 0 || index > Size) {
        throw new IndexOutOfRangeException("下标越界");
    }
    if (index == 0) {
        InsertAtHead(value);
        return;
    }
    Node newNode = new Node(value);
    Node current = Head;
    for (int i = 0; i < index - 1; i++) {
        current = current.Next;
    }
    newNode.Next = current.Next;
    current.Next = newNode;
    Size++;
}
```
{% endtab %}
{% endtabs %}

```
在下标 2 插入 25:
插入前:  [10] → [20] → [30] → [40] → None
                  ↑ current

步骤 1: new_node.next = current.next
        new_node([25]) → [30]

步骤 2: current.next = new_node
        [20] → [25] → [30]

插入后:  [10] → [20] → [25] → [30] → [40] → None
```

**删除头部节点**

将头节点更新为原头节点的下一个节点即可。时间复杂度 O(1)。

{% tabs %}
{% tab title="Python" %}
```python
def remove_at_head(self):
    if self.head is None:
        raise IndexError("链表为空")
    value = self.head.value
    self.head = self.head.next
    self.size -= 1
    return value
```
{% endtab %}
{% tab title="Java" %}
```java
int removeAtHead() {
    if (head == null) {
        throw new IndexOutOfBoundsException("链表为空");
    }
    int value = head.value;
    head = head.next;
    size--;
    return value;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
int removeAtHead() {
    if (head == nullptr) {
        throw std::out_of_range("链表为空");
    }
    int value = head->value;
    Node* temp = head;
    head = head->next;
    delete temp;
    size--;
    return value;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public int RemoveAtHead() {
    if (Head == null) {
        throw new InvalidOperationException("链表为空");
    }
    int value = Head.Value;
    Head = Head.Next;
    Size--;
    return value;
}
```
{% endtab %}
{% endtabs %}

**删除指定位置的节点**

遍历到目标位置的前一个节点，将其 `next` 跳过目标节点，直接指向目标节点的下一个节点。

{% tabs %}
{% tab title="Python" %}
```python
def remove_at(self, index):
    if index < 0 or index >= self.size:
        raise IndexError("下标越界")
    if index == 0:
        return self.remove_at_head()
    current = self.head
    for _ in range(index - 1):
        current = current.next
    value = current.next.value
    current.next = current.next.next
    self.size -= 1
    return value
```
{% endtab %}
{% tab title="Java" %}
```java
int removeAt(int index) {
    if (index < 0 || index >= size) {
        throw new IndexOutOfBoundsException("下标越界");
    }
    if (index == 0) {
        return removeAtHead();
    }
    Node current = head;
    for (int i = 0; i < index - 1; i++) {
        current = current.next;
    }
    int value = current.next.value;
    current.next = current.next.next;
    size--;
    return value;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
int removeAt(int index) {
    if (index < 0 || index >= size) {
        throw std::out_of_range("下标越界");
    }
    if (index == 0) {
        return removeAtHead();
    }
    Node* current = head;
    for (int i = 0; i < index - 1; i++) {
        current = current->next;
    }
    Node* temp = current->next;
    int value = temp->value;
    current->next = temp->next;
    delete temp;
    size--;
    return value;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public int RemoveAt(int index) {
    if (index < 0 || index >= Size) {
        throw new IndexOutOfRangeException("下标越界");
    }
    if (index == 0) {
        return RemoveAtHead();
    }
    Node current = Head;
    for (int i = 0; i < index - 1; i++) {
        current = current.Next;
    }
    int value = current.Next.Value;
    current.Next = current.Next.Next;
    Size--;
    return value;
}
```
{% endtab %}
{% endtabs %}

```
删除下标 2 的节点:
删除前:  [10] → [20] → [25] → [30] → None
                  ↑ current

current.next = current.next.next
        [20] ─────────→ [30]

删除后:  [10] → [20] → [30] → None
```

**查找元素**

从头开始逐个比较，时间复杂度 O(n)。

{% tabs %}
{% tab title="Python" %}
```python
def search(self, target):
    current = self.head
    index = 0
    while current is not None:
        if current.value == target:
            return index
        current = current.next
        index += 1
    return -1
```
{% endtab %}
{% tab title="Java" %}
```java
int search(int target) {
    Node current = head;
    int index = 0;
    while (current != null) {
        if (current.value == target) {
            return index;
        }
        current = current.next;
        index++;
    }
    return -1;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
int search(int target) {
    Node* current = head;
    int index = 0;
    while (current != nullptr) {
        if (current->value == target) {
            return index;
        }
        current = current->next;
        index++;
    }
    return -1;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
public int Search(int target) {
    Node current = Head;
    int index = 0;
    while (current != null) {
        if (current.Value == target) {
            return index;
        }
        current = current.Next;
        index++;
    }
    return -1;
}
```
{% endtab %}
{% endtabs %}

### 单链表操作复杂度

| 操作 | 时间复杂度 | 说明 |
| --- | --- | --- |
| 头部插入 | O(1) | 修改头指针 |
| 尾部插入 | O(n) | 需要遍历到末尾 |
| 指定位置插入 | O(n) | 需要遍历到前驱节点 |
| 头部删除 | O(1) | 修改头指针 |
| 指定位置删除 | O(n) | 需要遍历到前驱节点 |
| 按下标访问 | O(n) | 需要从头遍历 |
| 查找 | O(n) | 需要逐个比较 |

## 双向链表

单链表的一个限制是只能从前往后遍历。如果需要访问某个节点的前驱，就必须从头再遍历一遍。双向链表（Doubly Linked List）通过在每个节点中增加一个指向前驱的指针来解决这个问题。

```
双向链表节点:
+------+------+------+
| prev | 数据 | next |
+------+------+------+

双向链表示意:
None ← [10] ⇄ [20] ⇄ [30] ⇄ [40] → None
        head                    tail
```

### 节点定义

{% tabs %}
{% tab title="Python" %}
```python
class DoublyNode:
    def __init__(self, value):
        self.value = value
        self.prev = None   # 指向前驱节点
        self.next = None   # 指向后继节点
```
{% endtab %}
{% tab title="Java" %}
```java
class DoublyNode {
    int value;
    DoublyNode prev;   // 指向前驱节点
    DoublyNode next;   // 指向后继节点

    DoublyNode(int value) {
        this.value = value;
        this.prev = null;
        this.next = null;
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
struct DoublyNode {
    int value;
    DoublyNode* prev;   // 指向前驱节点
    DoublyNode* next;   // 指向后继节点

    DoublyNode(int value) : value(value), prev(nullptr), next(nullptr) {}
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
class DoublyNode {
    public int Value;
    public DoublyNode Prev;   // 指向前驱节点
    public DoublyNode Next;   // 指向后继节点

    public DoublyNode(int value) {
        Value = value;
        Prev = null;
        Next = null;
    }
}
```
{% endtab %}
{% endtabs %}

### 双向链表实现

{% tabs %}
{% tab title="Python" %}
```python
class DoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0

    def insert_at_head(self, value):
        new_node = DoublyNode(value)
        if self.head is None:
            self.head = new_node
            self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node
        self.size += 1

    def insert_at_tail(self, value):
        new_node = DoublyNode(value)
        if self.tail is None:
            self.head = new_node
            self.tail = new_node
        else:
            new_node.prev = self.tail
            self.tail.next = new_node
            self.tail = new_node
        self.size += 1

    def remove_at_head(self):
        if self.head is None:
            raise IndexError("链表为空")
        value = self.head.value
        if self.head == self.tail:
            self.head = None
            self.tail = None
        else:
            self.head = self.head.next
            self.head.prev = None
        self.size -= 1
        return value

    def remove_at_tail(self):
        if self.tail is None:
            raise IndexError("链表为空")
        value = self.tail.value
        if self.head == self.tail:
            self.head = None
            self.tail = None
        else:
            self.tail = self.tail.prev
            self.tail.next = None
        self.size -= 1
        return value
```
{% endtab %}
{% tab title="Java" %}
```java
class DoublyLinkedList {
    DoublyNode head;
    DoublyNode tail;
    int size;

    DoublyLinkedList() {
        head = null;
        tail = null;
        size = 0;
    }

    void insertAtHead(int value) {
        DoublyNode newNode = new DoublyNode(value);
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

    void insertAtTail(int value) {
        DoublyNode newNode = new DoublyNode(value);
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

    int removeAtHead() {
        if (head == null) {
            throw new IndexOutOfBoundsException("链表为空");
        }
        int value = head.value;
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

    int removeAtTail() {
        if (tail == null) {
            throw new IndexOutOfBoundsException("链表为空");
        }
        int value = tail.value;
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
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
class DoublyLinkedList {
public:
    DoublyNode* head;
    DoublyNode* tail;
    int size;

    DoublyLinkedList() : head(nullptr), tail(nullptr), size(0) {}

    void insertAtHead(int value) {
        DoublyNode* newNode = new DoublyNode(value);
        if (head == nullptr) {
            head = newNode;
            tail = newNode;
        } else {
            newNode->next = head;
            head->prev = newNode;
            head = newNode;
        }
        size++;
    }

    void insertAtTail(int value) {
        DoublyNode* newNode = new DoublyNode(value);
        if (tail == nullptr) {
            head = newNode;
            tail = newNode;
        } else {
            newNode->prev = tail;
            tail->next = newNode;
            tail = newNode;
        }
        size++;
    }

    int removeAtHead() {
        if (head == nullptr) {
            throw std::out_of_range("链表为空");
        }
        int value = head->value;
        DoublyNode* temp = head;
        if (head == tail) {
            head = nullptr;
            tail = nullptr;
        } else {
            head = head->next;
            head->prev = nullptr;
        }
        delete temp;
        size--;
        return value;
    }

    int removeAtTail() {
        if (tail == nullptr) {
            throw std::out_of_range("链表为空");
        }
        int value = tail->value;
        DoublyNode* temp = tail;
        if (head == tail) {
            head = nullptr;
            tail = nullptr;
        } else {
            tail = tail->prev;
            tail->next = nullptr;
        }
        delete temp;
        size--;
        return value;
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
class DoublyLinkedList {
    public DoublyNode Head;
    public DoublyNode Tail;
    public int Size;

    public DoublyLinkedList() {
        Head = null;
        Tail = null;
        Size = 0;
    }

    public void InsertAtHead(int value) {
        DoublyNode newNode = new DoublyNode(value);
        if (Head == null) {
            Head = newNode;
            Tail = newNode;
        } else {
            newNode.Next = Head;
            Head.Prev = newNode;
            Head = newNode;
        }
        Size++;
    }

    public void InsertAtTail(int value) {
        DoublyNode newNode = new DoublyNode(value);
        if (Tail == null) {
            Head = newNode;
            Tail = newNode;
        } else {
            newNode.Prev = Tail;
            Tail.Next = newNode;
            Tail = newNode;
        }
        Size++;
    }

    public int RemoveAtHead() {
        if (Head == null) {
            throw new InvalidOperationException("链表为空");
        }
        int value = Head.Value;
        if (Head == Tail) {
            Head = null;
            Tail = null;
        } else {
            Head = Head.Next;
            Head.Prev = null;
        }
        Size--;
        return value;
    }

    public int RemoveAtTail() {
        if (Tail == null) {
            throw new InvalidOperationException("链表为空");
        }
        int value = Tail.Value;
        if (Head == Tail) {
            Head = null;
            Tail = null;
        } else {
            Tail = Tail.Prev;
            Tail.Next = null;
        }
        Size--;
        return value;
    }
}
```
{% endtab %}
{% endtabs %}

### 双向链表的优势

与单链表相比，双向链表在以下操作上更高效：

| 操作 | 单链表 | 双向链表 |
| --- | --- | --- |
| 尾部插入 | O(n) | O(1)（有 tail 指针） |
| 尾部删除 | O(n) | O(1)（有 tail 指针） |
| 删除已知节点 | O(n)（需找前驱） | O(1)（直接访问前驱） |
| 反向遍历 | 不支持 | O(n) |

代价是每个节点多占用一个指针的空间。

## 循环链表

循环链表（Circular Linked List）的最后一个节点不指向 `None`，而是指回头节点，形成一个环。

```
单向循环链表:
+---+---+    +---+---+    +---+---+
| 10| -----→ | 20| -----→ | 30| --+
+---+---+    +---+---+    +---+---+|
  ↑ head                           |
  +--------------------------------+
```

循环链表适合处理具有环形特征的问题，例如：

- 约瑟夫环问题：n 个人围成一圈，从第一个人开始报数，每数到第 k 个人将其淘汰，求最后剩下的人
- 循环缓冲区：音视频流的环形缓冲
- 轮转调度：操作系统中进程的轮询调度

{% tabs %}
{% tab title="Python" %}
```python
class CircularLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0

    def insert(self, value):
        new_node = Node(value)
        if self.head is None:
            self.head = new_node
            new_node.next = self.head  # 指向自身
        else:
            current = self.head
            while current.next != self.head:
                current = current.next
            current.next = new_node
            new_node.next = self.head
        self.size += 1

    def traverse(self):
        if self.head is None:
            return
        current = self.head
        while True:
            print(current.value, end=" -> ")
            current = current.next
            if current == self.head:
                break
        print("(回到头节点)")
```
{% endtab %}
{% tab title="Java" %}
```java
class CircularLinkedList {
    Node head;
    int size;

    CircularLinkedList() {
        head = null;
        size = 0;
    }

    void insert(int value) {
        Node newNode = new Node(value);
        if (head == null) {
            head = newNode;
            newNode.next = head;  // 指向自身
        } else {
            Node current = head;
            while (current.next != head) {
                current = current.next;
            }
            current.next = newNode;
            newNode.next = head;
        }
        size++;
    }

    void traverse() {
        if (head == null) {
            return;
        }
        Node current = head;
        do {
            System.out.print(current.value + " -> ");
            current = current.next;
        } while (current != head);
        System.out.println("(回到头节点)");
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
class CircularLinkedList {
public:
    Node* head;
    int size;

    CircularLinkedList() : head(nullptr), size(0) {}

    void insert(int value) {
        Node* newNode = new Node(value);
        if (head == nullptr) {
            head = newNode;
            newNode->next = head;  // 指向自身
        } else {
            Node* current = head;
            while (current->next != head) {
                current = current->next;
            }
            current->next = newNode;
            newNode->next = head;
        }
        size++;
    }

    void traverse() {
        if (head == nullptr) {
            return;
        }
        Node* current = head;
        do {
            std::cout << current->value << " -> ";
            current = current->next;
        } while (current != head);
        std::cout << "(回到头节点)" << std::endl;
    }
};
```
{% endtab %}
{% tab title="C#" %}
```csharp
class CircularLinkedList {
    public Node Head;
    public int Size;

    public CircularLinkedList() {
        Head = null;
        Size = 0;
    }

    public void Insert(int value) {
        Node newNode = new Node(value);
        if (Head == null) {
            Head = newNode;
            newNode.Next = Head;  // 指向自身
        } else {
            Node current = Head;
            while (current.Next != Head) {
                current = current.Next;
            }
            current.Next = newNode;
            newNode.Next = Head;
        }
        Size++;
    }

    public void Traverse() {
        if (Head == null) {
            return;
        }
        Node current = Head;
        do {
            Console.Write(current.Value + " -> ");
            current = current.Next;
        } while (current != Head);
        Console.WriteLine("(回到头节点)");
    }
}
```
{% endtab %}
{% endtabs %}

## 链表与数组的对比

| 比较维度 | 数组 | 链表 |
| --- | --- | --- |
| 内存分布 | 连续 | 分散 |
| 随机访问 | O(1) | O(n) |
| 头部插入/删除 | O(n) | O(1) |
| 尾部插入/删除 | O(1)（动态数组） | O(1)（双向链表） |
| 中间插入/删除 | O(n) | O(1)（已知位置时） |
| 空间开销 | 无额外开销 | 每个节点需要存储指针 |
| 缓存友好性 | 好（连续内存） | 差（内存分散） |

选择建议：

- 频繁按下标访问 → 数组
- 频繁在头部或中间插入删除 → 链表
- 数据量不确定且变化剧烈 → 链表
- 需要高缓存性能 → 数组

## 常见链表技巧

在实际使用和面试中，有几个常用的链表操作技巧值得掌握。

### 哨兵节点

在链表头部添加一个不存储实际数据的哨兵节点（Sentinel Node，也叫哑节点 Dummy Node），可以简化边界条件的处理。

{% tabs %}
{% tab title="Python" %}
```python
# 没有哨兵节点时，删除头节点需要特殊处理
def remove_value(head, target):
    if head is not None and head.value == target:
        return head.next   # 特殊情况
    current = head
    while current.next is not None:
        if current.next.value == target:
            current.next = current.next.next
            return head
        current = current.next
    return head

# 使用哨兵节点，逻辑统一
def remove_value(head, target):
    dummy = Node(0)
    dummy.next = head
    current = dummy
    while current.next is not None:
        if current.next.value == target:
            current.next = current.next.next
        else:
            current = current.next
    return dummy.next
```
{% endtab %}
{% tab title="Java" %}
```java
// 没有哨兵节点时，删除头节点需要特殊处理
Node removeValue(Node head, int target) {
    if (head != null && head.value == target) {
        return head.next;   // 特殊情况
    }
    Node current = head;
    while (current.next != null) {
        if (current.next.value == target) {
            current.next = current.next.next;
            return head;
        }
        current = current.next;
    }
    return head;
}

// 使用哨兵节点，逻辑统一
Node removeValueWithDummy(Node head, int target) {
    Node dummy = new Node(0);
    dummy.next = head;
    Node current = dummy;
    while (current.next != null) {
        if (current.next.value == target) {
            current.next = current.next.next;
        } else {
            current = current.next;
        }
    }
    return dummy.next;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
// 没有哨兵节点时，删除头节点需要特殊处理
Node* removeValue(Node* head, int target) {
    if (head != nullptr && head->value == target) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return head;   // 特殊情况
    }
    Node* current = head;
    while (current->next != nullptr) {
        if (current->next->value == target) {
            Node* temp = current->next;
            current->next = temp->next;
            delete temp;
            return head;
        }
        current = current->next;
    }
    return head;
}

// 使用哨兵节点，逻辑统一
Node* removeValueWithDummy(Node* head, int target) {
    Node* dummy = new Node(0);
    dummy->next = head;
    Node* current = dummy;
    while (current->next != nullptr) {
        if (current->next->value == target) {
            Node* temp = current->next;
            current->next = temp->next;
            delete temp;
        } else {
            current = current->next;
        }
    }
    Node* newHead = dummy->next;
    delete dummy;
    return newHead;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
// 没有哨兵节点时，删除头节点需要特殊处理
Node RemoveValue(Node head, int target) {
    if (head != null && head.Value == target) {
        return head.Next;   // 特殊情况
    }
    Node current = head;
    while (current.Next != null) {
        if (current.Next.Value == target) {
            current.Next = current.Next.Next;
            return head;
        }
        current = current.Next;
    }
    return head;
}

// 使用哨兵节点，逻辑统一
Node RemoveValueWithDummy(Node head, int target) {
    Node dummy = new Node(0);
    dummy.Next = head;
    Node current = dummy;
    while (current.Next != null) {
        if (current.Next.Value == target) {
            current.Next = current.Next.Next;
        } else {
            current = current.Next;
        }
    }
    return dummy.Next;
}
```
{% endtab %}
{% endtabs %}

### 快慢指针

使用两个指针以不同速度遍历链表，可以解决很多经典问题。

**找到链表的中间节点：** 快指针每次走两步，慢指针每次走一步。当快指针到达末尾时，慢指针恰好在中间。

{% tabs %}
{% tab title="Python" %}
```python
def find_middle(head):
    slow = head
    fast = head
    while fast is not None and fast.next is not None:
        slow = slow.next
        fast = fast.next.next
    return slow
```
{% endtab %}
{% tab title="Java" %}
```java
Node findMiddle(Node head) {
    Node slow = head;
    Node fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
Node* findMiddle(Node* head) {
    Node* slow = head;
    Node* fast = head;
    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
Node FindMiddle(Node head) {
    Node slow = head;
    Node fast = head;
    while (fast != null && fast.Next != null) {
        slow = slow.Next;
        fast = fast.Next.Next;
    }
    return slow;
}
```
{% endtab %}
{% endtabs %}

**检测链表是否有环：** 如果链表中存在环，快指针最终会追上慢指针。

{% tabs %}
{% tab title="Python" %}
```python
def has_cycle(head):
    slow = head
    fast = head
    while fast is not None and fast.next is not None:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```
{% endtab %}
{% tab title="Java" %}
```java
boolean hasCycle(Node head) {
    Node slow = head;
    Node fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
bool hasCycle(Node* head) {
    Node* slow = head;
    Node* fast = head;
    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
bool HasCycle(Node head) {
    Node slow = head;
    Node fast = head;
    while (fast != null && fast.Next != null) {
        slow = slow.Next;
        fast = fast.Next.Next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```
{% endtab %}
{% endtabs %}

## 本章小结

- 链表通过指针将分散在内存中的节点串联起来，不需要连续内存
- 单链表每个节点只有一个后继指针，双向链表增加了前驱指针
- 链表的插入和删除（已知位置时）为 O(1)，但随机访问为 O(n)
- 循环链表适合处理具有环形特征的问题
- 哨兵节点和快慢指针是链表操作中的常用技巧
- 链表适合频繁插入删除的场景，数组适合频繁随机访问的场景
