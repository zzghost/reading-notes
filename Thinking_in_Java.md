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
