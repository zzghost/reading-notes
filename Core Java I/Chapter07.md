# Exceptions, Assertions, and Logging
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
The code in the finally clause executeds whether or not an exception was caught.

assert condition;
assert condition : expression;
