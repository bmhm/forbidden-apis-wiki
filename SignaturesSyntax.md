# Syntax of Forbidden Signatures #

Forbidden API Checker allows to define custom signatures files. Depending on
the type of task (Ant, Maven, or Gradle), you can add them via references to
local files from your project directory. Maven also supports to refer to them using
Maven coordinates.

The syntax of those files is: Each line that is not empty, does not start with a
`#` or `@` symbol is treated as a signature. The following types
of signatures are supported:


  * *Class reference:* A binary/fully-qualified class name (including package). You may
    use the output of [`Class.getName()`](https://docs.oracle.com/javase/6/docs/api/java/lang/Class.html#getName()).
    Be sure to use correct name for inner
    classes! Example: `java.lang.String`
  * *A package/class glob pattern:* To forbid all classes from a package, you may use
    glob patterns, like `sun.misc.**` ("`**`" matches against package
    boundaries).
  * *A field of a class:* `package.Class#fieldName`
  * *A method signature:* It consists of a binary class name, followed by `#`
    and a method name including method parameters: `java.lang.String#concat(java.lang.String)` --
    All method parameters need to use fully qualified class names!

The error message displayed when the signature matches can be given at the end of each
signature line using "`@`" as separator:

```
  java.lang.String @ You are crazy that you disallow strings
```

To not repeat the same message after each signature, you can prepend signatures
with a default message. Use a line starting with "`@defaultMessage`".

```
  @defaultMessage You are crazy that you disallow substrings
  java.lang.String#substring(int)
  java.lang.String#substring(int,int)
```
