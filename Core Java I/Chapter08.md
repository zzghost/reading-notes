# Generic Programming
wildcard type: Use <T> to decorate methods and class to implement Generic Programming.

Add BoundingType for T:  
use `<T extends BoundingType>` to let T have access to some interfaces. e.g.`T smallest = a[0]; if(smallest.comaresTo(a[1]) > 0)` Here, T should be restrict to the class which implements the Comparable interface. The author tells us that, although use *extends* instead of *implements*. The notation `<T extends BoundingType>` expresses that T should be a subtype of the BoundingType. Both T and the BoundingType can be either a class or an interface. The extends keyword was chosen because it is a reasonable approximation of the subtype concept, and the Java designers did not want to add a new keyword(like sub) to the language.  
 A type variable or wildcard can have multiple bounds.e.g. `<T extends Comparable & Serializable>`. As with Java inheritance, you can have as many interfaces supertypes as you like, but at most one of the bounds can be a class.  

How the compiler "erases" type parameters, and what implication that process has for Java programmers?  
Whenever you define a generic type, a corresponding raw type is automatically provided. The raw type replaces type variables with the first bound. if <T> is and unbounded type variable, it is simply replaced by *Object*.  
Method call: the compiler translates the method call into 2 virtual machine instructions:
  + A call to the raw method Pair.getFirst.
  + A cast of the returned Object to the type Employee.  

Now Consider this:
```java
//setSecond() is overrided with parameters list: (LocalDate second) and (Object second)
DateInterval interval = new DateInterval(..);
Pair<LocalDate> pair = interval;//assignment to superclass
pair.setSecond(aDate);
```
When meet Polymorphic methods in a class, the compiler generates a *bridge method* in the class.like this:
```java
  public void setSecond(Object second){
    setSecond((Date) second);
  }
```
when `pair.setSecond(aDate)` executes, pair is type Pair<LocalDate> object, that type has only single method setSecond(Object).virtual machine calls method on the object to which pair refers, it is DateInterval.Therefore, the method DateInterval.setSecond(Object) is c alled. That method is the synthesized bridge method. It calls DateInterval.setSecond(Date), which is what we want.  

Facts about translation of Java generics:
  + There are no generics in the virtual machine, only ordinary classes and methods.  
  + All type parameters are replaced by their bounds.  
  + Bridge methods are synthesized to preserve polymorphism.  
  + Casts are inserted as necessary to preserve type safety.  

Limitations and restrictions:
1. Type parameters cannot be instantiated with primitive types.
2. Runtime type inquiry only works with raw types.
3. You cannot create arrays of parameterized types.
  ```java
  Pair<String>[] table = new Pair<String>[10];//Error
  ```
4. Varargs warnings.
  although Java doesn't support create parameterized types arrays, passing instances of a generic type to a method with a variable number of arguments.  
  e.g.
  ```java
  public static <T> void addAll(Collection<T> coll, T..ts){
    for(t : ts)
      coll.add(t);
  }

  //Here is the call...
  Collection<Pair<String>> table = ...;
  Pair<String> pair1 = ..;
  Pair<String> pair2 = ..;
  addAll(table, pair1, pair2);
  ```
  This is surprisingly allowed, but it will generate a warning.  
  Avoid warning by adding @SuppressWarnings("unchecked") or annotating the addAll method itself with @SafeVarargs.  
5. You cannot instantiate type variables.  
  Type erasure would change T to Object.  
  ```java
  //Error
  public Pair(){
    first = new T();
    second = new T();
  }
  //Since Java 8:
  Pair<String> p = Pair.makePair(String::new);

  ```
  The makePair method receives a Supplier<T>, the functional interface for a function with no arguments and a result of type T:
  ```java
  public static <T> Pair<T> makePair(Supplier<T> constr){
    return new Pair<>(constr.get(), constr.get());
  }
  ```
6. You cannot construct a generic array.  
7. Type variables are not valid in static contexts of generic classes.  
8. You cannot throw or catch instances of a generic class.
9. You can defeat checked exception checking.
10. Beware of clashes after erasure.

There's no inheritance relationship between generic classes.`Pair<Manager> cannot be converted to Pair<Employee>`. But generic classes can extend or implement other generic classes.`ArrayList<Manager> can be converted to List<Manager>`

Pair<? extends Employee> denotes: the type parameter is a subclass of Employee.  
Pair<? super Manager> specifies a *supertyper bound*, it denotes all supertypes of Manager.  
Pair<?>: it's different from raw type Pair. It has methods like:  
+ ? getFirst()  
  the return value of getFirst() can only be assigned to an Object.
+ void setFirst(?)  
    the setFirst() can never be called, not even with an Object.
    you can call the setFirst method of the raw Pair class with any Object.
