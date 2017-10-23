# Inheritance
1. Definitions:  
  + *superclass*,*base class*, *parent class* are the same. *subclass*,*derived class*, *child class* are the same.  
  + Overriding(重写)：subclass re-write the methods in the superclass.  
  Subclass constructor can use `super` syntax to call superclass's constructor. This call statement must be the first statement in the constructor for the subclass. If the subclass constructor doesn't call a superclass constructor explicitly, the no-argument constructor of the superclass is invoked. If the superclass doesn't have a no-argument constructor and the subclass constructor doesn't call another superclass constructor explicitly, the Java compiler reports an error.  
  + Polymophism:  
  **Liskov Substitution Principle LSP**: You can use a subclass object whenever the program expects a superclass object.  
  ```java
  Manager boss = new Manager(..);
  Employee[] staff = new Employee[3];
  staff[0] = boss; //use LSP, correct
  boss.setBonus(..); //boss is Manager, correct
  staff[0].setBonus(..); // staff[0] is not Manager, incorrect
  ```
  + **static binding** and **dynamic binding**: If a method is *private*, *static*, *final* or a constructor, then the compiler knows exactly which method to call. Otherwise, the method to be called depends on the actual type of the implicit parameter, and dynamic binding must be used at runtime. It would be time consuming for JVM to pick up the most appropriate method. Therefore, JVM precomputes for each class a *method table* that lists all method signitures and the actual methods to be called. JVM will make a table lookup.
  + *final* class and methods: preventing inheritance. One reason to use *final* to define a method or class: to make sure its semantics cannot be changed in a subclass.Like accessors and mutators methods.
  + Casting:
  You can cast only within an inheritance hierarchy.  
  Use *instanceof* to check before casting from a superclass to a subclass.  
  Actually, converting the type of an object by a cast is not usually a good idea. The only reason to make the cast is to use a method that is unique to subclass. In general, it is best to minimize the use of casts and the *instanceof* operator.  
  + Abstract Classes: They can have concrete methods, which means not all methods in an abstract class are abstract.A class can be declared abstract though it has no abstract methods.Abstract class cannot be instantiated, but you can still create *object variables* of an abstract class, but such a variable must refer to an obj of a nonabstract subclass, like *Person p = new Student("Vince", "Economics")*, Person is an abstract class while Student is not.    
  When you extends an abstract class, you have two choices: 1) leave some or all abstract methods undefined, but tag the subclass as abstract. 2)Or, you can define all methods, and the subclass is no longer abstract.  
  + Protected Access:
  protected field: allow subclass's methods to access protected superclass's field and they only get the permission of watch this kind of field.  
  protected method: subclasses can use superclass's method.
  + Summary of the four access modifiers in Java:  
    1. Visible to the class only.(private)
    2. Visible to the world.(public)
    3. Visible to the package and all subclasses(protected).
    4. Visible to the package, the (unfortunate)default.No modifiers are needed.
2. Object: The Cosmic Superclass
every class in Java extends class *Object*.There's *equals(),  hashCode(), toString()* method.A hash code is an integer that is derived from an object.If two objects are distinct, x.hashCode() and y.hashCode() may(not surely) be different.Equal objects need to return identical hash codes. These sections are about redefine these methods. *toString()* method is a good tool for logging.  
3. Generic Array Lists  
It's been used for many times.Here's another tip to keep in mind:  
```java
public class EmployeeDB{
  public void update(ArrayList list){...}
  public ArrayList find(String query){...}
}
ArrayList<Employee> staff = ...;
employeeDB.update(staff);//correct
ArrayList<Employee> result = employeeDB.find(query); //warning
ArrayList<Employee> result = (ArrayList<Employee>)employeeDB.find(query); //another warning
//correct:
@SuppressWarnings("unchecked") ArrayList<Employee> result = (ArrayList<Employee>)employeeDB.find(query); //another warning.
```
For compatibility, the compiler translates all typed array lists into raw *ArrayList* objects after checking that the type rules were not violated. In a running program, all array lists are the same, there are no type parameters in the JVM. Thus, the casts (ArrayList) and (ArrayList<Employee>) carry out identical runtime checks.  
4. Object Wrappers and Autoboxing  
*Wrappers*: turn primitive types to their objects: Integer, Long, Short, Byte, Float, Double, Boolean, Character.(The first 6 inherit from common superclass *Number*).Wrappers are immutable and **final**.
We can see that `ArrayList<Integer>` takes Integer objects as input. Fortunately, however, there's a useful feature:   
*Autoboxing*: `list.add(3)` is automatically translated to `list.add(Integer.valueOf(3))`  
*Unboxing*: `int n = list.get(i)` is automatically translated to `int n = list.get(i).intValue()`
5. Methods with a Variable Number of Parameters  
like System.out.printf() method  
6. Enumaration Classes  
7. Reflection(It's heavily used in *JavaBeans*)   
*Reflective:* A program that can analyze the capabilities of classes.  
*Reflection:* a powerful tool to 1)analyze the capabilities of classes at runtime; 2) Inspect objects at runtime,e.g.,to write a single toString() that works for all classes; 3) Implement generic array manipulation code; 4) Take advantage of Method objects that work just like function pointers in languages such as C++.  
It's useful to tool builders, not application programmers.  
---skip---
8. Design Hints for Inheritance
  + Place common operations and fields in the superclass.  
  + Don't use protected fields.  
  Reasons: 1) anyone can form a subclass of your classes and then write code that directly accesses protected instance fields, thereby breaking encapsulation. 2)all classes in the same package have access to protected fields.  
  + Use inheritance to model the "is-a" relationship
  + Don't use inheritance unless all inherited methods make sense.  
  + Don't change the expected behavoir when you override a method.  
  + Use Polymophism, not type information.  
  + Don't overuse reflection.
