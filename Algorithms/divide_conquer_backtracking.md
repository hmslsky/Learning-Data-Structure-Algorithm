# 分治与回溯算法

## 1. 分治算法

### 1.1 基本概念
- 分治算法是一种将问题分解为更小的子问题，递归求解，然后合并结果的算法设计方法
- 分治算法的三个步骤：
  1. 分解（Divide）：将原问题分解为若干个子问题
  2. 解决（Conquer）：递归地解决各个子问题
  3. 合并（Combine）：将子问题的解合并为原问题的解

### 1.2 适用条件
- 问题可以分解为规模较小的相同问题
- 子问题的解可以合并为原问题的解
- 子问题之间相互独立

### 1.3 经典问题

#### 1.3.1 归并排序
- **基本思想**：将数组分成两半，递归排序，然后合并
- **时间复杂度**：O(n log n)
- **空间复杂度**：O(n)
- **稳定性**：稳定

#### 1.3.2 快速排序
- **基本思想**：选择一个基准元素，将数组分为两部分，递归排序
- **时间复杂度**：平均O(n log n)，最坏O(n²)
- **空间复杂度**：O(log n)
- **稳定性**：不稳定

#### 1.3.3 二分查找
- **基本思想**：在有序数组中，通过比较中间元素来缩小搜索范围
- **时间复杂度**：O(log n)
- **空间复杂度**：O(1)
- **适用场景**：有序数组

### 1.4 优化技巧
- 使用尾递归优化
- 避免重复计算
- 使用迭代代替递归
- 优化合并过程

## 2. 回溯算法

### 2.1 基本概念
- 回溯算法是一种通过穷举来解决问题的方法
- 核心思想：从初始状态出发，暴力搜索所有可能的解决方案
- 当遇到正确的解则记录，直到找到解或尝试所有可能的选择

### 2.2 算法框架
1. 选择：在当前状态下，选择一个可能的选项
2. 探索：递归地探索这个选项
3. 回溯：如果当前选项不满足条件，回退到上一步
4. 剪枝：通过约束条件减少搜索空间

### 2.3 经典问题

#### 2.3.1 N皇后问题
- **问题描述**：在N×N的棋盘上放置N个皇后，使它们互不攻击
- **解决思路**：
  - 逐行放置皇后
  - 检查当前位置是否安全
  - 回溯尝试其他位置
- **时间复杂度**：O(N!)
- **空间复杂度**：O(N)

#### 2.3.2 全排列问题
- **问题描述**：生成一个数组的所有可能排列
- **解决思路**：
  - 固定一个位置
  - 递归生成剩余位置的排列
  - 回溯尝试其他选择
- **时间复杂度**：O(n!)
- **空间复杂度**：O(n)

#### 2.3.3 子集问题
- **问题描述**：生成一个数组的所有可能子集
- **解决思路**：
  - 对每个元素，选择放入或不放入
  - 递归生成剩余元素的子集
  - 回溯尝试其他选择
- **时间复杂度**：O(2^n)
- **空间复杂度**：O(n)

### 2.4 优化技巧
- 使用位运算优化
- 实现剪枝策略
- 使用记忆化搜索
- 优化状态表示

## 3. 实际应用

### 3.1 分治应用
- 大规模数据处理
- 并行计算
- 图像处理
- 信号处理

### 3.2 回溯应用
- 游戏求解
- 组合优化
- 约束满足问题
- 路径规划

## 4. 注意事项

### 4.1 分治注意事项
- 子问题规模要适中
- 避免过度分解
- 注意合并开销
- 考虑并行性

### 4.2 回溯注意事项
- 合理设计状态表示
- 实现有效的剪枝
- 避免重复计算
- 注意内存使用

## 5. 代码实现示例

### 5.1 分治示例
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

### 5.2 回溯示例
```python
def solve_n_queens(n):
    def is_safe(board, row, col):
        # 检查列
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        
        # 检查左上对角线
        for i, j in zip(range(row-1, -1, -1), range(col-1, -1, -1)):
            if board[i][j] == 'Q':
                return False
        
        # 检查右上对角线
        for i, j in zip(range(row-1, -1, -1), range(col+1, n)):
            if board[i][j] == 'Q':
                return False
        
        return True
    
    def backtrack(board, row):
        if row == n:
            result.append([''.join(row) for row in board])
            return
        
        for col in range(n):
            if is_safe(board, row, col):
                board[row][col] = 'Q'
                backtrack(board, row + 1)
                board[row][col] = '.'
    
    result = []
    board = [['.' for _ in range(n)] for _ in range(n)]
    backtrack(board, 0)
    return result
``` 