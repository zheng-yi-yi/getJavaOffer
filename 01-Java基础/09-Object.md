## Object类的常见方法有哪些

Java中的所有类都直接或间接继承自`Object`类，因此`Object`类的方法对所有Java对象都可用。

以下是`Object`类的一些常见方法：

1. **`public String toString()`**：返回对象的字符串表示。默认实现返回的是对象的类名和哈希码的无符号十六进制表示。

2. **`public boolean equals(Object obj)`**：检查此对象是否等于指定的对象。默认实现是比较两个对象的内存地址（即`==`运算符的行为）。

3. **`public int hashCode()`**：返回对象的哈希码值，用于支持哈希表。如果两个对象通过`equals(Object)`方法比较相等，那么它们的`hashCode()`方法必须返回相同的整数。

4. **`protected Object clone() throws CloneNotSupportedException`**：创建并返回此对象的一个副本。默认实现是浅拷贝。

5. **`public final Class<?> getClass()`**：返回此对象运行时的类。

6. **`public final void wait() throws InterruptedException`**：使当前线程等待，直到其他线程调用此对象的`notify()`方法或`notifyAll()`方法。

7. **`public final void notify()`**：唤醒在此对象监视器上等待的单个线程。

8. **`public final void notifyAll()`**：唤醒在此对象监视器上等待的所有线程。

> 注意：
>
> `equals(Object)`和`hashCode()`方法通常需要在自定义类中重写，以实现符合业务逻辑的相等性检查和哈希码计算。

## == 和 equals() 的区别

1. **`==`运算符**：对于基本数据类型，`==`比较的是值是否相等；对于引用类型，`==`比较的是两个引用是否指向同一个对象（即比较的是内存地址）。

2. **`equals()`方法**：这是一个方法，不是运算符。它的行为取决于如何在具体的类中实现。在`Object`类中，`equals()`方法的默认行为和`==`运算符相同，比较的是两个对象的内存地址。但在一些类（如`String`、`Integer`、`Date`等）中，`equals()`方法已被重写，用于比较两个对象的内容是否相等。

因此，当我们需要比较两个对象的内容是否相等时，通常应该使用`equals()`方法。但如果我们的目标是比较两个引用是否指向同一个对象，或者比较两个基本数据类型的值，那么可以使用`==`运算符。

## 谈谈hashCode()和equals()的关系

1. **一致性**：如果两个对象通过`equals(Object)`方法比较相等，那么它们的`hashCode()`方法必须返回相同的整数。这是因为在哈希表中，相等的对象必须具有相同的哈希码，以便它们能被放入同一个哈希桶中。

2. **不一致性**：如果两个对象的`hashCode()`方法返回的哈希码相同，那么这两个对象不一定相等。也就是说，`hashCode()`方法的相等性并不要求`equals(Object)`方法的相等性。

在自定义类中，如果你重写了`equals(Object)`方法，那么通常也需要重写`hashCode()`方法，以保持它们之间的一致性。否则，你的类可能无法正确地在哈希表等集合中使用。

> 总结：
>
> - 哈希码相等的对象不一定相等
> - 相等的对象必须有相同的哈希码

## 为什么要重写hashCode()和equals()

在Java中，`hashCode()`和`equals()`方法是`Object`类的成员方法，所有的Java对象都继承了这两个方法。然而，`Object`类的默认实现可能并不适合所有的类，特别是那些需要自定义相等性检查逻辑的类，这就需要我们重写这两个方法。

为什么重写？原因如下：

1. **自定义相等性检查**：`Object`类的`equals()`方法默认比较的是两个对象的内存地址，也就是说，只有当两个引用指向同一个对象时，`equals()`方法才会返回`true`。然而，在许多情况下，我们希望能够基于对象的内容（即对象的字段）来判断它们是否相等，这就需要重写`equals()`方法。

2. **保持`hashCode()`和`equals()`的一致性**：在Java中，如果两个对象相等（即`equals(Object)`方法返回`true`），那么它们的哈希码（即`hashCode()`方法的返回值）必须相同。这是因为在哈希表（如`HashMap`、`HashSet`等）中，哈希码被用来确定对象的存储位置。如果相等的对象有不同的哈希码，那么哈希表的性能将大大降低。因此，当我们重写`equals()`方法时，通常也需要重写`hashCode()`方法，以保持它们的一致性。

3. **在集合中使用**：许多Java集合类（如`HashSet`、`HashMap`、`Hashtable`等）在存储和检索元素时，都依赖于`hashCode()`和`equals()`方法。如果我们的类的实例将被用作这些集合的元素，那么我们必须正确地重写这两个方法，以确保集合的正确行为。