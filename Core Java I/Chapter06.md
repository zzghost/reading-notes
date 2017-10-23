# Interfaces, Lambda Expressions, and Inner Classes
1. Interfaces(A set of *requirements* for the classes that want to conform to the interface.)   
  + Definition:  
  A class can implement not only one interfaces. Interface describes what classes should do, without specifying how they should do it.  
  + Properties:  
    1. Interfaces are not classes, so you cannot *new* them, but you can declare them like "InterfaceName x;" instead of "x = new InterfaceName(..);".  
    2. Use *instanceof* to check whether an obj is of a specific class.  
    3. Interfaces can be extended.  
    4. You cannot put instance fields or static methods in it, but you can put constants in it. Just as methods in an interface are automatically *public*, fields are always *public static final*
  + Compare to Abstract Classes:  
      A class can only extends a single class, but it can implement as many interfaces as it likes.
  + In Java 8, *static* and *default* keyword can be used to methods in Interfaces.  
  Resolving Default method Conflicts:
  They occur when method is defined as a default method in one interface, and then defined as a method of a superclass or another interface.  
  Here are tips:
    1. Superclasses win if the method in it is concrete.  
    2. Interfaces clash. if the method in superclass is also a default one, then you must resolve the conflict by overriding the method.  
  +  Three examples of interfaces.
    1. Interfaces and Callbacks.(ActionListener, Event)
    2. The Comparator Interface: Sort strings by length.
    3. **Object Cloning**. Object.clone() is shallow copy. classes should implement Cloneable interface, do super.clone(), and then copy the mutable fields.  
2. Lambda Expressions
Augment Java for functional programming.  
*functional interface*: in Java, it's best to think of a lambda expression as a function, not an object. Lambda Expressions can be passed to a functional interface. It's like: Arrays.sort(words, (first, second) -> first.length() - second.length()); the second expression actually is an object of some class that implements Comparator<String>. Invoking the compare method on that object executes the body of the lambda expression.  
*method reference*:  
There are 3 principal cases:   
  + object::instanceMethod
  + Class::staticMethod
  + Class::instanceMethod  
  The first two cases, the method reference is equivalent to a lambda expression that supplies the parameters of the method.System.out::println is equivalent to x -> System.out.println(x). Similarly, Math::pow is equivalent to (x, y) -> Math.pow(x, y)  
  The third case, the first parameter becomes the target of the method. String::compareToIgnoreCase is same as (x, y) -> x.compareToIgnoreCase(y)
  You can use *this* and *super* keywords.
*Constructor reference* : Person::new, Person[]::new
```java
ArrayList<String> names = ...;
//Person::new is a reference to a Person constructor.
Stream<Person> stream = names.stream().map(Person::new);
List<Person> people = stream.collect(Collectors.toList());
```
3. Inner Classes
3 Properties:
  + Inner Class methods can access the data from the scope in which they are defined, including private.
  + can be hidden from other classes in the same package.
  + Anonymous inner classes are handy when you want to define Callbacks without writing a lot of code.
*OuterClass.this*: reference to the outer object.  

4. Proxies(Objects that implement arbitrary interfaces)  
Use it at runtime, to create new classes that implement a given set of interfaces.  
