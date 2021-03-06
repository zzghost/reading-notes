# Object and Classes
1. For *Objects*, there are 3 key characteristics of them:  
First, *Behavior*: usually the methods.  
Second, *State*: a consequence of method calls.  
Third, *Identity*: may be identical items. Objects from the same Class differ in state and identity.  
These keys can influence each other.  
2. *final* instance field denotes that such field must be initialized when the object is constructed.That is, you must guarantee that the field value has been set after the end of every constructor.
3. *implicit* and *explicit* parameters:  
*implicit* means the parameters are the objects of the class.The keyword **this** refers to the implicit parameters.They don't appear in the method declaration.  
*explicit* means we can see in the method declaration.  
4. *static* field denotes that there's only such field per class.But each object has its own copy of all instance fields.
For example:
```java
class Employee{
  private static int countId = 1;
  private int Id;
}
```
All Employee objects share one `countId`, but they have their own `Id`.  
*static* constants(they have `final`) are common, such as `Math.PI`, `System.out`.    
It's not a good idea to have public fields because everyone can modify them. However, public constants are fine, since they are declared as `final`.  
*static* methods.They do not operate on objects.Thus, they don't have implicit methods so they can't access `this` parameter.But they can access static fields.     
Use *static* to define methods in two situations:   
  + The method don't have to access implicit parameters, which means they don't have to access objects.  
  + The method only needs to access static fields.  
**main methods are static methods because they don't need operate the objects.In fact, they generate class objects. **
5. *call by value* and *call by reference*
*call by value* : methods just gets the value that the caller provides.  
*call by reference* : methods gets the *location* of the variable that the caller provides.  
There are two kinds of method parameters:Primitive types(numbers, boolean values) and object references. **Java always uses call by value.** Thus, we can change an object reference instead of a primitive type. The method gets a copy of the object reference, and both the original and the copy refer to the same object.
There's an example we must keep in mind. If java is call by reference, this swap would work. It turns out that this doesn't work which means Java is totally call by value.**Object references are passed by value.**    
```java
public static void swap(Employee x, Employee y){
  Employee temp = x;
  x = y;
  y = temp;
}
public static void main(String[] args){
  Employee a = new Employee("Alice", ...);
  Employee b = new Employee("Bob", ...);
  swap(a, b);
}
```  
Here's a summary:
  + A method cannot modify a primitive parameter.  
  + A method can change the state of a object parameter.  
  + A method cannot make an object parameter refer to a new object.  
6. Constructors:  
  + Overloading(重载): It occurs if several methods have the same name but different parameters. Java allows you to overload any method, not just constructor methods. parameter types are used to let the compiler sort out the right method.They are called the signiture of the method. Take care that the return types are not signiture.  
  + Default Field Initialization(默认域初始化): In constructor method, if you don't explicitly set a field, it will set a default value: numbers to 0, boolean to false, object references to null.  
  + The Constructor with No Arguments: If you don't want the arguments to be initialized to default values above, you can write constructor with no arguments to initialize the default values.Take care that if you write constructor with arguments instead of no arguments, you cannot create the objects without arguments.    
  + Explicit Field Initialization: The initialization value doesn't have to be a constant value. It can be initialized with a method call, like this `private int id = assignId()`.It will run before constructor method.  
  + Parameter Names: You can write names in the constructor method parameter lists like `name = aName` and `salary = aSalary`, which means adding an prefix "a". Or, use `this.name = name`.
  + Calling Another Constructor:  
  *this* has two meaning: First, it refers to the implicit parameter of a method. Second, if the *the first statement of a constructor* has the form `this(...)`, then the constructor calls another constructor of the same class.  
  ```java
  //example of *this* keyword
  public Employee(double s){
    //*this* will call Employee(String name, double salary) constructor.
    this("Employee #" + nextId, s);
    nextId++;
  }
  ```
  + Initialization Blocks: There are three ways to initialize a data field:  1.set a value in a constructor. 2.assign a value in the declaration. 3.Initialization block.(Not so common).   
  **Initialize a static field:** supply an initial value or use static initialization block.Example: `private static int nextId =1` or `static{ nextId = ...}`  
  **Here's what happens in detail when a constructor is called:**  
  Step1: All data fields initialized to default values.  
  Step2: All fields initializers and initialization blocks are executed, in the order in which they occur in the class declaration.But the static initialization blocks always runs before object initialization blocks.  
  Step3: If the first line of the constructor calls a second constructor, then the body of the second constructor is executed.  
  Step4: The body of the constructor is executed.
  + Object Destruction and the *finalize* Method: Java does automatic garbage collection and manual memory reclamation is not needed, so Java does not support destructors. However, some objects may not only use memory, but also other resources, like a file or a handle to another object that uses system resources.In this case, it is imporant that the resources be reclaimed and recycled when they are no longer needed. You can use `finalize` method. This method will be called before garbage collector sweeps away the object. **In practice, do not rely on the finalize method for recycling any resources that are in shor supply, because you simply can know when this method will be called. If a resource needs to be closed as soon as you have finished using it, you needed to manage it manually. Supply a close method that does the nessary cleanup and call it when you are done with the object.**
7. Packages.  
Access modifier: *public* and *private*. *public* means it can be used by any class. *private* means it can only be used by the class that defines them. If there's no public and private, the feature can be accessed by all methods in the *package*.  
8. Class Design Hints
  + Always keep data private.
  + Always initialize data.
  + Don't use too many basic types in a class.
  + Not all fields need indiviual field accessors and mutators.
  + Break up classes that have too many responsibilities.
  + Make the names of your classes and methods reflect their responsibilities.
  + Prefer immutable classes.
