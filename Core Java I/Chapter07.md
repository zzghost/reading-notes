# Exceptions, Assertions, and Logging
Java provides three mechanisms to deal with system failure:
  + Throwing an Exception  
  + Logging
  + Using Assertions  
Exception Hierarchy:
Throwable(Error, Exception(IOException, Runtime Exception))
Doing java Programming is focusing on the Exception Hierarchy.
Runtime Error: You made a programming error.  
  + A bad cast
  + An out-of-bound array access
  + A null pointer access
IOException: I/O error.

You can catch multiple exception types in the same catch clause.

finally clause:
The code in the finally clause executes whether or not an exception was caught.
Be careful that code block in finally clause executes before the method returns.  
e.g. As you can see below, if you call f(2), the try block computes r = 4, and executes the return statement.However, the finally clause is executed before the method actually returns and causes the method to return 0,ignoring the original return value of 4.  
```java
public static int f(int n){
  try{
    int r = n * n;
    return r;
  }
  finally{
    if(n == 2)
      return 0;
  }
}
```
*Stack trace* : a listing of all pending method calls at a particular point in the execution of a program.  
Access stack trace by calling the *printStackTrace* method of the Throwable class.  

Tips for Using Exceptions:
1. Exception handling is not supposed to replace a simple test.  
Use exceptions for exceptional circumstances only.  
2. Do not micromanage exceptions.  
3. Make good use of the exception Hierarchy.
Don't just throw a RuntimeException or catch Throwable. Please be more specific. Throw NumberFormatException and turn it into a subclass of IOException or MySubSystemException.  
4. Do not squelch exceptions.  
5. When you detect an error, "tough love" works better than indulgence.  
6. Propagating exceptions is not a sign of shame.  

Use Assertion for parameter check:  
Suppose: you do `double y = Math.sqrt(x)`.  Use `assert x >= 0;` instead of `if(x < 0) throw new IllegalArgumentException("x < 0")`
assert condition;
assert condition : expression;
