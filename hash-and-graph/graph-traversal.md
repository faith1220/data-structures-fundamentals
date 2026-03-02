# 图的遍历 (DFS 与 BFS)

在前面“树形结构”的章节中，我们学过如何遍历一棵二叉树：你要么一头扎到底（深度优先 DFS），要么一层一层往下看（广度优先 BFS）。

**图的遍历和树的遍历非常相似，其核心目的也是：把图里的每一个顶点都走一遍，且只走一次。**

如果你把前一章存储在电脑里的“邻接表”画出来，你会发现图的遍历其实就是一个极其凶险的**“走迷宫”游戏**。
为什么凶险？因为**树是从上往下长的，永远不会绕回来**；但是**图里是有“环（Cycle）”的！**
如果 A 认识 B，B 认识 C，C 又认识 A。你在写遍历代码时如果没有防备，程序就会在 A、B、C 之间无限绕圈圈，直到把电脑内存撑爆，死机崩溃（Stack Overflow 栈溢出）。

为了不迷路、不死循环，我们在图的遍历中必须带上一件极其重要的**法宝：“到此一游”记号本（Visited 集合）**。

## 图遍历的唯一护身符：Visited 集合

在遍历图的每个角落时，我们必须在口袋里揣一个笔记本（在代码中通常是一个**哈希集合 HashSet** 或**布尔数组 boolean[]**）。

**核心铁律**：每当我们踏入一个顶点，第一件事就是在笔记本上写下它的名字（标为**已访问**）。当我们要顺着朋友关系去访问下一个顶点时，必须先查阅笔记本，**如果他已经在笔记本上了，绝对不要再踏进去！立马掉头！**

有了这把护身符，我们就可以把对付树的两大绝招（DFS 和 BFS）搬到图上来了。

---

## 策略一：深度优先搜索 (DFS - Depth First Search)

**思想来源**：不撞南墙不回头。
想象你正在走一个没有地图的真实地下迷宫。DFS 的策略就是：
1. 拿出一根粉笔（Visited 集合）。
2. 走进一个路口，在地上画个叉。
3. 随便挑一条没走过的岔路，**一直往死里走**，碰到路口就继续选一条没走过的往下钻。
4. 直到你走到一个死胡同，或者发现前面的所有路口地上都画着叉（全走过了）。
5. 你就**原路退回（回溯 Backtrack）**到上一个路口，去试那条还没走过的另一条岔路。

在代码实现上，这种“走到黑再退回来”的过程，简直就是为**递归（Recursion）**量身定制的。

---

## 策略二：广度优先搜索 (BFS - Breadth First Search)

**思想来源**：往湖面扔一颗石子，荡起的层层涟漪。
或者想象传染病的爆发模式：
1. 第 0 天：1 号病人感染（起始点）。
2. 第 1 天：传染给他**所有**直接接触的 1 级朋友。
3. 第 2 天：这群 1 级朋友再传染给他们各自的所有朋友（2 级朋友）。

BFS 坚决不“一条路走到黑”，而是**稳扎稳打，先扫荡完身边所有触手可及的邻居，再去扫荡邻居的邻居。**

在代码实现上，由于我们要严格遵守“先传染的人，他的朋友也优先被传染”的顺序，这就必须用到“先来后到”的数据结构——**队列（Queue）**！

---

## 核心实战：DFS 与 BFS 的代码模板

请准备好下面这张图（邻接表），我们将用代码来分别走一遍 DFS 和 BFS。

```text
 我们的迷宫图（无向图）：
      0
    /   \
   1     2
   |     | \
   3 --- 4  5
```
如果你从顶点 `0` 出发：
- **DFS 可能的路线**：0 -> 1 -> 3 -> 4 -> 2 -> 5 （一条路走到死，再退回找5）
- **BFS 的绝对路线**：0 (第一层) -> 1, 2 (第二层) -> 3, 4, 5 (第三层，扩散)

下面是各大语言中，用来闯荡图迷宫的最经典、最通用的**满分模板代码**（强烈建议收藏并理解）：

