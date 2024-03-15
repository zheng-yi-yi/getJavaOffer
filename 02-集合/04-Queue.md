## ArrayDeque 与 LinkedList 的区别

`ArrayDeque`和`LinkedList`都是Java中的队列（Queue）实现：

- 对于内部实现来说，`ArrayDeque`是基于循环数组实现的，而`LinkedList`是基于双向链表实现的。
- `ArrayDeque`的插入和删除操作通常比`LinkedList`更快，`LinkedList`的每个元素都需要额外的空间存储前后节点的链接，而`ArrayDeque`不需要。
- `LinkedList`除了实现`Deque`接口外，还实现了`List`接口，因此它可以作为一个列表使用，支持索引访问。而`ArrayDeque`只实现了`Deque`接口，不支持索引访问。
- `ArrayDeque`在扩容时会创建一个新的数组，所以可能会有一些空间浪费。`LinkedList`则是每次添加元素时分配空间，所以空间利用率更高。


## 说⼀说你对BlockingQueue的了解

`BlockingQueue` 是 Java 并发包 `java.util.concurrent` 中的一个接口，主要用于在多线程之间安全地传递数据。它提供了阻塞操作，以便在队列为空或队列已满时进行等待或阻塞。

以下是 `BlockingQueue` 的主要特性：

1. **阻塞操作**：`BlockingQueue` 提供了 `put` 和 `take` 方法。`put` 方法用于向队列中添加元素，如果队列已满，则会阻塞直到有空间可用。`take` 方法用于从队列中获取元素，如果队列为空，则会阻塞直到有元素可用。

2. **超时操作**：`offer` 和 `poll` 方法允许设置超时参数，以便在超时后返回而不是无限期等待。`offer` 方法用于向队列中添加元素，如果队列已满，则会等待指定的超时时间，超时后返回 false。`poll` 方法用于从队列中获取元素，如果队列为空，则会等待指定的超时时间，超时后返回 null。

3. **容量限制**：`BlockingQueue` 可以具有有界或无界的容量。有界队列的容量是固定的，而无界队列可以无限制地添加元素。

4. **支持公平性**：一些 `BlockingQueue` 的实现支持公平性，即在多个等待线程中，按照等待时间的顺序选择下一个执行的线程。

5. **线程安全**：所有的 `BlockingQueue` 实现都是线程安全的，因为它们内部都使用了锁和其他并发控制机制来保证操作的原子性。

6. **批量操作**：`BlockingQueue` 还有一个 `drainTo` 方法，可以一次性从 `BlockingQueue` 中移除所有可用的元素，并将它们添加到另一个集合中。

Java 提供了多种 `BlockingQueue` 的实现，如 `ArrayBlockingQueue`、`LinkedBlockingQueue`、`PriorityBlockingQueue`、`DelayQueue` 等，它们各有各的使用场景。

例如，`ArrayBlockingQueue` 是一个基于数组结构的有界阻塞队列，此队列按 FIFO（先进先出）原则对元素进行排序；`LinkedBlockingQueue` 是一个基于链表结构的阻塞队列，此队列按FIFO （先进先出）排序元素，吞吐量通常要高于 `ArrayBlockingQueue`；`SynchronousQueue` 是一个不存储元素的阻塞队列，每个插入操作必须等待另一个线程的相应移除操作，反之亦然。