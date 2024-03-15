## 谈谈String的存储原理

| 特点         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| 不可变性     | `String` 对象一旦创建，内容不可改变，改变值会创建新的对象    |
| 字符串比较   | 使用 `==` 比较对象引用，应使用 `equals` 方法比较值           |
| 字符串常量池 | 提高性能和减少内存使用，存储字符串字面量（例如，`String s = "hello"`，Java会首先在字符串常量池中查找是否已经存在值为 "hello" 的 `String` 对象。如果存在，Java就会返回这个已存在的对象的引用；如果不存在，Java就会在字符串常量池中创建一个新的 `String` 对象，并返回新对象的引用。） |
| 堆内存       | 使用 `new` 创建的 `String` 对象存储在堆内存中。例如，`String s = new String("hello")`，Java会在堆内存中创建一个新的 `String` 对象，其中存的是指向“字符串常量池”的内存地址，然后在栈中村的是指向堆的引用地址。 |
| 字符串拼接   | 在Java中，可以使用 `+` 运算符或 `StringBuilder` / `StringBuffer` 类来拼接字符串。由于 `String` 对象是不可变的，所以每次使用 `+` 运算符拼接字符串时，都会在内存中创建一个新的 `String` 对象，这可能会导致内存和性能问题。因此，对于大量的字符串拼接操作，建议使用 `StringBuilder` 或 `StringBuffer`。 |

## String 能被继承吗

不，`String` 类在 Java 中被声明为 `final`，因此它不能被继承。在 Java 中，`final` 关键字可以用于修饰类、方法和变量，用于表示它们不能被修改。

## String底层使用的是什么类型

**在JDK1.8之前，String类的底层使用的是 `char` 数组来存储字符数据**。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {

    private final char[] value;
    // ...
}
```

**在 Java 9 及以后的版本中**，为了优化性能和内存使用，**使用一个 `byte` 数组**和一个编码标志字段来存储字符数据。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {

    private final byte[] value;

    private final byte coder;
    // ...
}
```

## String/StringBuffer/StringBuilder的区别

这三个类都是用于处理字符串的，但它们的主要区别在于可变性和线程安全性。

1. **String**：`String`是不可变的。这意味着一旦创建了`String`对象，就不能改变它。如果你尝试改变它，实际上会创建一个新的`String`对象。

2. **StringBuffer**：`StringBuffer`是可变的。`StringBuffer`的对象可以改变，不会创建新的对象。此外，`StringBuffer`是线程安全的，所有的公共方法都是同步的，所以它适合多线程编程。

3. **StringBuilder**：`StringBuilder`也是可变的，和`StringBuffer`类似。但是，`StringBuilder`不是线程安全的，因为它的公共方法没有同步。因此，它在单线程环境下的性能比`StringBuffer`要好。

总结： `String` 适⽤于不经常修改的情况，⽽ `StringBuffer` 和 `StringBuilder` 适⽤于需要频繁修改字符串的情况，具体选择取决于是否需要线程安全以及性能的考虑。

## 列举一下String类的常见方法

1. **`length()`**：返回字符串的长度。

   ```java
   String str = "Hello";
   int len = str.length();  // len = 5
   ```

2. **`charAt(int index)`**：返回指定索引处的字符。

   ```java
   String str = "Hello";
   char ch = str.charAt(1);  // ch = 'e'
   ```

3. **`substring(int beginIndex, int endIndex)`**：返回一个新的字符串，它是此字符串的一个子字符串。

   ```java
   String str = "Hello";
   String sub = str.substring(1, 4);  // sub = "ell"
   ```

4. **`indexOf(String str)`**：返回指定子字符串在此字符串中第一次出现处的索引。

   ```java
   String str = "Hello";
   int index = str.indexOf("l");  // index = 2
   ```

5. **`equals(Object anObject)`**：将此字符串与指定的对象比较。

   ```java
   String str1 = "Hello";
   String str2 = "Hello";
   boolean isEqual = str1.equals(str2);  // isEqual = true
   ```

6. **`toUpperCase()`** 和 **`toLowerCase()`**：将此字符串转换为全大写或全小写。

   ```java
   String str = "Hello";
   String upper = str.toUpperCase();  // upper = "HELLO"
   String lower = str.toLowerCase();  // lower = "hello"
   ```

7. **`trim()`**：返回字符串的副本，忽略前导空白和尾部空白。

   ```java
   String str = " Hello ";
   String trimmed = str.trim();  // trimmed = "Hello"
   ```

8. **`replace(CharSequence target, CharSequence replacement)`**：返回一个新的字符串，通过用指定的替换项替换此字符串所有匹配的目标项得到的。

   ```java
   String str = "Hello";
   String replaced = str.replace('l', 'w');  // replaced = "Hewwo"
   ```