{% tabs %}
{% tab title="Python" %}
```python
from collections import deque

# 我们的图 (邻接表字典实现)
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0, 4, 5],
    3: [1, 4],
    4: [2, 3],
    5: [2]
}

# --- 1. 深度优先 (DFS) 递归版本 ---
def dfs(graph, node, visited):
    if node in visited: # 如果画过叉了，立马回头
        return

    print(node, end=" ") # 走到新路口，一探究竟！
    visited.add(node)    # 掏出粉笔，在本子上记下名字

    # 顺着这个路口的所有岔路往下钻 (不撞南墙不回头)
    for neighbor in graph[node]:
        dfs(graph, neighbor, visited)

# --- 2. 广度优先 (BFS) 队列版本 ---
def bfs(graph, start_node):
    visited = set() # BFS 也有自己的记事本
    queue = deque([start_node]) # 初始病人进入队列排队
    visited.add(start_node) # 在进队时就立马标记，防止同一天被两次拉进队

    while queue: # 只要还有人在排队
        node = queue.popleft() # 叫出队头的病人
        print(node, end=" ")

        # 传染给他所有还健康的朋友
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor) # 标记感染
                queue.append(neighbor) # 强行拉进队伍末尾

print("DFS 遍历结果: ", end="")
dfs(graph, 0, set()) # 找个新本子 set() 给 DFS 带着

print("\nBFS 遍历结果: ", end="")
bfs(graph, 0)
```
{% endtab %}
{% tab title="Java" %}
```java
import java.util.*;

public class GraphTraversal {

    // --- 1. 深度优先 (DFS) 递归版本 ---
    public static void dfs(Map<Integer, List<Integer>> graph, int node, Set<Integer> visited) {
        if (visited.contains(node)) return; // 画过叉了，回头

        System.out.print(node + " "); // 探索新路口
        visited.add(node);            // 掏出粉笔做记号

        // 死磕每一条能走的岔路
        for (int neighbor : graph.get(node)) {
            dfs(graph, neighbor, visited);
        }
    }

    // --- 2. 广度优先 (BFS) 队列版本 ---
    public static void bfs(Map<Integer, List<Integer>> graph, int startNode) {
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();

        queue.offer(startNode);
        visited.add(startNode); // 进队立刻做记号

        while (!queue.isEmpty()) {
            int node = queue.poll(); // 出队
            System.out.print(node + " ");

            // 感染所有接触的朋友
            for (int neighbor : graph.get(node)) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor); // 加入长龙
                }
            }
        }
    }

    public static void main(String[] args) {
        // 构建那张图
        Map<Integer, List<Integer>> graph = new HashMap<>();
        graph.put(0, Arrays.asList(1, 2));
        graph.put(1, Arrays.asList(0, 3));
        graph.put(2, Arrays.asList(0, 4, 5));
        graph.put(3, Arrays.asList(1, 4));
        graph.put(4, Arrays.asList(2, 3));
        graph.put(5, Collections.singletonList(2));

        System.out.print("DFS 遍历结果: ");
        dfs(graph, 0, new HashSet<>());

        System.out.print("\nBFS 遍历结果: ");
        bfs(graph, 0);
    }
}
```
{% endtab %}
{% tab title="C++" %}
```cpp
#include <iostream>
#include <vector>
#include <queue>

// 对于顶点编号是 0~N 的数组图，可以用 vector<bool> 做护身符，速度极快！

// --- 1. 深度优先 (DFS) ---
void dfs(const std::vector<std::vector<int>>& graph, int node, std::vector<bool>& visited) {
    if (visited[node]) return; // 走过了，防死循环

    std::cout << node << " ";
    visited[node] = true; // 做记号

    for (int neighbor : graph[node]) {
        dfs(graph, neighbor, visited);
    }
}

// --- 2. 广度优先 (BFS) ---
void bfs(const std::vector<std::vector<int>>& graph, int startNode) {
    std::vector<bool> visited(graph.size(), false);
    std::queue<int> q;

    q.push(startNode);
    visited[startNode] = true;

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        std::cout << node << " ";

        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}

int main() {
    // 6 个顶点的图
    std::vector<std::vector<int>> graph(6);
    graph[0] = {1, 2};
    graph[1] = {0, 3};
    graph[2] = {0, 4, 5};
    graph[3] = {1, 4};
    graph[4] = {2, 3};
    graph[5] = {2};

    std::cout << "DFS 遍历结果: ";
    std::vector<bool> visited(6, false);
    dfs(graph, 0, visited);

    std::cout << "\nBFS 遍历结果: ";
    bfs(graph, 0);

    return 0;
}
```
{% endtab %}
{% tab title="C#" %}
```csharp
using System;
using System.Collections.Generic;

class Program {

    // --- 1. 深度优先 (DFS) ---
    static void Dfs(Dictionary<int, List<int>> graph, int node, HashSet<int> visited) {
        if (visited.Contains(node)) return;

        Console.Write(node + " ");
        visited.Add(node);

        foreach (int neighbor in graph[node]) {
            Dfs(graph, neighbor, visited);
        }
    }

    // --- 2. 广度优先 (BFS) ---
    static void Bfs(Dictionary<int, List<int>> graph, int startNode) {
        HashSet<int> visited = new HashSet<int>();
        Queue<int> queue = new Queue<int>();

        queue.Enqueue(startNode);
        visited.Add(startNode);

        while (queue.Count > 0) {
            int node = queue.Dequeue();
            Console.Write(node + " ");

            foreach (int neighbor in graph[node]) {
                if (!visited.Contains(neighbor)) {
                    visited.Add(neighbor);
                    queue.Enqueue(neighbor);
                }
            }
        }
    }

    static void Main() {
        var graph = new Dictionary<int, List<int>> {
            { 0, new List<int> { 1, 2 } },
            { 1, new List<int> { 0, 3 } },
            { 2, new List<int> { 0, 4, 5 } },
            { 3, new List<int> { 1, 4 } },
            { 4, new List<int> { 2, 3 } },
            { 5, new List<int> { 2 } }
        };

        Console.Write("DFS 遍历结果: ");
        Dfs(graph, 0, new HashSet<int>());

        Console.Write("\nBFS 遍历结果: ");
        Bfs(graph, 0);
    }
}
```
{% endtab %}
{% endtabs %}

