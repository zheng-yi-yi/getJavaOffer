## 你对static关键字有哪些认识

| 特点     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| 静态变量 | 被 `static` 修饰的变量，也称为类变量，属于类本身而不是实例，类的所有实例共享同一个静态变量。静态变量在类加载时初始化，只初始化一次。 |
| 静态方法 | 被 `static` 修饰的方法，可以直接通过类名调用，而不需要创建类的实例。静态方法只能访问静态变量，不能访问非静态变量，也不能使用 `this` 关键字。 |
| 静态块   | 一组在类加载时执行的语句块，通常用于初始化静态变量。         |
| 静态类   | 只有嵌套类（内部类）可以被声明为静态的。静态内部类不持有对外部类的引用，无法访问外部类的非静态成员。 |

举例：

```java
public class MyClass {
    static int staticVar = 10; // Static variable

    static { // Static block
        System.out.println("Static block is called");
    }

    static void staticMethod() { // Static method
        System.out.println("Static method is called");
    }

    static class StaticNestedClass { // Static class
        // Some code here
    }
}
```

## static和final有什么区别

`static` 和 `final` 是Java中的两个关键字，它们有不同的用途：

1. **static**：`static` 关键字用于声明类级别的变量和方法。`static` 变量被所有类的实例共享，而 `static` 方法可以在不创建类的实例的情况下直接通过类名调用。`static` 关键字还可以用于创建静态块和静态内部类。

2. **final**：`final` 关键字用于声明不能被修改的变量、不能被重写的方法和不能被继承的类。
   - 当 `final` 用于变量时，该变量成为常量，一旦赋值就不能再改变。
   - 当 `final` 用于方法时，该方法不能在子类中被重写。
   - 当 `final` 用于类时，该类不能被继承。

## final、finally、finalize的区别

1. **final**：`final` 关键字用于声明不能被修改的变量、不能被重写的方法和不能被继承的类。

2. **finally**：`finally` 关键字用于创建在 `try`/`catch` 语句块后执行的代码块，无论是否发生异常，`finally` 块中的代码总是被执行。这通常用于清理资源，如关闭文件、数据库连接等。

3. **finalize**：`finalize` 是 `Object` 类的一个方法，它在垃圾收集器准备回收对象所占内存之前被调用。这个方法可以被子类重写，以确保清理对象持有的资源。但是，Java不推荐使用 `finalize` 方法，因为Java提供了更好的资源管理和清理机制，如 `try-with-resources` 和 `java.lang.ref` 包。

举例：

```java
public class MyClass {
    final int finalVar = 10; // final variable

    void myMethod() {
        try {
            // Some code here
        } catch (Exception e) {
            // Handle exception
        } finally {
            // Cleanup code
        }
    }

    @Override
    protected void finalize() throws Throwable {
        // Cleanup code
        super.finalize();
    }
}
```
