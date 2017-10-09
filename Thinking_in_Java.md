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
String.matches().
String.split().