9. **`startsWith(String prefix)`**：测试此字符串是否以指定的前缀开始。

   ```java
   String str = "Hello";
   boolean starts = str.startsWith("He");  // starts = true
   ```

10. **`endsWith(String suffix)`**：测试此字符串是否以指定的后缀结束。

    ```java
    String str = "Hello";
    boolean ends = str.endsWith("lo");  // ends = true
    ```

11. **`contains(CharSequence s)`**：当且仅当此字符串包含指定的 char 值序列时，返回 true。

    ```java
    String str = "Hello";
    boolean contains = str.contains("ell");  // contains = true
    ```

12. **`split(String regex)`**：根据给定的正则表达式的匹配拆分此字符串。

    ```java
    String str = "Hello, World";
    String[] parts = str.split(", ");  // parts = ["Hello", "World"]
    ```

13. **`valueOf(data type)`**：返回给定数据类型的字符串表示形式。

    ```java
    String str = String.valueOf(123);  // str = "123"
    ```

14. **`isEmpty()`**：当且仅当 `length()` 为 `0` 时返回 true。

    ```java
    String str = "";
    boolean isEmpty = str.isEmpty();  // isEmpty = true
    ```

15. **`concat(String str)`**：将指定字符串连接到此字符串的结尾。

    ```java
    String str1 = "Hello";
    String str2 = " World";
    String str = str1.concat(str2);  // str = "Hello World"
    ```

16. **`compareTo(String anotherString)`**：按字典顺序比较两个字符串。

    ```java
    String str1 = "Hello";
    String str2 = "World";
    int cmp = str1.compareTo(str2);  // cmp < 0 because "Hello" is less than "World"
    ```

## 什么是字符串常量池

字符串常量池（*String Constant Pool*）是Java堆内存中的一个特殊区域，用于存储所有的字符串字面量。这是Java为了提高性能和减少内存开销而采用的一种优化策略。

当你创建一个字符串字面量（例如，`String str = "Hello";`）时，Java首先会检查字符串常量池中是否已经存在"Hello"。如果存在，Java就会返回对已存在的"Hello"的引用，而不是创建一个新的对象。如果不存在，Java就会在字符串常量池中创建一个新的"Hello"，然后返回对它的引用。

这种优化策略有两个主要的好处：

1. **节省内存**：由于相同的字符串字面量共享同一个存储位置，所以可以大大减少内存的使用。

2. **提高比较效率**：对于字符串常量池中的字符串，我们可以使用`==`运算符来比较它们的引用，而不是使用`equals()`方法来比较它们的内容，这可以提高字符串比较的效率。

> **注意：**
>
> 这种优化策略只适用于字符串字面量。对于通过`new`关键字创建的字符串对象（例如，`String str = new String("Hello");`），Java会在堆内存中创建一个新的对象，而不是使用字符串常量池。

## 字符串拼接发⽣了什么

| 方法                               | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| 使用`+`运算符拼接                  | 在每次拼接时创建新的字符串对象，效率较低。如果拼接的都是字符串直接量，编译器会在编译时优化为一个完整的字符串。如果拼接字符串中包含变量，编译器会使用`StringBuilder`进行优化。但在循环中使用时，每次循环都会创建一个新的`StringBuilder`实例。 |
| 使用`concat()`方法                 | 每次调用都会创建一个新的字符串对象，不修改原有字符串对象。可通过`intern()`方法使新创建的字符串对象指向字符串常量池中的字符串，节省内存。 |
| 使用`StringBuilder`/`StringBuffer` | 可通过`append()`方法多次追加字符串，操作在同一个可变对象上进行，不产生大量的中间字符串对象。最后，通过`toString()`方法将可变对象转换为不可变字符串对象。 |

## String str = new String("abc") 和 String str = "abc" 的区别

1. **内存中的位置**：`String str = new String("abc")`会在堆内存中创建一个新的字符串对象，同时，"abc"这个字符串字面量也会被放入字符串常量池（如果常量池中还没有这个字符串）。所以，这个表达式实际上至少创建了一个对象（在堆内存中），可能创建了两个对象（如果字符串常量池中还没有"abc"）。而`String str = "abc"`只会检查字符串常量池中是否已经存在"abc"，如果存在，就直接返回对它的引用，如果不存在，就在字符串常量池中创建一个新的字符串。
2. **字符串的比较**：由于`String str = new String("abc")`每次都会创建新的字符串对象，所以使用`==`运算符比较两个通过`new String("abc")`创建的字符串时，结果是`false`。而`String str = "abc"`创建的字符串都是在字符串常量池中，所以使用`==`运算符比较两个通过`String str = "abc"`创建的字符串时，结果是`true`。

通常推荐使⽤ `String a = "abc"` 这种⽅式，因为它更符合字符串的常⽤特性，避免了不必要的对象创建。