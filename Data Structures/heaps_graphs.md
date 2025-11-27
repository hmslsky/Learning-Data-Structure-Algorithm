# 堆与图（Heaps and Graphs）

## 1. 堆（Heap）

### 1.1 基本概念
堆是一种特殊的树形数据结构，它满足以下性质：
- 堆是一个完全二叉树
- 堆中每个节点的值都大于等于（或小于等于）其子节点的值

### 1.2 堆的类型
- 最大堆：每个节点的值都大于等于其子节点的值
- 最小堆：每个节点的值都小于等于其子节点的值

### 1.3 堆的实现
```python
class Heap:
    def __init__(self, heap_type='min'):
        self.heap = []
        self.heap_type = heap_type
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
    def heapify_up(self, i):
        parent = self.parent(i)
        if i > 0 and (
            (self.heap_type == 'min' and self.heap[i] < self.heap[parent]) or
            (self.heap_type == 'max' and self.heap[i] > self.heap[parent])
        ):
            self.swap(i, parent)
            self.heapify_up(parent)
    
    def heapify_down(self, i):
        min_index = i
        left = self.left_child(i)
        right = self.right_child(i)
        
        if left < len(self.heap) and (
            (self.heap_type == 'min' and self.heap[left] < self.heap[min_index]) or
            (self.heap_type == 'max' and self.heap[left] > self.heap[min_index])
        ):
            min_index = left
        
        if right < len(self.heap) and (
            (self.heap_type == 'min' and self.heap[right] < self.heap[min_index]) or
            (self.heap_type == 'max' and self.heap[right] > self.heap[min_index])
        ):
            min_index = right
        
        if i != min_index:
            self.swap(i, min_index)
            self.heapify_down(min_index)
    
    def insert(self, key):
        self.heap.append(key)
        self.heapify_up(len(self.heap) - 1)
    
    def extract(self):
        if len(self.heap) == 0:
            return None
        
        result = self.heap[0]
        self.heap[0] = self.heap[-1]
        self.heap.pop()
        
        if len(self.heap) > 0:
            self.heapify_down(0)
        
        return result
```

### 1.4 堆的应用
- 优先队列
- 堆排序
- 图算法（如Dijkstra最短路径算法）
- 事件驱动模拟

## 2. 图（Graph）

### 2.1 基本概念
图是由一组顶点和一组边组成的非线性数据结构。图可以是有向的或无向的，可以有权重或无权重。

### 2.2 图的表示方法

#### 2.2.1 邻接矩阵
```python
class GraphMatrix:
    def __init__(self, num_vertices):
        self.num_vertices = num_vertices
        self.matrix = [[0] * num_vertices for _ in range(num_vertices)]
    
    def add_edge(self, v1, v2, weight=1):
        self.matrix[v1][v2] = weight
        self.matrix[v2][v1] = weight  # 无向图
    
    def remove_edge(self, v1, v2):
        self.matrix[v1][v2] = 0
        self.matrix[v2][v1] = 0  # 无向图
```

#### 2.2.2 邻接表
```python
class GraphList:
    def __init__(self, num_vertices):
        self.num_vertices = num_vertices
        self.adj_list = [[] for _ in range(num_vertices)]
    
    def add_edge(self, v1, v2, weight=1):
        self.adj_list[v1].append((v2, weight))
        self.adj_list[v2].append((v1, weight))  # 无向图
    
    def remove_edge(self, v1, v2):
        self.adj_list[v1] = [(v, w) for v, w in self.adj_list[v1] if v != v2]
        self.adj_list[v2] = [(v, w) for v, w in self.adj_list[v2] if v != v1]  # 无向图
```

### 2.3 图的遍历

#### 2.3.1 深度优先搜索（DFS）
```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    print(start, end=' ')
    
    for neighbor in graph.adj_list[start]:
        if neighbor[0] not in visited:
            dfs(graph, neighbor[0], visited)
```

#### 2.3.2 广度优先搜索（BFS）
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        vertex = queue.popleft()
        print(vertex, end=' ')
        
        for neighbor in graph.adj_list[vertex]:
            if neighbor[0] not in visited:
                visited.add(neighbor[0])
                queue.append(neighbor[0])
```

### 2.4 图算法

#### 2.4.1 最短路径算法
```python
def dijkstra(graph, start):
    distances = {vertex: float('infinity') for vertex in range(graph.num_vertices)}
    distances[start] = 0
    pq = [(0, start)]
    
    while pq:
        current_distance, current_vertex = heapq.heappop(pq)
        
        if current_distance > distances[current_vertex]:
            continue
        
        for neighbor, weight in graph.adj_list[current_vertex]:
            distance = current_distance + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances
```

#### 2.4.2 最小生成树算法
```python
def prim(graph, start):
    mst = set()
    visited = {start}
    edges = [(weight, start, to) for to, weight in graph.adj_list[start]]
    heapq.heapify(edges)
    
    while edges:
        weight, frm, to = heapq.heappop(edges)
        if to not in visited:
            visited.add(to)
            mst.add((frm, to, weight))
            
            for next_vertex, next_weight in graph.adj_list[to]:
                if next_vertex not in visited:
                    heapq.heappush(edges, (next_weight, to, next_vertex))
    
    return mst
```

### 2.5 图的应用
- 社交网络
- 地图导航
- 网络拓扑
- 任务调度
- 依赖关系分析

## 3. 堆与图的结合应用

### 3.1 优先队列实现
```python
class PriorityQueue:
    def __init__(self):
        self.heap = []
    
    def push(self, item, priority):
        heapq.heappush(self.heap, (priority, item))
    
    def pop(self):
        return heapq.heappop(self.heap)[1]
    
    def is_empty(self):
        return len(self.heap) == 0
```

### 3.2 实际应用案例
- 网络路由算法
- 任务调度系统
- 游戏AI寻路
- 社交网络推荐系统
- 物流配送优化 