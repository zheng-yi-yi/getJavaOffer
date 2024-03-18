# 谈谈volatile关键字

`volatile`是Java中的一个关键字，它主要用于确保变量的可见性和有序性。

1. **可见性**：当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去主存中读取新值。而普通的共享变量不能保证可见性，因为普通共享变量被修改之后，什么时候被写入主存是不确定的，当其他线程去读取时，此时内存中可能还是原来的旧值，导致数据的不一致。

2. **有序性**：在Java内存模型中，允许编译器和处理器对指令进行重排序，但是重排序过程不会影响到单线程程序的执行，却会影响到多线程并发执行的正确性。在Java中，volatile也能够阻止指令重排序。volatile关键字会对标记的变量进行特殊处理，确保指令不会重排序。

需要注意的是，volatile并不能保证原子性。也就是说，volatile不能替代`synchronized`或`Lock`，它无法保证复杂的线程安全问题。

例如，以下的代码就不能保证i的值最后为20000：

```java
public class VolatileTest {
    public volatile int i = 0;

    public void add() {
        i++;
    }
}
```

# 如何理解 volatile 保证变量的可见性？

在多线程环境下，为了提高效率，每个线程都会有一个工作内存（可以理解为CPU的高速缓存），线程对变量的所有操作都会在自己的工作内存中进行，然后再同步回主内存中。如果一个变量被`volatile`关键字修饰，那么这个变量就具备了可见性。

可见性是指当一个线程修改了这个变量的值，新值对于其他线程来说是可以立即得知的。也就是说，如果线程A把变量V修改为V1，那么线程B能够立即看到这个修改。

具体来说，当一个共享变量被`volatile`修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去主存中读取新值。而普通的共享变量不能保证可见性，因为普通共享变量被修改之后，什么时候被写入主存是不确定的，当其他线程去读取时，此时内存中可能还是原来的旧值，导致数据的不一致。

因此，`volatile`关键字能够保证变量的可见性，使得每次读取的都是最新的值。

# 谈谈 volatile 确保变量有序性的例子

在Java中，`volatile`关键字可以防止JVM的指令重排序，确保变量的有序性。这是通过插入特定的内存屏障来实现的。

这里有一个常见的例子，就是双重检查锁定（Double-Checked Locking）实现单例模式：

```java
public class Singleton {
    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

在这个例子中，`uniqueInstance`变量被`volatile`关键字修饰。

`uniqueInstance = new Singleton();`这段代码其实是分为三步执行：

1. 为`uniqueInstance`分配内存空间
2. 初始化`uniqueInstance`
3. 将`uniqueInstance`指向分配的内存地址

但是由于JVM具有指令重排的特性，执行顺序有可能变成1->3->2。指令重排在单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。例如，线程T1执行了1和3，此时T2调用`getUniqueInstance()`后发现`uniqueInstance`不为空，因此返回`uniqueInstance`，但此时`uniqueInstance`还未被初始化。

使用`volatile`关键字修饰`uniqueInstance`后，可以防止这种指令重排，确保`uniqueInstance`在被初始化之后才被其他线程看到。


# 为什么 volatile 不能保证对变量的操作是原子性的？

来看这段代码：

```java
/**
 * 微信搜 JavaGuide 回复"面试突击"即可免费领取个人原创的 Java 面试手册
 *
 * @author Guide哥
 * @date 2022/08/03 13:40
 **/
public class VolatoleAtomicityDemo {
    public volatile static int inc = 0;

    public void increase() {
        inc++;
    }

    public static void main(String[] args) throws InterruptedException {
        ExecutorService threadPool = Executors.newFixedThreadPool(5);
        VolatoleAtomicityDemo volatoleAtomicityDemo = new VolatoleAtomicityDemo();
        for (int i = 0; i < 5; i++) {
            threadPool.execute(() -> {
                for (int j = 0; j < 500; j++) {
                    volatoleAtomicityDemo.increase();
                }
            });
        }
        // 等待1.5秒，保证上面程序执行完成
        Thread.sleep(1500);
        System.out.println(inc);
        threadPool.shutdown();
    }
}
```

理想状态下，我们希望上述代码的运行结果是2500。但实际结果却是小于2500的。

这个问题的原因在于`inc++`这个操作并不是原子性的。它实际上是分为三步进行的：读取`inc`的值，将`inc`的值加1，然后将新的值写回到`inc`。在这三步操作中，如果有其他线程也在进行`inc++`操作，就可能导致数据的不一致。

比如线程 1 对 inc 进行读取操作之后，还未对其进行修改。线程 2 又读取了 inc的值并对其进行修改（+1），再将inc 的值写回内存。线程 2 操作完毕后，线程 1 对 inc的值进行修改（+1），再将inc 的值写回内存。虽然两个线程分别对 inc 进行了一次自增操作，但 inc 实际上只增加了 1。

因此我们说，虽然`volatile`关键字能保证变量的可见性，但是它不能保证操作是原子性的。也就是说，`volatile`不能保证复杂的线程安全问题。

备注：解决上述代码可以有多种方法：

1. 使用`synchronized`关键字：

```java
public synchronized void increase() {
    inc++;
}
```

2. 使用`AtomicInteger`：

```java
public static AtomicInteger inc = new AtomicInteger(0);

public void increase() {
    inc.getAndIncrement();
}
```

3. 使用`Lock`：

```java
private Lock lock = new ReentrantLock();

public void increase() {
    lock.lock();
    try {
        inc++;
    } finally {
        lock.unlock();
    }
}
```

以上三种方法都可以保证`inc++`操作的原子性，从而解决这个问题。