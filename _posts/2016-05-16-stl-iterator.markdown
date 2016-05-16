---
layout: post
title:  "STL之迭代器失效"
date:   2016-05-16 00:00:00
categories: ["base"]
author: "Chyler"
---


STL所有container都是sequence或associative container的model

**三种序列sequence**


vector：快速随机访问 注：iterator易失效

deque：高效率两段安插移除元素 注：iterator易失效

list：双向链接链表 注：iterator不会失效

vector：实现方式是把元素安排在连续的存储块中，iterators可以是一般指针。vector会管理其内存，从内存开头处构造元素。vector大小等于其容量，超过容量，需要重新分配一块新的更大的内存。内存重新分配时，其iterator失效！在vector中间插入删除元素，会使指向该插入点或删除点之后的元素的所有iterators失效！

deque：与vector不同，提供常量时间内于序列开头新增或移除元素。insert（包括push_back, push_front）都会使所有iterators失效，对中段调用erase也会使所有iterators失效。(deque远比
vector复杂，迭代器累加动作至少多一次比较操作，所以比vector慢很多)

list：双向链表，有前驱和后继。重要性质：安插与结合（splicing）动作不会使指向list的iterator失效，删除只会使本身的iterator失效。


**四种关联容器associative container**

set元素一定会以递增顺序排序 注：iterator不会失效

map 注：iterator不会失效

multiset 注：iterator不会失效

multimap 注：iterator不会失效

sequence的插入和删除

insert S.insert(p, x)操作：在p之前插入元素, iterator的range是非对称的，可以在S.begin()之前做插入，也可以在S.end()尾端前做插入。

erase S.erase(p)移除并摧毁该元素。

vector, list, deque主要差异在于提供的iterator种类以及iterator的无效语义。

```
multiset < int, greater<int> > //默认从小到大，现在改成从大到小
```
