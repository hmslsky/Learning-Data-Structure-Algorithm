# 哈希表（Hash Table）

## 1. 基本概念

哈希表是一种使用哈希函数组织数据，以支持快速插入和搜索的数据结构。它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。

## 2. 核心组件

### 2.1 哈希函数
哈希函数将任意大小的数据映射到固定大小的数据。一个好的哈希函数应该：
- 计算速度快
- 分布均匀
- 冲突少

常见的哈希函数：
```python
def hash_function(key, size):
    # 简单取模法
    return key % size

def hash_function_string(key, size):
    # 字符串哈希
    hash_value = 0
    for char in key:
        hash_value = (hash_value * 31 + ord(char)) % size
    return hash_value
```

### 2.2 冲突处理
当两个不同的键映射到同一个位置时，就会发生冲突。主要的冲突处理方法有：

#### 2.2.1 开放寻址法
- 线性探测
- 二次探测
- 双重哈希

#### 2.2.2 链地址法
- 使用链表存储冲突的元素

## 3. 实现示例

### 3.1 使用链地址法的哈希表实现
```python
class ListNode:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.next = None

class HashTable:
    def __init__(self, size=1000):
        self.size = size
        self.table = [None] * size
    
    def _hash(self, key):
        if isinstance(key, int):
            return key % self.size
        return hash_function_string(str(key), self.size)
    
    def put(self, key, value):
        index = self._hash(key)
        if self.table[index] is None:
            self.table[index] = ListNode(key, value)
        else:
            current = self.table[index]
            while current:
                if current.key == key:
                    current.val = value
                    return
                if current.next is None:
                    break
                current = current.next
            current.next = ListNode(key, value)
    
    def get(self, key):
        index = self._hash(key)
        current = self.table[index]
        while current:
            if current.key == key:
                return current.val
            current = current.next
        return None
    
    def remove(self, key):
        index = self._hash(key)
        if self.table[index] is None:
            return
        
        if self.table[index].key == key:
            self.table[index] = self.table[index].next
            return
        
        current = self.table[index]
        while current.next:
            if current.next.key == key:
                current.next = current.next.next
                return
            current = current.next
```

### 3.2 使用开放寻址法的哈希表实现
```python
class HashTableOpenAddressing:
    def __init__(self, size=1000):
        self.size = size
        self.table = [None] * size
        self.deleted = object()  # 标记已删除的位置
    
    def _hash(self, key):
        if isinstance(key, int):
            return key % self.size
        return hash_function_string(str(key), self.size)
    
    def _probe(self, index, i):
        # 线性探测
        return (index + i) % self.size
    
    def put(self, key, value):
        index = self._hash(key)
        i = 0
        while i < self.size:
            pos = self._probe(index, i)
            if self.table[pos] is None or self.table[pos] is self.deleted:
                self.table[pos] = (key, value)
                return
            if self.table[pos][0] == key:
                self.table[pos] = (key, value)
                return
            i += 1
        raise Exception("Hash table is full")
    
    def get(self, key):
        index = self._hash(key)
        i = 0
        while i < self.size:
            pos = self._probe(index, i)
            if self.table[pos] is None:
                return None
            if self.table[pos] is not self.deleted and self.table[pos][0] == key:
                return self.table[pos][1]
            i += 1
        return None
    
    def remove(self, key):
        index = self._hash(key)
        i = 0
        while i < self.size:
            pos = self._probe(index, i)
            if self.table[pos] is None:
                return
            if self.table[pos] is not self.deleted and self.table[pos][0] == key:
                self.table[pos] = self.deleted
                return
            i += 1
```

## 4. 性能分析

### 4.1 时间复杂度
- 插入：平均 O(1)，最坏 O(n)
- 查找：平均 O(1)，最坏 O(n)
- 删除：平均 O(1)，最坏 O(n)

### 4.2 空间复杂度
- O(n)，其中 n 是存储的元素数量

## 5. 应用场景

### 5.1 常见应用
- 字典/映射实现
- 缓存系统
- 数据库索引
- 文件系统
- 密码存储

### 5.2 实际案例
- 编程语言中的字典/映射类型
- 数据库中的哈希索引
- 缓存系统（如Redis）
- 文件系统的inode表
- 密码存储中的盐值哈希 