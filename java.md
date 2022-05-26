[TOC]

# lambda

## 是什么

 Java变量可以被赋**“值”**

![e](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203160043274.png)

我想把**“一块代码”**赋给一个变量。

![image-20210203160212864](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203160212864.png)

Java 8之前做不到。Java 8后可以用Lambda实现

![image-20210203160238229](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203160238229.png)

这个写法并不是很简洁，使这个赋值操作更加优雅, 移除一些没用的声明。

![image-20210203160257667](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203160257667.png)

而**“这块代码”**，或者说**“这个被赋给一个变量的函数”**，就是一个Lambda表达式。

但是这里有一个问题，变量aBlockOfCode的类型应该是什么？

在Java 8里面，**所有的Lambda变量的类型都是一个接口，而Lambda表达式本身，也就是”那段代码“，是这个接口方法的实现（此接口只有一个方法）。**这是理解Lambda的一个关键所在，简而言之就是，**Lambda****表达式本身就是一个接口的实现**。

继续上面例子。给aBlockOfCode加上一个类型：

![https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWM0LnpoaW1nLmNvbS84MC92Mi01NWRlNjYwNjBiNGNiNzAxOTNkZGM3ZmVhMjAxYjI1N19oZC5qcGc?x-oss-process=image/format,png](file:///C:/Users/yunan10/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

我们自定义一个接口**MyLambdaInterface**,这里只有**一个接口函数需要被实现，我们叫它”函数式接口“。**为了避免别人在这个接口中增加其它接口函数**导致其有多个接口函数需要被实现，变成"非函数接口”**，可以在这个上面加上一个声明@FunctionalInterface, 这样别人就无法在里面添加新的接口函数会出现编译错误：

![https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMzLnpoaW1nLmNvbS84MC92Mi0yYzU3ZTc0MTFkZTIyN2QxZWIwOWMzMjdkMDFmYjc2Nl9oZC5qcGc?x-oss-process=image/format,png](file:///C:/Users/yunan10/AppData/Local/Temp/msohtmlclip1/01/clip_image003.jpg)

这样，我们就得到了一个完整的Lambda表达式声明：

![https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWM0LnpoaW1nLmNvbS84MC92Mi0wMmVlZGM1MjhmY2VlMTE1ZjVlZDBiN2IwNDU4NDZkN19oZC5qcGc?x-oss-process=image/format,png](file:///C:/Users/yunan10/AppData/Local/Temp/msohtmlclip1/01/clip_image005.jpg)

此时 **aBlockOfCode**变量调用**接口中的函数**会输出s的值。

## 2. 优点

**最直观的作用就是使代码变得异常简洁。**

我们可以对比一下Lambda表达式和传统的Java对同一个接口的实现：

![https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMyLnpoaW1nLmNvbS84MC92Mi1kYmQ0NmNmOWQxODhkMGZkZTI1ZGI3MDBjMjNkY2M3OV9oZC5qcGc?x-oss-process=image/format,png](file:///C:/Users/yunan10/AppData/Local/Temp/msohtmlclip1/01/clip_image007.jpg)

两种写法是等价的。显然Java 8中的写法更加简洁。

并且，如下图：由于Lambda可以直接赋值给一个变量，**我们就可以直接把Lambda作为参数传给函数, 而传统的Java必须有明确的接口实现的定义，初始化才行：**

![https://imgconvert.csdnimg.cn/aHR0cHM6Ly9waWMzLnpoaW1nLmNvbS84MC92Mi0yODYwNmY0MzI4MzA4YmFmN2Y3MGEzNmJkNjg5ZTVlYV9oZC5qcGc?x-oss-process=image/format,png](file:///C:/Users/yunan10/AppData/Local/Temp/msohtmlclip1/01/clip_image009.jpg)

有些情况下，这个接口实现只需要用到一次。传统的Java 7必须要求你定义一个“污染环境”的接口MyInterfaceImpl，而相较之下Java 8的Lambda, 就显得干净很多。

## 3. 用法

###### 3.1前提

1.需要函数式接口支持

2.函数式接口：只有一个抽象方法，并且不能跟Object类中的方法同名。jdk1.8中可以有多个default方法。

用@FunctionalInterface注解修饰(不是强制需要)，编译器会对该接口做检查，如果有多个抽象方法，会报错。

3.任何函数式接口都可以使用lambda表达式替换。

######  3.2 基础用法

java中，引入了一个新的操作符“->”，称为箭头操作符，或者lambda操作符；箭头操作符将lambda分成了两个部分：左侧为lambda表达式的参数列表，右侧为lambda表达式中所需要执行的功能，即lambda函数体

lambda表达式语法格式；

1）无参数，无返回值 ：() -> {System.out.println("hello lambda")；} 或() -> System.out.println("hello lambda")；（函数体只有一行可以省略{}）

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

``` java
Runnable r1 = ()->System.out.println("lambda");
r1.run();

//结果
lambda
```

2)一个参数,无返回值：(x) -> System.out.println(x);  或 x -> System.out.println(x); 一个参数可以省略参数的小括号

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

```java
Consumer<String> consumer = (x) -> System.out.println(x);
consumer.accept("lambda");
```

3）有两个参数，有返回值：(x, y) -> x + y；

```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
 
}
```

```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
    R apply(T t, U u);
}
```

```java
BinaryOperator<Integer> binary = (x, y) -> x + y;
System.out.println(binary.apply(1, 2));  // 3
```

4）多个参数依次类推

5）上面的例子，函数体中，都是一行语句，最后介绍下多行语句，无返回值和有返回值的的用法

```java
      // 无返回值lambda函数体中用法
      Runnable r1 = () -> {
                System.out.println("hello lambda1");
                System.out.println("hello lambda2");
                System.out.println("hello lambda3");
                };
       r1.run();

       // 有返回值lambda函数体中用法
       BinaryOperator<Integer> binary = (x, y) -> {
                int a = x * 2;
                int b = y + 2;
                return a + b;
        };
        System.out.println(binary.apply(1, 2));// 3
```

可以看到，多行的，只需要用大括号{}把语句包含起来就可以了；有返回值和无返回值的，只有一个return的区别；只有一条语句的，大括号和renturn都可以不用写；

6）lambda的类型推断

```java
BinaryOperator<Integer> binary = (Integer x, Integer y) -> x + y;
System.out.println(binary.apply(1, 2));// 3
```

可以看到，在lambda中的参数列表，可以不用写参数的类型，跟java7中 new ArrayList<>(); 不需要指定泛型类型，这样的<>棱形操作符一样，根据上下文做类型的推断

###### 3.3 方法的引用

前面讲述了lambda表达式使用的都是函数式接口

接下来介绍lambda表达式中，使用方法的引用，来传递方法的行为参数化；

方法的引用，在《java8实战》介绍如下：

方法的引用让你可以重复使用现有的方法定义，并像lambda一样传递他们，在一些情况下，比起使用lambda表达式，它们似乎更易读，感觉也更自然。

注意是：

①方法引用所引用的方法的参数列表与返回值类型，需要与函数式接口中抽象方法的参数列表和返回值类型保持一致！

②若Lambda 的参数列表的第一个参数，是实例方法的调用者，第二个参数(或无参)是实例方法的参数时，格式： ClassName::MethodName

方法的引用的语法，主要有三类

1.静态方法:指向静态方法的方法引用

类：：静态法名

例如Integer的parseInt方法 ，可以写成Integer::parseInt

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

```java
//lambda
Function<String,Integer> fun1 = (x) -> Integer.parseInt(x);
System.out.println(fun1.apply("123"));
 
//指向静态方法的方法引用
Function<String,Integer> fun2 = Integer::parseInt;
System.out.println(fun2.apply("123"));
```

2.实例方法：指向任意类型实例方法的方法引用，

类：：实例方法名

例如String的equals方法，写成String:: equals；

```java
@FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
}
```

```java
//lambda
BiPredicate<String, String> bp = (x, y) -> x.equals(y);
System.out.println(bp.test("a", "b"));
 
//指向任意类型实例方法的方法引用
BiPredicate<String, String> bp1 = String :: equals;
System.out.println(bp1.test("a", "b"));
```

3.对象实例方法：指向现有对象的实例方法的方法引用

对象：：实例方法名

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

```java
//lambda
Consumer<String> con1 = x -> System.out.println(x);
con1.accept("abc");
 
//指向现有对象的实例方法的方法引用
Consumer<String> con = System.out::println;
con.accept("abc");
```

4.构造器的引用

对于一个现有构造函数，你可以利用它的名称和关键字new来创建它的一个引用ClassName::new；

java8中的函数式接口提供了无参构造函数以及有参构造函数创建实例的方式,构造器的参数列表，需要与函数式接口中参数列表保持一致！

无参

```java
@Getter
@Setter
public class User {
    private String name;
    private Integer age;
}
```

```java
@FunctionalInterface
public interface Supplier<T> {
    T get()
}
```

```java
//lambda
Supplier<User> use = () -> new User();
System.out.println(use.get());
 
//无参
Supplier<User> use1 = User::new;
System.out.println(use1.get());
```

1个参数

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

```java
// lambda
Function<String, User> fun = name -> new User(name);
System.out.println(fun.apply("yunan").getName());
 
//一个参数
Function<String, User> fun1 = User::new;
System.out.println(fun1.apply("yunan").getName());
```

2个参数

```java
// lambda
BiFunction<String, Integer, User> bFun = (name, age) -> new User(name, age);
System.out.println(bFun.apply("yunan", 26));
// 两个参数
BiFunction<String, Integer, User> bFun1 = User::new;
System.out.println(bFun1.apply("yunan", 26));
```

#### 4.常用场景

1.替代匿名内部类
 毫无疑问，lambda表达式用得最多的场合就是替代匿名内部类，而实现Runnable接口是匿名内部类的经典例子。lambda表达式的功能相当强大，用()->就可以代替整个匿名内部类。

```java
//匿名内部类
new Thread(new Runnable() {
 @Override
 public void run() {
     System.out.println("The old runable now is using!");
 }
}).start();


//lamb
new Thread(() -> System.out.println("It's a lambda function!")).start();
```

2对集合进行迭代

```java
List<String> languages = Arrays.asList("java","scala","python");
//before java8
for(String each:languages) {
    System.out.println(each);
}

//lambda
languages.forEach(x -> System.out.println(x));
languages.forEach(System.out::println);
```

如果熟悉scala的同学，肯定对forEach不陌生。它可以迭代集合中所有的对象，并且将lambda表达式带入其中。

``` java
languages.forEach(System.out::println); 这一行看起来有点像c++里面作用域解析的写法，在这里也是可以的。
```

3.用lambda表达式实现map

```java
List<Double> cost = Arrays.asList(10.0, 20.0,30.0);
cost.stream().map(x -> x + x*0.05).forEach(x -> System.out.println(x));
```

map函数可以说是函数式编程里最重要的一个方法了。map的作用是将一个对象变换为另外一个。在我们的例子中，就是通过map方法将cost增加了0,05倍的大小然后输出。

4用lambda表达式实现map与reduce
既然提到了map，又怎能不提到reduce。reduce与map一样，也是函数式编程里最重要的几个方法之一。map的作用是将一个对象变为另外一个，而reduce实现的则是将所有值合并为一个，请看：

```java
List<Double> cost = Arrays.asList(10.0, 20.0,30.0);
double allCost = cost.stream().map(x -> x+x*0.05).reduce((sum,x) -> sum + x).get();
System.out.println(allCost);
```

最终的结果为：63.0

如果我们用for循环来做这件事情：

```java
List<Double> cost = Arrays.asList(10.0, 20.0,30.0);
 	double sum = 0;
 	for(double each:cost) {
 	each += each * 0.05;
 	sum += each;
}
System.out.println(sum);
```

相信用map+reduce+lambda表达式的写法高出不止一个level。

5 filter操作
 filter也是我们经常使用的一个操作。在操作集合的时候，经常需要从原始的集合中过滤掉一部分元素。

```java
List<Double> cost = Arrays.asList(10.0, 20.0,30.0,40.0);
List<Double> filteredCost = cost.stream().filter(x -> x > 25.0).collect(Collectors.toList());
filteredCost.forEach(x -> System.out.println(x));
```

最后的结果：

30

40

6与函数式接口Predicate配合
 除了在语言层面支持函数式编程风格，Java 8也添加了一个包，叫做 java.util.function。它包含了很多类，用来支持Java的函数式编程。其中一个便是Predicate，使用 java.util.function.Predicate 函数式接口以及lambda表达式，可以向API方法添加逻辑，用更少的代码支持更多的动态行为。Predicate接口非常适用于做过滤。

```java
public static void filterTest(List<String> languages, Predicate<String> condition) {
   languages.stream().filter(x -> condition.test(x)).forEach(x -> System.out.println(x + " "));
 }

 public static void main(String[] args) {
   List<String> languages = Arrays.asList("Java","Python","scala","Shell","R");
   System.out.println("Language starts with J: ");
   filterTest(languages,x -> x.startsWith("J"));
   System.out.println("\nLanguage ends with a: ");
   filterTest(languages,x -> x.endsWith("a"));
   System.out.println("\nAll languages: ");
   filterTest(languages,x -> true);
   System.out.println("\nNo languages: ");
   filterTest(languages,x -> false);
   System.out.println("\nLanguage length bigger three: ");
   filterTest(languages,x -> x.length() > 4);
 }
```

最后的输出结果：

```
Language starts with J: 
Java 
 
Language ends with a: 
Java 
scala 
 
All languages: 
Java 
Python 
scala 
Shell 
R 
 
No languages: 
 
Language length bigger three: 
Python 
scala 
Shell
```



可以看到，Stream API的过滤方法也接受一个Predicate，这意味着可以将我们定制的 filter() 方法替换成写在里面的内联代码。

#### 5.lambda与其它特性结合

1. Lambda结合Functional Interface Lib, forEach, stream()，method reference等新特性可以使代码变的更加简洁

假设Person的定义和List<Person>的值都给定。

![image-20210203182155099](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182155099.png)

现在需要打印出guiltyPersonsz中所有LastName以"Z"开头的人的FirstName。

**原生态Lambda写法**：定义两个函数式接口，定义一个静态函数，调用静态函数并给参数赋值Lambda表达式。

![image-20210203182223486](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182223486.png)

这个代码实际上已经比较简洁了，还可以更简洁么？

当然可以。在Java 8中有一个函数式接口的包，里面定义了大量可能用到的函数式接口（[java.util.function (Java Platform SE 8 )](https://link.zhihu.com/?target=https%3A//docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)）。所以压根都不需要定义NameChecker和Executor这两个函数式接口，直接用Java 8函数式接口包里的Predicate<T>和Consumer<T>就可以了——因为他们这一对的接口定义和NameChecker/Executor其实是一样的。

![image-20210203182244378](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182244378.png)

**第一步简化 - 利用函数式接口包：**

![image-20210203182253051](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182253051.png)

静态函数里面的for each循环其实是非常碍眼的。这里可以利用Iterable自带的forEach()来替代。forEach()本身可以接受一个Consumer<T> 参数。

**第二步简化 - 用Iterable.forEach()取代foreach loop：**

![image-20210203182302302](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182302302.png)

由于静态函数其实只是对List进行了一通操作，这里我们可以甩掉静态函数，直接使用stream()特性来完成。stream()的几个方法都是接受Predicate<T>，Consumer<T>等参数的（[java.util.stream (Java Platform SE 8 )](https://link.zhihu.com/?target=https%3A//docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)）。你理解了上面的内容，stream()这里就非常好理解了，并不需要多做解释。

**第三步简化 - 利用stream()替代静态函数：**

![image-20210203182310458](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182310458.png)

对比最开始的Lambda写法，这里已经非常非常简洁了。但是如果要求变成print这个人的全部信息，及p -> System.out.println(p); 那么还可以利用Method reference来继续简化。所谓Method reference, 就是用已经写好的别的Object/Class的method来代替Lambda expression。格式如下：

![image-20210203182324005](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182324005.png)

**第四步简化 - 如果是println(p)，则可以利用Method reference代替forEach中的Lambda表达式：**

![image-20210203182336031](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182336031.png)

这基本上就是最简洁的版本了。

 2.Lambda配合Optional<T>可以使Java对于null的处理变的异常优雅

这里假设我们有一个person object，以及一个person object的Optional wrapper:

![image-20210203182406091](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182406091.png)

Optional<T>如果不结合Lambda使用的话，并不能使原来繁琐的null check变的简单。

![image-20210203182414085](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182414085.png)

**只有当Optional<T>结合Lambda一起使用的时候，才能发挥出其真正的威力！**

我们现在就来对比一下下面四种常见的null处理中，Java 8的Lambda+Optional<T>和传统Java两者之间对于null的处理差异。

**情况一 - 存在**

![image-20210203182423185](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182423185.png)

**情况二 - 存在则返回，无则返回空**

![image-20210203182429573](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182429573.png)

**情况三 - 存在则返回，无则由函数产生**

![image-20210203182436273](C:\Users\yunan10\AppData\Roaming\Typora\typora-user-images\image-20210203182436273.png)

**情况四 – 多重null检查**



由上述四种情况可以清楚地看到，Optional<T>+Lambda可以让我们少写很多ifElse块。尤其是对于情况四那种多重null检查，传统java的写法显得冗长难懂，而新的Optional<T>+Lambda则清新脱俗，清楚简洁。

##### 附录：JDK提供的函数式接口

JDK 1.8之前已有的函数式接口

| java.lang.Runnable                  |
| ----------------------------------- |
| java.util.concurrent.Callable       |
| java.security.PrivilegedAction      |
| java.util.Comparator                |
| java.io.FileFilter                  |
| java.nio.file.PathMatcher           |
| java.lang.reflect.InvocationHandler |
| java.beans.PropertyChangeListene    |
| java.awt.event.ActionListener       |
| javax.swing.event.ChangeListener    |

Jdk1.8增加了一个新的包：java.util.function，里面包含了常用的函数式接口，总共43个分类如下： 

###### Consumer接口

| 接口        | 入参 | 出参 |
| ----------- | ---- | ---- |
| Consumer<T> | T    | void |

引申的Consumer接口:

| 接口                                                        | 入参                    | 出参             |
| ----------------------------------------------------------- | ----------------------- | ---------------- |
| IntConsumer  LongConsumer  DoubleConsumer                   | int  long  double       | void  void  void |
| ObjIntConsumer<T>  ObjLongConsumer<T>  ObjDoubleConsumer<T> | T,int  T,long  T,double | void  void  void |
| BiConsumer<T,U>                                             | T,U                     | void             |

######  Function接口

| 接口          | 入参 | 出参 |
| ------------- | ---- | ---- |
| Function<T,R> | T    | R    |

引申的Function接口:

| 接口                                                         | 入参              | 出参              |
| ------------------------------------------------------------ | ----------------- | ----------------- |
| IntFunction<R>  LongFunction<R>  DoubleFunction<R>           | int  long  double | R  R  R           |
| ToIntFunction<T>  ToLongFunction<T>  ToDoubleFunction<T>     | T  T  T           | Int  long  double |
| ToIntBiFunction<T,U>  ToLongBiFunction<T,U>  ToDoubleBiFunction<T,U> | T,U  T,U  T,U     | Int  long  double |
| IntToLongFunction  IntToDoubleFunction                       | int  int          | Long  double      |
| LongToIntFunction  LongToDoubleFunction                      | long  long        | int  double       |
| DoubleToIntFunction  DoubleToLongFunction                    | double  double    | int  long         |
| BiFunction<T,U,R>                                            | T,U               | R                 |

######  Predicate接口

| 接口         | 入参 | 出参    |
| ------------ | ---- | ------- |
| Predicate<T> | T    | boolean |

引申的Predicate接口：

| 接口                                         | 入参              | 出参                      |
| -------------------------------------------- | ----------------- | ------------------------- |
| IntPredicate  LongPredicate  DoublePredicate | Int  long  double | boolean  boolean  boolean |
| BiPredicate<T,U>                             | T,U               | boolean                   |

######  Supplier接口

| 接口        | 入参 | 出参 |
| ----------- | ---- | ---- |
| Supplier<T> | void | T    |

引申的Supplier接口：

| 接口                                                       | 入参                   | 出参                       |
| ---------------------------------------------------------- | ---------------------- | -------------------------- |
| IntSupplier  LongSupplier  DoubleSupplier  BooleanSupplier | void  void  void  void | int  long  double  boolean |

###### 其它接口

入参和出参同类型

| 接口名称                                                     | 入参                                     | 出参                 |
| ------------------------------------------------------------ | ---------------------------------------- | -------------------- |
| IntUnaryOperator  LongUnaryOperator  DoubleUnaryOperator  UnaryOperator<T> | int  long  double  T                     | int  long  double  T |
| IntBinaryOperator  LongBinaryOperator  DoubleBinaryOperator  BinaryOperator<T> | int, int  long, long  double,double  T,T | int  long  double  T |



#### #题

# 集合框架

### ArrayList

#### 是否会越界？ 

会，在多线程同时觉得不需要扩容，但是实际已经到了扩容的临界值的时候，就爆发了这个异常。

#### Arraylist LinkedList 异同

-  都是不同步的，不保证线程安全
- Arraylist 底层是Object数组；LinkedList 底层是双向循环链表；
- LinkedList 比 ArrayList 需要更多的内存  (ArrayList的空间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）
- LinkedList 在插入和删除数据时效率更高，ArrayList 在查找某个 index 的数据时效率更高；

#### 在ArrayList和LinkedList尾部加元素,谁的效率高

当输入的数据一直是小于千万级别的时候，大部分是LinkedList效率高，而当数据量大于千万级别的时候，就会出现ArrayList的效率比较高了。为什么呢？

原来 LinkedList每次增加的时候，会new 一个Node对象来存新增加的元素，所以当数据量小的时候，这个时间并不明显，而ArrayList需要扩容，所以LinkedList的效率就会比较高，如果ArrayList出现不需要扩容的时候，那么ArrayList的效率应该是比LinkedList高的。

当数据量很大的时候，new对象的时间大于扩容的时间，那么就会出现ArrayList的效率比LinkedList高了

#### 4、ArrayList,Vector,LinkedList的存储性能和特性是什么？

ArrayList 和Vector他们底层的实现都是一样的，都是使用数组方式存储数据，此数组元素数大于实际存储的数据以便增加和插入元素，它们都允许直接按序号查找元素，但是插入元素要涉及数组元素移动等内存操作，所以查找数据快而插入数据慢。

 Vector中的方法由于添加了synchronized修饰，因此Vector是线程安全的容器，但性能上较ArrayList差，因此已经是Java中的遗留容器。

 LinkedList使用双向链表实现存储（将内存中零散的内存单元通过附加的引用关联起来，形成一个可以按序号索引的线性结构，这种链式存储方式与数组的连续存储方式相比**，内存的利用率更高**，按序号索引数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快。

### 3.Iterator和ListIterator的区别是什么？ 

Iterator可用来遍历Set和List集合,但是ListIterator只能用来遍历List。Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引等等。

### 4.为什么集合类没有实现Cloneable和Serializable接口？ 

克隆(cloning)或者是序列化(serialization)的语义和含义是跟具体的实现相关的。因此，应该由集合类的具体实现来决定如何被克隆或者是序列化。

### 5. Java集合类框架的基本接口有哪些？ 

![image-20220526161138935](https://raw.githubusercontent.com/yn1007220096/picture/master/202205261611047.png)

​                               

1.总共有两大接口：Collection和Map，一个是元素集合，一个是键值对集合。

2.其中List接口和Set接口继承了Collection接口，一个是有序元素集合，一个是无序元素集合

3.ArrayList类和LinkList类实现了List接口

4.HashSet（哈希表、散列表）实现了Set接口

5.TreeSet实现了SortedSet接口（图上没画出来）。无序，不可重复，但可按照元素大小自动排序，或者自定义排序方法

6.HashMap和HashTable实现了Map，其中HashTable是线程安全的，但是HashMap性能更好

7.TreeMap实现了SortedMap接口（图上没画出来）。无序，不可重复，但可按照元素大小自动排序或者自定义排序方法

 

 

 

 

### 6. Hashmap

#### 1. 说一下HashMap

HashMap是基于哈希表(散列表)，实现Map接口的双列集合，数据结构是“链表散列”，也就是数组+链表 ，key唯一的value可以重复，允许存储null 键null 值，元素无序。

1.HashMap可以接受null键值和值。

2.HashMap是非线程安全;

3.HashMap查询很快；

4.HashMap储存的是键值对

 

#### 2.HashMap的工作原理 HashMap的get()方法的工作原理

HashMap是基于hashing的原理，

我们使用put(key, value)存储对象到HashMap中，使用get(key)从HashMap中获取对象。当我们给put()方法传递键和值时，我们先对key调用hashCode()方法，返回的hashCode用于找到bucket位置来储存Entry对象。”这里关键点在于指出，HashMap是在bucket中储存键对象和值对象，作为Map.Entry。这一点有助于理解获取对象的逻辑。如果你没有意识到这一点，或者错误的认为仅仅只在bucket中存储值的话，你将不会回答如何从HashMap中获取对象的逻辑。这个答案相当的正确，也显示出面试者确实知道hashing以及HashMap的工作原理。

 

#### 3.两个对象的hashcode相同会发生什么？

hashcode相同，所以它们的bucket位置相同，会发生碰撞。因为HashMap使用链表存储对象，这个Entry(包含有键值对的Map.Entry对象)会存储在链表中。”这个答案非常的合理，虽然有很多种处理碰撞的方法，这种方法是最简单的，也正是HashMap的处理方法。

在HashMap中采用链地址法来解决hash冲突，当发生碰撞是，将相同hash值的元素存储在链表的下一个节点处。除此之外还有开放地址法，再散列法。

 

 

#### 4.两个键的hashcode相同，你如何获取值对象？

当我们调用get()方法，HashMap会使用键对象的hashcode找到bucket位置，然后获取值对象。如果有两个值对象储存在同一个bucket，找到bucket位置之后，会调用keys.equals()方法去找到链表中正确的节点，最终找到要找的值对象。

 

许多情况下，面试者会在这个环节中出错，因为他们混淆了hashCode()和equals()方法。因为在此之前hashCode()屡屡出现，而equals()方法仅仅在获取值对象的时候才出现。一些优秀的开发者会指出使用不可变的、声明作final的对象，并且采用合适的equals()和hashCode()方法的话，将会减少碰撞的发生，提高效率。不可变性使得能够缓存不同键的hashcode，这将提高整个获取对象的速度，使用String，Interger这样的wrapper类作为键是非常好的选择。

 

#### 5.如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

HashMap中默认的负载因子为0.75，当HashMap中数组元素个数超过负载因子乘以数组总容量时会发生扩容，将数组扩展为原来的两倍，对原数组中的元素再进行rehashing运算，获得在新数组中的位置。扩容非常消耗性能。

#### HashMap的resize（rehash）

当 HashMap 中的元素越来越多的时候，hash冲突的几率也就越来越高，因为数组的长度是固定的，所以为了提高查询的效率，就要对HashMap的数组进行扩容，在HashMap数组扩容之后，最消耗性能的点是：原数组中的数据必须重新计算其在新数组中的位置，并放进去，这就是 resize（rehash）
 loadFactor指的是负载因子 HashMap能够承受住自身负载（大小或容量）的因子，loadFactor的默认值为 0.75认情况下，数组大小为 16，那么当HashMap中元素个数超过 *12* *的时候，就把数组的大小扩展为* 32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知 HashMap 中元素的个数，那么预设元素的个数能够有效的提高HashMap 的性能
 负载因子越大表示散列表的装填程度越高，反之愈小。对于使用链表法的散列表来说，查找一个元素的平均时间是 O(1+a)，因此如果负载因子越大，对空间的利用更充分，然而后果是查找效率的降低；如果负载因子太小，那么散列表的数据将过于稀疏，对空间造成严重浪费

#### 6.你了解重新调整HashMap大小存在什么问题吗？

你可能回答不上来，这时面试官会提醒你当多线程的情况下，可能产生条件竞争(race condition)。

当重新调整HashMap大小的时候，确实存在条件竞争，因为如果两个线程都发现HashMap需要重新调整大小了，它们会同时试着调整大小。在调整大小的过程中，存储在链表中的元素的次序会反过来，因为移动到新的bucket位置的时候，HashMap并不会将元素放在链表的尾部，而是放在头部，这是为了避免尾部遍历(tail traversing)。如果条件竞争发生了，那么就死循环了。这个时候，你可以质问面试官，为什么这么奇怪，要在多线程的环境下使用HashMap呢？：）

 

#### 7.为什么String, Interger这样的wrapper类适合作为键？

HashMap推荐使用不可变变量作为键,String最为常用。因为String是final修饰，是不可变的，并且重写了equals()和hashCode()方法。其他的wrapper类也有这个特点。不可变性是必要的，因为为了要计算hashCode()，就要防止键值改变，如果键值在放入时和获取时返回不同的hashcode的话，那么就不能从HashMap中找到你想要的对象。

 

不可变性还有其他的优点如线程安全。如果你可以仅仅通过将某个field声明成final就能保证hashCode是不变的，那么请这么做吧。因为获取对象的时候要用到equals()和hashCode()方法，那么键对象正确的重写这两个方法是非常重要的。如果两个不相等的对象返回不同的hashcode的话，那么碰撞的几率就会小些，这样就能提高HashMap的性能。

 

**我们可以使用自定义的对象作为键吗？**

可以，但是要确保这个对象重写了`hashCode()`和`equals()`方法，并且该对象插入Map后就不再改变，只要遵循了这些，它就已经可以作为键了。 

#### 9.我们能否让HashMap同步？

通过`Map m = Collections.synchronizeMap(hashMap)`实现同步。在、`synchronizeMap`中给HashMap的所有操作增加`synchronized`竞争监视器锁来实现线程同步。我们可采用`ConcurrentHashMap`

#### 10.我们可以使用CocurrentHashMap来代替Hashtable吗？``

```
可以，并且推荐使用CocurrentHashMap。HashTable采用synchronized实现同步，对其中所有方法添加监视器锁，问题在于，在一个线程进行put操作时，其他线程无法获得get或其他操作,`效率不是很高.`而CocurrentHashMap采用分段锁技术，比HashTable提供更强的线程安全。
```

#### 11.HashMap什么时候需要重写hashcode和equals方法？

```
当key值为对象时必须要求重写hashcode()和equals()方法。因为默认的hashcode()使用的是该值在内存中的地址作为该值的hash值，如果两个具有相同意义的对象进行比较时，由于其地址不同会导致计算出的hash值也不同。而重写equals()可以确保两个对象具有相同意义的属性。
```

#### 12.HashMap存储函数的实现put(K key, V value)

```
（1）计算key的hashCode()值
（2）然后对该哈希码值进行再哈希、
（3）然后把哈希值和(数组长度-1)进行按位与操作，得到存储的数组下标
（4）如果该位置处设有链表节点，那么就直接把包含<key,value>的节点放入该位置。如果该位置有结点，就对链表进行遍历，看是否有hash,key和要放入的节点相同的节点，如果有的话，就替换该节点的value值，如果没有相同的话，就创建节点放入值，并把该节点插入到链表表头(头插法)。
```

调用put 方法时候，如果key存在key不会覆盖，新的value会替代旧的value，返回旧的value;如果key不存在，该方法返回null

调用put时候，根据key的hashCode重新计算hash值，根据hash值得到这个元素在数组中的位置 也就是下标，如果数组在该位置上已有其他元素，那么在这个位置上的元素将以链表的形式存放，新加入的在立案表头，先加入的在尾部

```
public V put(K key, V value) {
    //其允许存放null的key和null的value，当其key为null时，调用putForNullKey方法，放入到table[0]的这个位置
    if (key == null)
        return putForNullKey(value);
    //通过调用hash方法对key进行哈希，得到哈希之后的数值。该方法实现可以通过看源码，其目的是为了尽可能的让键值对可以分不到不同的桶中
    int hash = hash(key);
    //根据上一步骤中求出的hash得到在数组中是索引i
    int i = indexFor(hash, table.length);
    //如果i处的Entry不为null，则通过其next指针不断遍历e元素的下一个元素。
    for (Entry<K,V> e = table[i]; e != null; e = e.next) {
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
 
    modCount++;
    addEntry(hash, key, value, i);
    return null;}
 
```

#### 13.为何数组的长度是2的n次方呢?

1.这个方法非常巧妙，它通过h & (table.length-1)来得到该对象的保存位，而HashMap底层数组的长度总是2的n次方，2^n -1得到的二进制数的每个位上的值都为1,那么与全部为1的一一个数进行与操作，速度会大大提升。

2.当length总是2的n次方时，h& (length-1)运 算等价王对length取模，也就是h%length,但是&比%县有更高的效率。

3.当数组长度为2的n次幂的时候，不同的key算得的index相同的几率较小，那么数据在数组上分布就比较均匀，也就是说碰擁的几率小，相对的，查询的时候就不用遍历某个位置上的链表，这样查询效率也就较高了。

#### 14.为什么阈值变为8变成红黑树？

当hashCode离散性很好的时候，树型bin用到的概率非常小，因为数据均匀分布在每个bin中，几乎不会有bin中链表长度会达到阈值。但是在随机hashCode下，离散性可能会变差，然而JDK又不能阻止用户实现这种不好的hash算法，因此就可能导致不均匀的数据分布。不过理想情况下随机hashCode算法下所有bin中节点的分布频率会遵循泊松分布，我们可以看到，一个bin中链表长度达到8个元素的概率为0.00000006，几乎是不可能事件。所以，之所以选择8，不是拍拍屁股决定的，而是根据概率统计决定的。

#### 15. HashMap中的hash碰撞问题，最坏的情况下，所有的key都被分配到了同一个桶，这时map的put和get时间复杂度都是O(n)。

#### 16.hashmap遍历方法，遍历过程中可以删除吗

 

```
public static void main(String[] args) {
    Map<String, Integer> map = new HashMap<>();
    map.put("a", 1);
    map.put("b", 2);
 
1.    Iterator<Map.Entry<String, Integer>> entryIterator = map.entrySet().iterator();
    while (entryIterator.hasNext()) {
        Map.Entry<String, Integer> next = entryIterator.next();
        System.out.println("key=" + next.getKey() + " value=" + next.getValue());
    }
 
运行结果没有显示，表明HashMap中的元素被正确删除了。
 
 2.   Iterator<String> iterator = map.keySet().iterator();
    while (iterator.hasNext()) {
        String key = iterator.next();
        System.out.println("key=" + key + " value=" + map.get(key));
    }
 
3.    // 等同于第一种方式  foreach模式，适用于不需要修改HashMap内元素的遍历，只需要获取元素的键/值的情况。
    for (Map.Entry<String, Integer> entry : map.entrySet()) {
        System.out.println("key=" + entry.getKey() + " value=" + entry.getValue());
    }
}
```

 

运行上面的代码，Java抛出了 java.util.ConcurrentModificationException 的异常。　可以推测，由于我们在遍历HashMap的元素过程中删除了当前所在元素，下一个待访问的元素的指针也由此丢失了。

#### 17.hashmap为什么要用红黑树

红黑树相比avl树，在检索的时候效率其实差不多，都是通过平衡来二分查找。但对于插入删除等操作效率提高很多。红黑树不像avl树一样追求绝对的平衡，他允许局部很少的不完全平衡，这样对于效率影响不大，但省去了很多没有必要的调平衡操作，avl树调平衡有时候代价较大，所以效率不如红黑树，在现在很多地方都是底层都是红黑树的天下啦~

### 7.、List、Map、Set三个接口存取元素时，各有什么特点？ 

List与Set都是单列元素的集合，它们有一个功共同的父接口Collection。

(1)Set里面不允许有重复的元素，

存元素：add方法有一个boolean的返回值，当 中没有某个元素，此时add方法可成功加入该元素时，则返回true；当集合含有与某个元素equals相等的 元素时，此时add方法无法加入该元素，返回结果为false。

取元素：没法取第几个，只能以Iterator接口取得所有的元素，再逐一遍历各个元素。

(2)List表示有先后顺序的集合，

存元素：多次调用add(Object)方法时，每次加入的对象按先来后到的顺序排序，也可以插队，即调用add(int index,Object)方法，就可以指定当前对象在集合中的存放位置。

取元素：方法1：Iterator接口取得所有，逐一遍历各个元素

​      方法2：调用get(index i)来明确说明取第几个。

 

(3)Map是双列的集合，存放用put方法:put(obj key,obj value)，每次存储时，要存储一对key/value，不能存储重复的key，这个重复的规则也是按equals比较相等。

取元素：用get(Object key)方法根据key获得相应的value。

​      也可以获得所有的key的集合，还可以获得所有的value的集合，

​       还可以获得key和value组合成的Map.Entry对象的集合。

List以特定次序来持有元素，可有重复元素。Set 无法拥有重复元素,内部排序。Map 保存key-value值，value可多值。

# JAVA基础

### 1.数组(Array)和列表(ArrayList)有什么区别？什么时候应该使用Array而不是ArrayList？ 

(1)存储内容比较：Array 数组可以包含基本类型和对象类型，ArrayList 却只能包含对象类型。Array 数组在存放的时候一定是同种类型的元素。ArrayList 就不一定了，因为ArrayList可以存储Object。

(2)空间大小比较：Array 数组的空间大小是固定的,所以需要事前确定合适的空间大小。ArrayList 的空间是动态增长的,如果空间不够，它会创建一个空间比原空间多大约0.5倍的新数组，然后将所有元素复制到新数组中，接着抛弃旧数组。而且，每次添加新的元素的时候都会检查内部数组的空间是否足够。

Arraylist扩充机制：newCapacity=oldCapacity+(oldCapacity>>1)但由于源码里（不再分析，这里简要略过）传过来的minCapcatiy的值是size+1，能够实现grow方法调用就肯定是(size+1)>elementData.length的情况，所以size就是初始最大容量或上一次扩容后达到的最大容量，所以才会进行扩容。因此，扩容后的大小应该是原来的1.5倍+1

(3)方法上的比较：

ArrayList 方法上比 Array 更多样化，比如添加全部 addAll()、删除全部 removeAll()、返回迭代器 iterator() 等。对于基本类型数据，集合使用自动装箱来减少编码工作量。但是，当处理固定大小的基本数据类型的时候，这种方式相对比较慢。

适用场景：

如果想要保存一些在整个程序运行期间都会存在而且不变的数据，我们可以将它们放进一个全局数组里， 但是如果我们单纯只是想要以数组的形式保存数据，而不对数据进行增加等操作，只是方便我们进行查找的话，那么，我们就选择 ArrayList。

如果我们需要对元素进行频繁的移动或删除，或者是处理的是超大量的数据，那么，使用 ArrayList 就真的不是一个好的选择，因为它的效率很低，使用数组进行这样的动作就很麻烦，那么，我们可以考虑选择 LinkedList。

### 2、String、StringBuffer、StringBuilder的区别

a.执行速度:StringBuilder > StringBuffer > String

b.线程安全:String StringBuffer线程安全. StringBuilder线程不安全

c.String适用与少量字符串操作

StringBuilder适用单线程下在字符缓冲区下进行大量操作的情况

StringBuffer使用多线程下在字符缓冲区进行大量操作的情况

### 3、Object若不重写hashCode()的话，hashCode()如何计算出来的？

Object的hashcode 方法是本地方法，也就是用 c 语言或 c++ 实现的，该方法直接返回对象的JVM 内存地址。

### 4、==比较的是什么？ 

1、对象引用类型,比较的是对象的内存地址。

public class ArrayTest {

```
public static void main(String[] args){
    String a = new String("aw");
    String b = new String("aw");
    System.out.println(a==b);//false
}
```

}

显然，尽管 a 与 b 对象的值相同，但是在内存中的地址是不同的，即两个对象是不一样的。再看一个例子：

```
public static void main(String[] args){
    String c= "aa";
    String d= "aa";
    System.out.println(c==d);//true
}
```

这是因为常量池的存在。而运行时常量池其实是属于方法区的一部分。通俗的说，c 和 d 其实都是都是指向 “aa”这个常量。

但是这里要注意，对于Integer对象来说，其能存储的范围为（-128~127），超过范围则存储到堆内存中。

2、基本类型数据，比较值。

### 5.若对一个类不重写，它的equals()方法是如何比较的？

如果没有对equals方法进行重写，会直接使用超类 Object 的 equals 方法，里面是用“==”比较，比较的是引用类型的变量所指向的对象的地址；诸如String、Date等类对equals方法进行了重写的话，比较的是所指向的对象的内容。

### 8、Java支持的数据类型有哪些？什么是自动拆装箱？

基本数据类型：

整数值型：byte,short,int,long

字符型：char

浮点类型：float,double

布尔型：boolean

整数默认int型，小数默认是double型。Float和long类型的必须加后缀。

首先知道String是引用类型不是基本类型，引用类型声明的变量是指该变量在内存中实际存储的是一个引用地址，实体在堆中。引用类型包括类、接口、数组等。String类还是final修饰的。

而包装类就属于引用类型，自动装箱和拆箱就是基本类型和引用类型之间的转换，至于为什么要转换，因为基本类型转换为引用类型后，就可以new对象，从而调用包装类中封装好的方法进行基本类型之间的转换或者toString（当然用类名直接调用也可以，便于一眼看出该方法是静态的），还有就是如果集合中想存放基本类型，泛型的限定类型只能是对应的包装类型。

### 9.什么是值传递和引用传递？ 

值传递:在方法的调用过程中，实参把它的实际值传递给形参，此传递过程就是将实参的值复制一份传递到函数中，这样如果在函数中对该值（形参的值）进行了操作将不会影响实参的值。因为是直接复制，所以这种方式在传递大量数据时，运行效率会特别低下。

引用传递:引用传递弥补了值传递的不足，如果传递的数据量很大，直接复过去的话，会占用大量的内存空间，而引用传递就是将对象的地址值传递过去，函数接收的是原始值的首地址值。在方法的执行过程中，形参和实参的内容相同，指向同一块内存地址，也就是说操作的其实都是源数据，所以方法的执行将会影响到实际对象。

  public class Example {

​    String str = new String("hello");

​    char[] ch = {'a', 'b'};

​    public static void main(String[] args) {

​      Example ex = new Example();

​      ex.change(ex.str, ex.ch);

​      System.out.println(ex.str + " and");

​      System.out.println(ex.ch);

​    }

​    public void change(String str, char[] ch) {

​      str = "ok";

​      ch[0] = 'c';

​    } }

输出是：

hello and

cb

过程分析：

1、为对象分配空间

2、 

2、执行change()方法

执行前实参（黑色）和形参（红色）的指向如下：

 

因为String是不可变类且为值传递，而ch[]是引用传递，所以方法中的str = "ok",相当于重新创建一个对象并没有改变实参str的值，数组是引用传递，直接改变，所以执行完方法后，指向关系如下：

· 基本数据类型传值，对形参的修改不会影响实参；

· 引用类型传引用，形参和实参指向同一个内存地址（同一个对象），所以对参数的修改会影响到实际的对象。

· String, Integer, Double等immutable的类型特殊处理，可以理解为传值，最后的操作不会修改实参对象。

### 11、String是最基本的数据类型吗? 

String不是基本的数据类型，是final修饰的java类。

### 12.int 和 Integer 有什么区别

(1)Integer是int的包装类，int则是java的一种基本数据类型 
 (2)Integer变量必须实例化后才能使用，而int变量不需要 
 (3)Integer实际是对象的引用，当new一个Integer时，实际上是生成一个指针指向此对象；而int则是直接存储数据值 
 (4)Integer的默认值是null，int的默认值是0

**延伸：关于Integer和int的比较** 
 (1)由于Integer变量实际上是对一个Integer对象的引用，所以两个通过new生成的Integer变量永远是不相等的（因为new生成的是两个对象，其内存地址不同）。

```
Integer i = new Integer(100);

Integer j = new Integer(100);

System.out.print(i == j); //false
```

(2)Integer变量和int变量比较时，只要两个变量的值是相等的，则结果为true（因为包装类Integer和基本数据类型int比较时，java会自动拆包装为int，然后进行比较，实际上就变为两个int变量的比较）

```
Integer i = new Integer(100);

int j = 100；

System.out.print(i == j); //true
```

(3)非new生成的Integer变量和new Integer()生成的变量比较时，结果为false。（因为非new生成的Integer变量指向的是java常量池中的对象，而new Integer()生成的变量指向堆中新建的对象，两者在内存中的地址不同）

```
Integer i = new Integer(100);

Integer j = 100;

System.out.print(i == j); //false
```

(4)两个非new生成的Integer对象

```
Integer i = 100;

Integer j = 100;

System.out.print(i == j); //true (-128-127)
Integer i = 128;

Integer j = 128;

System.out.print(i == j); //false
```


 java在编译Integer i = 100 ;时，会翻译成为Integer i = Integer.valueOf(100)；，而java API中对Integer类型的valueOf的定义如下：

```
public static Integer valueOf(int i){

    assert IntegerCache.high >= 127;

    if (i >= IntegerCache.low && i <= IntegerCache.high){

        return IntegerCache.cache[i + (-IntegerCache.low)];

    }

    return new Integer(i);

}
```

java对于-128到127之间的数，会进行缓存，Integer i = 127时，会将127进行缓存，下次再写Integer j = 127时，就会直接从缓存中取，就不会new了（java常量池？）

### 13.String 和StringBuffer和StringBuilder的区别

(1)String含义为引用数据类型,是字符串常量.是不可变的对象,(显然线程安全)在每次对string类型进行改变的时候其实都等同与生成了一个新的String对象.然后指针指向新的String对象,所以经常改变内容的字符串最好不使用String,因为每次生成对象都会对系统性能产生影响,特别当内存中无引用对象多了之后.JVM的垃圾回收(GC)就会开始工作,对系统的性能会产生影响 

(2)StringBuffer:线程安全的可变字符序列,对StringBuffer对象本身进行操作,而不是生成新的对象.所所以在改变对象引用条件下,一般推荐使用StringBuffer.同时主要是使用append和insert方法,

(3)StringBuilder:线程不安全的可变字符序列.提供一个与StringBuffer兼容的API,但不同步.设计作为StringBuffer的一个简易替换,用在字符缓冲区被单个线程使用的时候.效率比StringBuffer更快

### 15、&和&&的区别？ 

Java中&&和&都是表示与的逻辑运算符，都表示逻辑运输符and，当两边的表达式都为true的时候，整个运算结果才为true，否则为false。

&&的短路功能，当第一个表达式的值为false的时候，则不再计算第二个表达式；&则两个表达式都执行。



