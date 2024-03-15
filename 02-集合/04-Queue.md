## ArrayDeque 与 LinkedList 的区别

`ArrayDeque`和`LinkedList`都是Java中的队列（Queue）实现：

- 对于内部实现来说，`ArrayDeque`是基于循环数组实现的，而`LinkedList`是基于双向链表实现的。
- `ArrayDeque`的插入和删除操作通常比`LinkedList`更快，`LinkedList`的每个元素都需要额外的空间存储前后节点的链接，而`ArrayDeque`不需要。
- `LinkedList`除了实现`Deque`接口外，还实现了`List`接口，因此它可以作为一个列表使用，支持索引访问。而`ArrayDeque`只实现了`Deque`接口，不支持索引访问。
- `ArrayDeque`在扩容时会创建一个新的数组，所以可能会有一些空间浪费。`LinkedList`则是每次添加元素时分配空间，所以空间利用率更高。