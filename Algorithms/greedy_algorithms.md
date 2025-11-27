# 贪心算法

## 1. 基本概念

### 1.1 什么是贪心算法
- 贪心算法是一种在每一步选择中都采取当前状态下最好或最优的选择，从而希望导致结果是最好或最优的算法
- 贪心算法不是对所有问题都能得到整体最优解，但对许多问题它能产生整体最优解
- 贪心算法是动态规划的一种特殊情况

### 1.2 贪心算法的特点
- **局部最优**：在每一步都做出局部最优的选择
- **无后效性**：当前的选择不会影响后续的选择
- **子问题最优**：每个子问题的最优解能导致原问题的最优解

### 1.3 适用条件
- 问题具有最优子结构
- 问题具有贪心选择性质
- 问题可以分解为子问题

## 2. 经典问题

### 2.1 活动选择问题
- **问题描述**：选择最多的互不重叠的活动
- **贪心策略**：选择结束时间最早的活动
- **时间复杂度**：O(n log n)
- **空间复杂度**：O(1)

### 2.2 霍夫曼编码
- **问题描述**：构建最优前缀编码
- **贪心策略**：选择频率最小的两个节点合并
- **时间复杂度**：O(n log n)
- **空间复杂度**：O(n)

### 2.3 最小生成树
- **问题描述**：找到连接所有顶点的最小权重的树
- **贪心策略**：
  - Kruskal算法：选择权重最小的边
  - Prim算法：选择与当前树相连的最小权重边
- **时间复杂度**：O(E log E)
- **空间复杂度**：O(V)

### 2.4 最短路径
- **问题描述**：找到从源点到其他所有点的最短路径
- **贪心策略**：Dijkstra算法
- **时间复杂度**：O(V²)
- **空间复杂度**：O(V)

## 3. 实现方法

### 3.1 基本步骤
1. 确定问题的最优子结构
2. 设计贪心策略
3. 证明贪心策略的正确性
4. 实现算法
5. 分析算法复杂度

### 3.2 证明方法
- 交换论证
- 归纳法
- 反证法
- 数学归纳法

## 4. 优化技巧

### 4.1 数据结构优化
- 使用优先队列
- 使用并查集
- 使用哈希表
- 使用树结构

### 4.2 算法优化
- 预处理
- 剪枝
- 记忆化
- 并行处理

## 5. 实际应用

### 5.1 调度问题
- 任务调度
- 资源分配
- 进程调度
- 作业调度

### 5.2 网络优化
- 路由选择
- 带宽分配
- 负载均衡
- 流量控制

### 5.3 压缩算法
- 数据压缩
- 图像压缩
- 视频压缩
- 音频压缩

## 6. 注意事项

### 6.1 常见错误
- 贪心策略不正确
- 忽略边界条件
- 没有证明正确性
- 实现细节错误

### 6.2 调试技巧
- 使用小规模测试用例
- 验证贪心策略
- 检查边界条件
- 分析反例

### 6.3 性能考虑
- 时间复杂度优化
- 空间复杂度优化
- 内存使用优化
- 代码可维护性

## 7. 代码实现示例

### 7.1 活动选择问题
```python
def activity_selection(activities):
    # 按结束时间排序
    activities.sort(key=lambda x: x[1])
    
    result = [activities[0]]
    last_end = activities[0][1]
    
    for activity in activities[1:]:
        if activity[0] >= last_end:
            result.append(activity)
            last_end = activity[1]
    
    return result
```

### 7.2 霍夫曼编码
```python
import heapq

def huffman_encoding(freq):
    # 创建优先队列
    heap = [[weight, [char, ""]] for char, weight in freq.items()]
    heapq.heapify(heap)
    
    # 构建霍夫曼树
    while len(heap) > 1:
        lo = heapq.heappop(heap)
        hi = heapq.heappop(heap)
        
        for pair in lo[1:]:
            pair[1] = '0' + pair[1]
        for pair in hi[1:]:
            pair[1] = '1' + pair[1]
        
        heapq.heappush(heap, [lo[0] + hi[0]] + lo[1:] + hi[1:])
    
    return sorted(heapq.heappop(heap)[1:], key=lambda p: (len(p[-1]), p))
```

### 7.3 最小生成树（Kruskal算法）
```python
def kruskal(graph):
    def find(parent, i):
        if parent[i] != i:
            parent[i] = find(parent, parent[i])
        return parent[i]
    
    def union(parent, rank, x, y):
        xroot = find(parent, x)
        yroot = find(parent, y)
        
        if rank[xroot] < rank[yroot]:
            parent[xroot] = yroot
        elif rank[xroot] > rank[yroot]:
            parent[yroot] = xroot
        else:
            parent[yroot] = xroot
            rank[xroot] += 1
    
    result = []
    i = 0
    e = 0
    
    # 按权重排序边
    graph = sorted(graph, key=lambda item: item[2])
    
    parent = []
    rank = []
    
    # 创建V个单元素集合
    for node in range(V):
        parent.append(node)
        rank.append(0)
    
    # 选择V-1条边
    while e < V - 1:
        u, v, w = graph[i]
        i += 1
        x = find(parent, u)
        y = find(parent, v)
        
        if x != y:
            e += 1
            result.append([u, v, w])
            union(parent, rank, x, y)
    
    return result
``` 