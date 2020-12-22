---
title: String类及intern方法
top: false
cover: false
toc: true
date: 2020-07-26 12:04:57
password:
summary:
tags: ["面试"]
categories: [JAVA]
---

​	在JAVA语言中有8中基本数据类型和一种比较特殊的类型`String`。为了使这些类型在运行过程中速度更快，更节省内存，大部分包装类提供了一种常量池的概念（除了Float,Double）。常量池就类似一个JAVA系统级别提供的缓存。

<!-- more -->

### String类型使用（jdk1.8）

#### 例子

```java
public class StringTest {
    public static void main(String[] args) {
        // 使用 "" 创建  会直接存储在 常量池中
        String a = "a1";
        // 使用 new String 创建， 会将zahngsan存储到常量池中，然后在Heap中创建对象指向b
        String b = new String("zhangsan");
        // 使用 字符串连接符拼接 ，会直接存储 'wangwuzhaoliu' 字符串在常量池中
        String c = "wangwu" + "zhaoliu";
        // 使用字符串 "引用" 拼接 ，不执行 intern 方法，不会存放到常量池中，但是会将 --- 存入到常量池中
        String d = a + "---";
        // 使用 new String 拼接 ，不执行 intern 方法，不会存放到常量池中，但是会将wang 和 jiu 两个字符串存到常量池中
        String f = new String("wang") + "jiu";
        // 使用 new String 拼接 ，不执行 intern 方法，不会存放到常量池中， 但是会将 zhao 和 ba 两个字符串存入到常量池中
        String g = new String("zhao") + new String("ba");
    }
}
```

