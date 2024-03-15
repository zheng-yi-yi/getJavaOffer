## HashSet、LinkedHashSet 和 TreeSet 有什么区别

> 总结：
>
> - `HashSet`使用哈希表，不保证元素顺序；
> - `LinkedHashSet`继承自`HashSet`，使用链表维护插入顺序；
> - `TreeSet`使用红黑树，保证元素有序。

`HashSet`、`LinkedHashSet`和`TreeSet`都是Java中的Set接口的实现，区别如下：

1. `HashSet`：它是基于哈希表实现的，不保证元素的顺序，允许存储一个`null`值。添加、删除和查找元素的时间复杂度都是`O(1)`。

2. `LinkedHashSet`：它是`HashSet`的一个子类，内部使用链表维护元素的插入顺序，允许存储一个`null`值。在遍历`LinkedHashSet`时，元素的顺序是按照它们被插入到集合中的顺序。添加、删除和查找元素的时间复杂度都是`O(1)`。

3. `TreeSet`：它是基于红黑树（自平衡的排序二叉树）实现的，元素会按照自然顺序（通过`Comparable`接口）或者自定义的比较器（通过`Comparator`接口）进行排序。添加、删除和查找元素的时间复杂度都是`O(log n)`。

## Set 和 Map 有什么区别？

`Set` 代表无序的，元素不可重复的集合，主要用于检查一个元素是否存在。

`Map`是一个键值对的集合，每组元素都包含一个键和一个值。键在`Map`中是唯一的，每个键最多只能映射到一个值。`Map`主要用于通过键快速查找值。