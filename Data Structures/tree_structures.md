# 树结构（Tree Structures）

## 1. 基本概念

树是一种非线性的数据结构，它是由n（n>=0）个有限节点组成一个具有层次关系的集合。树具有以下特点：
- 每个节点有零个或多个子节点
- 没有父节点的节点称为根节点
- 每一个非根节点有且只有一个父节点
- 除了根节点外，每个子节点可以分为多个不相交的子树

## 2. 树的表示

### 2.1 基本节点定义
```python
from typing import List, Generic, TypeVar

T = TypeVar('T')

class TreeNode(Generic[T]):
    def __init__(self, id: str, name: str, parent_id=None):
        assert id, '[id] must has value'
        assert name, '[name] must has value'
        self.id = id                                     # ID
        self.name = name                                 # 名称
        self.attr: T = None                              # 属性
        self.parent_id = parent_id                       # 父级节点
        self.children: List[TreeNode] = []               # 下级节点

    def add_child(self, child):
        """增加子节点"""
        if child not in self.children:
            self.children.append(child)

    def delete_child(self, child):
        """删除子节点"""
        assert child in self.children, f'{child} not in children'
        self.children.remove(child)
```

## 3. 树的遍历

### 3.1 广度优先搜索（BFS）
广度优先搜索按照从根节点到叶节点的距离，从近到远依次访问所有节点。

**优点：**
- 容易实现
- 可以找到图或树中的最短路径

**缺点：**
- 空间复杂度较高，需要存储所有待访问的节点
- 可能无法找到图或树中的所有环

```python
from collections import deque

def bfs(root: TreeNode, func):
    """
    广度遍历，从根节点由远及近访问所有节点
    
    Args:
        func: 节点处置方法，该方法返回True代表继续遍历，如果返回False代表停止遍历
        root：待遍历的根节点
    Returns:
        None
    """
    queue = deque()
    queue.append(root)
    while queue:
        node = queue.popleft()
        # 执行func函数，并判断是否终止遍历
        res = func(node)
        if not res:
            break
        if node.children:
            [queue.append(child) for child in node.children]
```

### 3.2 深度优先搜索（DFS）
深度优先搜索沿着一条路径一直向下搜索，直到遇到不可达的节点，然后回退到上一个节点，再沿着另一条路径继续搜索。

**优点：**
- 空间复杂度较低，只需要存储当前访问路径上的节点
- 可以找到图或树中的所有环

**缺点：**
- 难以实现
- 可能无法找到图或树中的最短路径

```python
def dfs(root: TreeNode, func):
    """
    深度优先遍历，沿着一条路径一直向下搜索，直到遇到不可达的节点，再沿着另一条路径继续搜索。
    
    Args:
        func: 节点处置方法，该方法返回True代表继续遍历，如果返回False代表停止遍历
        root：待遍历的根节点
    Returns:
        None
    """
    stack = [root]
    visit_nodes = {}
    while stack:
        node = stack.pop()
        if node.children and list(filter(lambda x: x not in visit_nodes, node.children)):
            stack.append(node)
            [stack.append(child) for child in node.children]
        else:
            # 执行func函数，并判断是否终止遍历
            res = func(node)
            if not res:
                break
```

### 3.3 遍历方式

#### 3.3.1 前序遍历
先访问根节点，然后递归地访问左子树和右子树。

```python
def preorder_recursive(root: TreeNode) -> List[TreeNode]:
    """前序遍历（递归方式）"""
    result = []

    def dfs(node: TreeNode):
        if not node:
            return
        result.append(node)
        for child in node.children:
            dfs(child)
    dfs(root)
    return result

def preorder_iterative(root: TreeNode) -> List[TreeNode]:
    """前序遍历（迭代方式）"""
    result = []
    stack = [root]
    while stack:
        node = stack.pop()
        result.append(node)
        stack.extend(node.children)
    return result
```

#### 3.3.2 后序遍历
先递归地访问左子树，然后递归地访问右子树，最后访问根节点。

```python
def postorder_recursive(root: TreeNode) -> List[TreeNode]:
    """后序遍历（递归方式）"""
    result = []

    def dfs(node: TreeNode):
        if not node:
            return
        for child in node.children:
            dfs(child)
        result.append(node)
    dfs(root)
    return result

def postorder_iterative(root: TreeNode) -> List[TreeNode]:
    """后序遍历（迭代方式）"""
    result = []
    stack = [root]
    visit_nodes = {}
    while stack:
        node = stack.pop()
        if node.children and list(filter(lambda x: x not in visit_nodes, node.children)):
            stack.append(node)
            result.extend(node.children)
        else:
            result.append(node)
    return result
```

## 4. 特殊树结构

### 4.1 二叉树
每个节点最多有两个子节点的树结构。

### 4.2 二叉搜索树
一种特殊的二叉树，其中每个节点的值大于其左子树中所有节点的值，小于其右子树中所有节点的值。

### 4.3 平衡二叉树
一种特殊的二叉搜索树，其中任意节点的左右子树高度差不超过1。

### 4.4 B+树
B+树是一个n叉树，每个节点通常有多个孩子，一颗B+树包含根节点、内部节点和叶子节点。

#### 4.4.1 B+树特征
一个m阶的B+树具有以下特征：
1. 根节点至少有两个子女
2. 每个中间节点至少包含ceil(m/2)个孩子，最多有m个孩子
3. 每一个叶子节点都包含k-1个元素，其中m/2 <= k <= m
4. 所有的叶子节点都位于同一层
5. 每个节点中的元素从小到大排序，节点当中k-1个元素正好是k个孩子包含的元素的值域分划

#### 4.4.2 应用场景
- 数据库索引
- 文件系统
- 内存管理

## 5. 树的应用

### 5.1 常见应用
- 文件系统
- 数据库索引
- 表达式求值
- 决策树
- 语法分析

### 5.2 实际案例
- 文件系统的目录结构
- 数据库的B+树索引
- 编译器的语法树
- 机器学习中的决策树
- XML/HTML文档的DOM树 