- **变量a：** "a1" 是**字符串常量**，在**编译期就被确定**了，先检查**字符串常量池中是否含有"a1"字符串**,若没有则**添加**"a1"到字符串常量池中，并且直接指向它。所以a直接指向**字符串常量池**的”a1”,也就是变量a指向的地址是**常量池中的 a1**。
- **变量b：\**用new String() 创建的字符串不是常量，\*\*不能在编译期就确定\*\*，所以new String() 创建的字符串不放入常量池中，它们有自己的地址空间\**(Java Heap 中)**，**变量b的引用的地址在Java Heap中**。 但是"zhangsan"字符串常量在编译期也会被加入到字符串常量池（如果常量池不存在的话）。
- **变量c：**"wangwu"和"zhaoliu"也都是**字符串常量**，当**一个字符串**由**多个字符串常量**连接而成时，它自己**肯定也是字符串常量**，在编译器会被编译器优化成"wangwuzhaoliu"，所以c也同样在编译期就被解析为一个字符串常量，并且c是常量池中”wangwuzhaoliu”的一个引用，**所以变量c的引用地址在常量池中**。
- **变量d：** JVM对于字符串引用，由于在字符串的”+”连接中，有字符串引用存在，而引用的值在程序编译期是无法确定的，即`(a+"---")
- **变量f：** 变量f同样不能在编译期确定，**但是"wang"和"jiu"这两个字符串常量会添加到字符串常量池中**，并且在堆中创建String对象。（字符串常量池并不会存放"wangjiu"这个字符串,除非执行f.intern()方法）
- **变量g：** 同理变量f。

#### Sring拼接

- **字符串拼接**

  ```java
  public class StringTest {
      public static void main(String[] args) {
          String a = "zhang" + "san";
      }
  }
  ```

  zhang和 san都是**字符串**，都是在编译器**可知的**，编译器会将这行代码优化，**当一个字符串是由多个可知的字符串(非引用字符串)连接组成**，将会优化为如下

  ```java
  public class StringTest {
      public static void main(String[] args) {
        String a = "zhangsan";  
      }
  }
  ```

- **引用拼接**

  ```java
  public class StringTest {
      public static void main(String[] args) {
          String a = "zhang";
        	String b = a + "san";
        	// 上下含义相同
        	String c = "li";
        	String d = "si";
        	String f = c + d;
      }
  }
  ```

  当Java编译器遇到**字符串引用** 或 **字符串引用和可知字符串**拼接的时候，会创建一个`StringBuilder`对象，后面的append()。

  因为有字符串引用存在，而引用的值在程序编译期是无法确定的。**另外 "zhang"、"san" 都会编译器添加到字符串常量池中（如果没有的话）**，因为它们都是编译期确定的字符串常量，**但是最后的"lantao"并不会添加到字符串常量池, 除非执行b.intern() 方法**

- **final拼接**

  ```java
  public class StringTest {
      public static void main(String[] args) {
          final String a = "zhang";
       		final String b = "san";
        	String c = a + b + "1111";
      }
  }
  ```

  final拼接和以上两者的区别就是在前边增加了final修饰，**用final修饰的字符串就是在编译期可知的**，编译期就会将以上代码优化为

  ```java
  public class StringTest {
      public static void main(String[] args) {
  				String str = "zhangsan1111";
      }
  }
  ```

  **这里 final 拼接的效果是和字符串拼接是一致的。**

### String#intern方法

#### intern方法详解

String.intern()是一个Native(本地)方法，它的作用是如果**字符串常量池已经包含一个等于此String对象的字符串，则返回字符串常量池中这个字符串的引用**, **否则将当前String对象的引用地址（堆中）添加到字符串常量池中并返回**。

#### 使用

**注意：基本数据类型之间的 == 是比较值，引用数据类型 == 比较的是地址值**

- ##### **常量池中存在字符串**

  ```java
  public class StringTest {
      public static void main(String[] args) {
        	// 基本数据类型之间的 ==  是比较值，引用数据类型 == 比较的是地址值
        	// 1：在Java Heap中创建对象 2：在字符串常量池中添加 zhangsan
          String a = new String("zhangsan");
        	// 调用 intern 方法，因上一步中已经将zhangsan存入常量池中，这里直接返回常量池 zhangsan 的引用地址
          String b = a.intern();
        	// a 的地址在Java Heap中 ， b的地址在 常量池中 ，所以结果是flase
          System.out.println(a == b);
        	// 因为常量池中已经包含zhangsan，所以直接返回
          String c = "zhangsan";
          // b c 的地址一致，所以是true
          System.out.println(b == c);
      }
  }
  
  //结果
  false
  true
  ```

  解释：

  1：在**Java Heap**中**创建对象**然后在**字符串常量**池中**添加** zhangsan。

  2：调用 **intern** 方法，因上一步中已经将zhangsan存入常量池中，这里直接返回常量池 zhangsan 的引用地址。

  3：因 a 的地址在Heap中，b的地址在字符串常量池中。

  4：因为常量池中已经包含zhangsan，所以直接返回

  5： b c 的地址一致，所以是true

  地址可以使用System.identityHashCode(a)方法获取

- ##### **常量池中不存在字符串**

  ```java
  public class StringTest {
      public static void main(String[] args) {
        	//1： 首先会在Heap中创建对象，然后在常量池中放入zhagnsan 和 wangwu ，但是并不会放入zhagnsanwangwu
          String a = new String("zhangsan") + "wangwu";
        	// 2：调用 intern ，因为字符串常量池中没有”zhangsanwangwu”这种拼接后的字符串，所以将堆中String对象的引用地址添加到字符串常量池中。jdk1.7后常量池引入到了Heap中，所以可以直接存储引用
          String b = a.intern();
        	// 3：因为 a 的地址和 b的地址一致，锁以是true
          System.out.println(a == b);
        
        	//4：因常量池中已经存在 zhangsanwangwu 了，所以直接返回引用就是 a 类型 a ==b 锁 a==b==c
          String c = "zhangsanwangwu";
          System.out.println(a == c); // true
        	System.out.println(b == c); // true
        
        	// 5：首先会在Heap中创建对象，然后会在常量池中存储 zhang 和 san
        	String d = new String("zhang") + "san";
        	// 6： 返回的是 常量池中的 地址，因在a变量时已经将 zhangsan 放入到了常量池中
        	String f = d.intern();
        	System.out.println(d = f); // false
      }
  }
  ```

  解释：

  1：首先会在**Heap中创建对象a，然后在**常量池中放入zhagnsan 和 wangwu** ，但是**并不会放入**zhagnsanwangwu。

  2：调用 intern ，因为字符串常量池中没有”zhangsanwangwu”这种拼接后的字符串，所以将堆中**String对象的引用地址添加到字符串常量池中**。jdk1.7后常量池引入到了Heap中，所以可以直接存储引用。

  3：因为 a 的地址和 b的地址一致，所以是true。

  4：因常量池中**已经存在 zhangsanwangwu** 了，所以**直接返回引用就是 a 类型**， a ==b 所以 a==b==c。

  5：首先会在**Heap中创建对象d**，然后会在**常量池中存储 zhang 和 san**。

  6：因在创建对象a时，已经将 "zhangsan"放入到了常量池，所以**返回的是常量池中的zhangsan地址**，对象d的地址在Heap中，f的地址在常量池中，并不是一个，所以false；

#### 参考：     [https://blog.csdn.net/qian520ao/article/details/78966179](https://blog.csdn.net/qian520ao/article/details/78966179#已存储字符串再intern)