## 实战绝招：DFS 与 BFS 分别用来干什么？

会写代码只是第一步。在真实的互联网开发和笔试题中，你应该在什么时候掏出 DFS，什么时候掏出 BFS 呢？

### 选择 DFS 的黄金场景：
1. **走迷宫找出口**：只要问你“能不能走到目的地”，用 DFS 递归去撞墙最快。
2. **寻找“连通块”**：比如给你一张巨大的网，问你上面有几个互不干涉的“帮派（连通区域）”，DFS 一钻到底能瞬间把同一个帮派的人全部抓出来标黑。
3. **查出图里有没有“环（死循环）”**：如果是树，怎么走都不会绕回来；如果在 DFS 途中，发现前面有个邻居明明之前已经在口袋的 `Visited` 里记过了（并且它不是你刚走过来的来路节点），那说明这个世界有“环”！

### 选择 BFS 的黄金场景：
1. **寻找“最短路径”**：这是 BFS 的神仙应用！只要迷宫的每一段路长短是一样的（无权图），你要找从 A 到 B 最短的走法，**绝对不能用 DFS**，必须用水波纹一圈圈扩散的 BFS。因为 BFS 第一圈碰到的目的地，绝对是最短、最少步数的到达路径！
2. **六度人脉推荐**：在社交网络上，要找出“你朋友的朋友”（2度人脉），或者“通过三个人认识世界首富”（3度人脉），只有 BFS 层层递进的网状扫描才能办到。

## 本章小结

- **最大的敌人是环**：图因为有“环”存在，导致遍历随时可能陷入死循环。
- **唯一护身符 Visited**：在踏入每一个顶点之前，一定要先查验/打上已访问的标记。
- **DFS（不撞南墙不回头）**：底层依赖**递归函数（或栈 Stack）**实现。找连通图、探寻死路的绝对主力。
- **BFS（水波纹层层扩散）**：底层依赖**先进先出的队列（Queue）**实现。找**无权图最短路径**的唯一王者。

祝贺你！到此为止，你已经不仅掌握了天下第一快的哈希表，还成功学会了走遍世界最复杂“图网络”迷宫的两大绝学！整个**【第三部分：散列与图】**的基础知识已经被你全部吃透。

前三个部分，我们都在研究“数据该怎么存放（数据结构）”。从下一场战役开始，我们将进入纯粹的逻辑艺术：面对一堆乱七八糟的数据，如何用最优雅的手段将它们变得井井有条？
我们将踏入全文档的最后一个部分：**【第四部分：排序与查找】**，领略一下经典排序算法（冒泡、插入、快速、归并）的暴力与美学！