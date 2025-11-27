# 线性数据结构

## 1. 数组（Array）

### 1.1 基本概念
数组是一种线性数据结构，它用一组连续的内存空间来存储一组具有相同类型的数据。

### 1.2 特点
- 随机访问：通过索引可以直接访问任意位置的元素，时间复杂度为O(1)
- 连续存储：所有元素在内存中连续存放
- 固定大小：创建时需要指定大小，且大小固定

### 1.3 基本操作
- 访问元素：O(1)
- 插入元素：O(n)
- 删除元素：O(n)
- 查找元素：O(n)

### 1.4 实现示例
```python
class Array:
    def __init__(self, capacity):
        self.capacity = capacity
        self.data = [None] * capacity
        self.size = 0
    
    def get(self, index):
        if index < 0 or index >= self.size:
            raise IndexError("Index out of range")
        return self.data[index]
    
    def insert(self, index, element):
        if self.size >= self.capacity:
            raise Exception("Array is full")
        if index < 0 or index > self.size:
            raise IndexError("Index out of range")
        
        for i in range(self.size - 1, index - 1, -1):
            self.data[i + 1] = self.data[i]
        self.data[index] = element
        self.size += 1
    
    def delete(self, index):
        if index < 0 or index >= self.size:
            raise IndexError("Index out of range")
        
        for i in range(index, self.size - 1):
            self.data[i] = self.data[i + 1]
        self.size -= 1
```

## 2. 链表（Linked List）

### 2.1 基本概念
链表是一种线性数据结构，它通过指针将一组零散的内存块串联起来使用。

### 2.2 特点
- 动态扩容：不需要连续的内存空间
- 插入删除：不需要移动其他元素
- 随机访问：需要遍历，时间复杂度为O(n)

### 2.3 基本操作
- 访问元素：O(n)
- 插入元素：O(1)
- 删除元素：O(1)
- 查找元素：O(n)

### 2.4 实现示例
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class LinkedList:
    def __init__(self):
        self.head = None
        self.size = 0
    
    def get(self, index):
        if index < 0 or index >= self.size:
            raise IndexError("Index out of range")
        
        current = self.head
        for _ in range(index):
            current = current.next
        return current.val
    
    def add_at_head(self, val):
        self.add_at_index(0, val)
    
    def add_at_tail(self, val):
        self.add_at_index(self.size, val)
    
    def add_at_index(self, index, val):
        if index < 0 or index > self.size:
            raise IndexError("Index out of range")
        
        if index == 0:
            self.head = ListNode(val, self.head)
        else:
            current = self.head
            for _ in range(index - 1):
                current = current.next
            current.next = ListNode(val, current.next)
        self.size += 1
    
    def delete_at_index(self, index):
        if index < 0 or index >= self.size:
            raise IndexError("Index out of range")
        
        if index == 0:
            self.head = self.head.next
        else:
            current = self.head
            for _ in range(index - 1):
                current = current.next
            current.next = current.next.next
        self.size -= 1
```

## 3. 栈（Stack）

### 3.1 基本概念
栈是一种特殊的线性表，它只允许在表的一端进行插入和删除操作。这一端被称为栈顶，另一端被称为栈底。

### 3.2 特点
- 后进先出（LIFO）：最后入栈的元素最先出栈
- 只能在一端操作：只能在栈顶进行插入和删除操作

### 3.3 基本操作
- 入栈（push）：O(1)
- 出栈（pop）：O(1)
- 查看栈顶元素（peek）：O(1)
- 判断栈是否为空：O(1)

### 3.4 实现示例
```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        raise Exception("Stack is empty")
    
    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        raise Exception("Stack is empty")
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

## 4. 队列（Queue）

### 4.1 基本概念
队列是一种特殊的线性表，它只允许在表的一端进行插入操作，在另一端进行删除操作。插入端称为队尾，删除端称为队头。

### 4.2 特点
- 先进先出（FIFO）：最先入队的元素最先出队
- 两端操作：在队尾插入，在队头删除

### 4.3 基本操作
- 入队（enqueue）：O(1)
- 出队（dequeue）：O(1)
- 查看队头元素（peek）：O(1)
- 判断队列是否为空：O(1)

### 4.4 实现示例
```python
class Queue:
    def __init__(self):
        self.items = []
    
    def enqueue(self, item):
        self.items.append(item)
    
    def dequeue(self):
        if not self.is_empty():
            return self.items.pop(0)
        raise Exception("Queue is empty")
    
    def peek(self):
        if not self.is_empty():
            return self.items[0]
        raise Exception("Queue is empty")
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

## 5. 应用场景

### 5.1 数组的应用
- 存储固定大小的数据集合
- 实现其他数据结构（如栈、队列）
- 矩阵运算
- 图像处理

### 5.2 链表的应用
- 实现其他数据结构（如栈、队列）
- 内存管理
- 文件系统
- 浏览器历史记录

### 5.3 栈的应用
- 函数调用栈
- 表达式求值
- 括号匹配
- 深度优先搜索

### 5.4 队列的应用
- 任务调度
- 消息队列
- 广度优先搜索
- 打印机任务队列 