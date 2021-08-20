# Java Collection 教程


Collection 是用于表示和操作集合的统一体系结构。

<!--more-->

## 简介

所有 Collection 都包含以下内容：

- **接口（Interfaces）**：这些是表示集合的抽象数据类型。接口允许集合独立于其表示的细节进行操作。在面向对象语言中，接口通常形成一个层次结构。
- **实现（Implementations）**：这些是集合接口的具体实现。本质上，它们是可重用的数据结构。
- **算法（Algorithms）**：这些方法对实现集合接口的对象执行有用的计算，如搜索和排序。这些算法被认为是多态的：也就是说，相同的方法可以用于相应集合接口的许多不同实现。本质上，算法是可重用的功能。

{{< mermaid >}}classDiagram
    Collection <|-- Set
    Collection <|-- List
    Collection <|-- Queue
    Collection <|-- Deque
    Set <|-- SortedSet
    Map <|-- SortedMap
{{< /mermaid >}}

上图描述了核心 Collection 接口：

- **Collection**：集合表示一组称为其元素的对象。
- **Set**：集合，不能包含重复元素的集合。
- **List**：列表，有序集合（有时称为序列），可以包含重复的元素，可以控制每个元素在列表中的插入位置，并可以通过其整数索引（位置）访问元素。
- **Queue**：队列，先进先出（通常，除了优先级队列）的集合，只能在一端插入元素，一端删除元素。
- **Deque**：双向队列，在两端均可以插入和删除元素。
-
