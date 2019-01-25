---
title: python数据结构-链表
date: 2018-03-18 16:14:46
tags: 
- python
- 数据结构
categories: [python]
---

python实现链表依赖于类生成的实例，每个节点都是一个对象，组合在一起形成一个完整链表

对于node类只需关注两点：value 和 next 
对于linked_list需要关心的：head（头结点，默认是node），length（列表的长度）

## 链表的数据结构

如下图所示：

![](/img/链表.png)
## 链表的实现
### 定义链表
定义**节点类Node**
```python
class Node:
     def __init__(self, value):
     '''
     value: 节点保存的数据，next: 保存下一个节点对象
     '''
         self.val = value
         self.next = None
     def __repr__(self):
        '''
        用来定义Node的字符输出，print为输出value
        '''
        return str(self.value)
```
### 操作链表
定义**链表类LinkedList**
链表要包括：
**属性：**
链表头：head
链表长度：length

**方法：**
- is_empty() 链表是否为空
- add(item) 链表头部添加元素
- append(item) 链表尾部添加元素
- insert(index, item) 指定位置添加元素
- delete(index) 删除指定位置节点
- remove(item) 删除某个数值节点
- search(item) 查找节点是否存在

#### 判断是否为空: is_Empty()
```python
def is_empty(self):
	return self.length == 0
```
#### 头部添加一个节点：add()
```python
def add(self, item):
　　node = Node(item)
　　# 将新节点的链接域next指向头节点，即head指向的位置
　　node.next = self.head
　　# 将链表的头head指向新节点
　　self.head = node
```
#### 尾部添加一个节点：append()
```python
def append(self, item):
    # 确保item是node对象
    item = Node(item)
    # 先判断链表是否为空，若是空链表，则将head指向新节点
　　if self.is_empty():
　　　　self.head = node
    # 若不为空，则找到尾部，将尾节点的next指向新节点
    else:
        node = self.head
        while node.next:
            node = node.next
        node.next = item
    self.length += 1
```
#### 指定位置添加节点：insert()
```python
def insert(self, index, item):　　
　　# 若指定位置pos为第一个元素之前，则执行头部插入
　　if index <= 0:
   　　self.add(item)
    
　　# 若指定位置超过链表尾部，则执行尾部插入
　　elif index > (self.length()-1):
   　　self.append(item)
    
　　# 找到指定位置
　　else:
　　　　node = Node(item)　　　　
　　　　# 从头节点开始移动到插入位置的前一个节点
　　　　pre = self.head
　　　　while index-1:　　　　　　
　　　　　　pre = pre.next
           index -= 1
　　　　# 先将新节点node的next指向插入位置的节点
　　　　node.next = pre.next
　　　　# 将插入位置的前一个节点的next指向新节点
　　　　pre.next = node
    self.length += 1
```
#### 删除数值是item的节点：remove(item)
```python
def remove(self, item):
    pre = None     # 前一个节点
    cur = self.head  # 当前节点
    while cur:
        # 找到了指定元素
        if cur.item == item:
            # 如果第一个就是删除的节点
            if not pre:
                # 将头指针指向头节点的后一个节点
                self.head = cur.next
            else:
                # 将删除位置前一个节点的next指向删除位置的后一个节点
                pre.next = cur.next
            self.length -= 1
            break
        else:
            # 继续按链表后移节点
            pre = cur
            cur = cur.next
```
#### 删除某个位置的节点：delete(index)
```python
def delete(self, index):
	if index > self.length:
     	# 索引值超出范围直接提示并且退出
     	print("Index  is out of range.")
        return
     else:
     	if index == 0:
        	self.head = self.head.next
        else:
            pre = self.head
            while index - 1:
                pre = pre.next
                index -= 1
            pre.next = pre.next.pnext            
         self.length -= 1
```
#### 查找节点是否存在：search()
```python
def search(self,item):
　　"""链表查找节点是否存在，并返回True或者False"""
　　cur = self.head
　　while cur != None:
　　　　if cur.item == item:
　　　　　　return True
　　　　cur = cur.next
　　return False
```