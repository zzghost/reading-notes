## Chapter 13
### 13.1 不可变String
String是类，String对象是不可改变的，每一个看起来修改了String对象的方法其实都是在创建一个全新的String对象。  
### 13.2 重载"+"与StringBuilder
由于String对象的不可变特性，因而在拼接字符串时，需要不断构造并垃圾回收中间对象，影响效率。作者从JVM字节码的角度，举例向我们展示了拼接字符串的过程，发现使用+号拼接时，编译器“自作主张”地使用了java.util.StringBuilder类。但这并不意味着，我们可以依赖编译器的优化而随意使用String对象。此处作者展示了在循环结构中隐式创建StringBuilder和显式创建时的JVM字节码。得出如下结论：  
```
在循环结构中显式创建StringBuilder，只会生成一个StringBuilder对象；而隐式调用会在每一次循环中创建一个新的StringBuilder对象
显式创建的另一个好处是，可以预定义大小，避免多次重新分配缓冲。
如果不涉及比较复杂的String操作，可以不用StringBuilder；否则还是使用它比较好。
```
StringBuilder的常用方法：append(),delete(int start, int end),toString(),insert(),replace(),substring(),reverse()  
### 13.3 无意识的递归
java中的类基本都继承自Object类，标准容器类也是如此。所以每个容器类肯定也有toString()方法。  
当新建一个类并在里边重写toString()方法时，如果要得到该对象的内存地址，就不该使用this而应该使用super.toString()方法。因为this会调用重写的toString()方法，从而进行递归调用，最终导致抛出StackOverflowError。  
### 13.4 String上的操作
构造器、length()、chatAt()、getChars()/getBytes()、toCharArray()、equals()/equalsIgnoreCase()、compareTo()、contains()、contentEquals()  
equalsIgnoreCase()、regionMatcher()、startsWith()、endsWith()、indexOf()、lastIndexOf()、substring()、concat()、replace()、toLowerCase()toUpperCase()、trim()、valueOf()、intern()  
从这些方法当中，可以总结出：如果需要改变字符串的内容，String类的方法就会返回一个新的String对象；如果对象的内容没有发生改变，则返回的是指向原对象的引用。
### 13.5 格式化输出
System.out.printf()与System.out.format()是等价的，类似于C语言中的printf()。  
Formatter类是用来处理新的格式化功能。  
width:控制域的最小尺寸，适用于所有数据类型；precision:指明最大尺寸,对不同的数据类型功能不同，例如String:输出字符的最大数量,浮点:小数部分位数,precision无法用于整数  
### 13.6 正则表达式
1. String类自带的正则表达式工具有  
"String".matches("regex")：检查一个string是否匹配正则表达式  
"String".split("regex")：将字符串从正则表达式匹配处切开  
"String".replaceFirst("regex","replaceString"),"String".replaceAll("regex","replaceString"):将匹配正则表达式的第一个/所有子串替换为其他String.  
2. 创建正则表达式  
可查看JDK文档java.util.regex包中的[Pattern类](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)
3. 创建比String类更强大的正则表达式对象  
官网文档案例：  
```java
Pattern p = Pattern.compile("a*b");//regular expression,create a pattern
Mathcer m = p.matcher("aaaab");//user string,create a matcher
boolean b = m.matches();
```
compile()的另一个版本：compile("regex",int flag),flag来自Pattern类中的7个常量（自行查阅）  
一个matcher对象可以用来做三种操作：  
matches()：判断整个输入字符串是否匹配正则表达式  
lookingAt()：部分匹配，从第一个字符开始匹配，无论匹配成功与否则不再继续匹配  
find(),find(int i)：部分匹配，从i位置（不指定则默认从0开始）开始匹配，找到一个匹配的子串，下一次匹配的位置从上一次位置开始  
**Groups的概念**  
用括号区分不同的组  
类似于A(B(C))D，共有3个组：0组ABCD，1组BC，2组C。  
Matcher对象提供了一系列方法以获取组相关信息：public int groupCount(),public String group(),public String group(int i),public int start(int group),public int end(int group)  
**替换操作**  
replaceFirst()  
replaceAll()  
appendReplacement(StringBuffer,replacementString),替换，并且把替换后的子串以及其之前到上次匹配子串之后的字符串段添加到StringBuffer中  
appendTail(StringBuffer)，在appendReplacement()方法后执行，用于将剩余未处理部分存入StringBuffer中。
reset("string")：将现有的Matcher对象应用于一个新的串上
4. 正则表达式与java I/O
作者以应用正则表达式在一个文件中进行搜索匹配操作，做了一次演示。最后输出的是有匹配的部分以及匹配在行中的位置。
### 13.7 扫描输入
引出Scanner类，读入String用nextLine(),读入int用nextInt(),读入double用nextDouble()  
但是，想要说的是，Scanner类可以结合正则表达式使用，比如Scanner对象可以调用useDelimiter("regex")方法来设置界定符。  
还有delimiter()，可以返回当前正在作为界定符的Pattern对象  
作者还举例：使用自定义的正则表达式结合Scanner类扫描防火墙日志文件中记录的威胁数据  
scanner.hasNext(pattern), scanner.next(pattern)表示找到下一个匹配pattern的输入部分,再调用match()方法可以获得匹配结果  
```java
while(scanner.hasNext(pattern)){
    scanner.next(pattern);
    MatchResult match = scanner.match();
    String ip = match.group(1);
    String date = match.group(2);
}
```
要注意的是:*如果pattern里边有界定符，则匹配不可能成功，因为scanner永远是针对下一个输入进行匹配，有界定符的话就匹配不上*
### StringTokenizer
作者举了个例子展示StringTokenizer类，表明现在基本是用Scanner和正则表达式，因为后者可以使用更加复杂的模式来分割一个字符串  
## Chapter 14 RTTI（一大堆坑待填）
### 14.1 为什么要用RTTI
简单来说，“多态”是面向对象编程的基本目标，而这是基于RTTI来实现的，而RTTI又是通过Class对象来完成的
### 14.2 Class对象
每个类都会有一个Class对象。在编写后并且编译一个新类时，就会产生一个Class底箱。为了生成一个类的这个对象，JVM使用了*类加载器*子系统。这个子系统可以包含一系列类加载器链，但是只有一个*原生类加载器*，它加载可信类，比如Java API类。  
Java的一个特性就是，所有类都是动态加载到JVM中的。所以Java程序在它刚开始运行时没有都加载，而是各个部分在需要时才会被加载进来。类加载器首先会检查Class对象是否已经加载。如果没有，就去查找.class文件。在加载入内存后，它就会被用来创建这个类的所有对象。
当程序创建第一个对类的静态成员的引用时，就会加载这个类，这个说明即使没有标明static, 类的构造器也是类的静态方法。在使用new操作符创建类的新对象时，这个过程也被当作是对类的静态成员的引用。    
Java有以下两种方法来获取对Class对象的引用：  
1） Class.forName("classname"):找加载类，如果找不到则抛出ClassNotFoundException的异常。和2）不同的是，这种方法调用后会立刻进行初始化。  
2） ClassName.class.基本类型也包含.class方法，而他们的包装器类则有.TYPE字段，作者建议使用.class，为了保持一致。   
.class的准备工作有三个：1） 加载，类加载器会去查找字节码，从字节码中创建Class对象。2） 链接，验证字节码，为静态域分配存储空间，并且若必需的话，将解析这个类创建的对其他类的所有引用。 3）初始化，如果该类具有超类，则对其初始化，执行静态初始化器和初始化块。。。。所以使用.class来创建Class对象的引用时，不会自动初始化该Class对象，而是延迟到了对静态方法或者非常数静态域进行首次引用时才执行。  
关于此过程，书P319给出了一个详细的例子，对于理解初始化比较有用，可以再复习。这个例子说明，对于static final型的编译时常量，所以不需要初始化就可以直接读取，但是只加上static final却不能保证可以直接读取，因为它有可能跟超类有关，这就需要用到上边第3）条，强制初始化，再访问。如果一个static域不为final，则对它访问时，要先进行1）和2）的过程。  
Class引用也有泛型语法。Class<Integer>,Class<Number>,Class<?>,Class<? extends Number>.为它添加泛型语法主要是为了提供编译期类型检查。  
Class引用还有转型语法。classname.class.cast(对象)方法，将对象转为classname类对象.  
### 14.3 类型转换前先做检查  
RTTI在Java中的三种形式：  
1）普通类型转换，比如（Shape），RTTI会确保转换的正确性，否则抛出ClassNotFoundException.  
2) Class对象。通过查询Class对象可以获得RTTI。  
3）**instanceof**关键字。它返回布尔值，告诉我们某一对象是否为某个类的实例。  
这一节没细读，待填坑
### 14.4 注册工厂
介绍了工厂方法的设计模式，待填坑
### 14.5 instanceof与Class等价性
作者给了一个例子，说明instanceof与Class的区别：instanceof含义是“你是这个类或者它的派生类吗？”，而Class是指你的确切类型。  
### 14.5 反射：运行时的类信息
在编译时，编译器必需知道所有要通过RTTI来处理的类，也就是说，编译器会在编译时打开和检查.class文件。但是，现实世界很多情况下，无法在编译时获取RTTI。比如：1）在IDE里边拖拽编程。 2）跨网络的远程平台上创建和运行对象，Java程序会将对象分布在多台机器上。这时就有了**反射**。.class文件在编译时不可获取，在运行时才打开与检查。  
例子和解说都没看明白，待填坑
### 14.7 动态代理
介绍了**代理**设计模式。待填坑
### 14.8 空对象
**空对象**允许我们假设所有的对象都是有效的，而不必再花精力去判断一个对象是否为null.  
作者写了一个扫雪机器人程序，用了代理设计模式，创建了空机器人，需要创建空机器人时，就可以new。照着他的程序敲完，发现还是有点小问题：会抛出空指针异常错误。待填坑。
### 14.9 接口与类型信息
看不懂interface的解耦相关。待填坑。

## Chapter 15 泛型
## Chapter 16 数组
