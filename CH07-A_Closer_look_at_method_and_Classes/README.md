# A Closer Look at Methods and Classes

## Introducing Access Control

## public
	When a member of a class is modified by public, then that member can be accessed by any other code. When a member of a class is specified as private, then that member can only be accessed by other members of its class. Now you can understand why main( ) has always been preceded by the public modifier. It is called by code that is outside the program—that is, by the Java run-time system. 
	When no access modifier is used, then by default the member of a class is public within its own package, but cannot be accessed outside of its package. (Packages are discussed in the following chapter.)

## Understanding static

Single copy of variable.

Methods declared as static have several restrictions:

* They can only directly call other static methods.
* They can only directly access static data.
* They cannot refer to this or super in any way. (The keyword super relates to inheritance and is described in the next chapter.)

If you need to do computation in order to initialize your static variables, you can declare a static block that gets executed exactly once, when the class is first loaded. The following example shows a class that has a static method, some static variables, and a static initialization block:


```
class UseStatic{
	static int a= 3;
	static int b;

	static void main(int x){
		---
	}

	static{
		b=a*4
	}


}
```
```
As soon as the UseStatic class is loaded, all of the static statements are run. First, a is set to 3, then the static block executes, which prints a message and then initializes b to a*4 or 12. Then main( ) is called, which calls meth( ), passing 42 to x. The three println( ) statements refer to the two static variables a and b, as well as to the local variable x.
```

Outside of the class in which they are defined, static methods and variables can be used independently of any object. To do so, you need only specify the name of their class followed by the dot operator. For example, if you wish to call a static method from outside its class, you can do so using the following general form:
classname.method( )



## Introducing final
In addition to fields, both method parameters and local variables can be declared final. Declaring a parameter final prevents it from being changed within the method. Declaring a local variable final prevents it from being assigned a value more than once.

## Introducing Nested and Inner Classes

* It is possible to define a class within another class; such classes are known as nested classes. The scope of a nested class is bounded by the scope of its enclosing class. Thus, if class B is defined within class A, then B does not exist independently of A. A nested class has access to the members, including private members, of the class in which it is nested. However, the enclosing class does not have access to the members of the nested class. A nested class that is declared directly within its enclosing class scope is a member of its enclosing class. It is also possible to declare a nested class that is local to a block.
* There are two types of nested classes: static and non-static.
* A static nested class is one that has the static modifier applied. Because it is static, it must access the non-static members of its enclosing class through an object. That is, it cannot refer to non-static members of its enclosing class directly. Because of this restriction, static nested classes are seldom used.
* The most important type of nested class is the inner class. An inner class is a non-static nested class. It has access to all of the variables and methods of its outer class and may refer to them directly in the same way that other non-static members of the outer class do.

* It is important to realize that an instance of Inner can be created only in the context of class Outer. The Java compiler generates an error message otherwise. In general, an inner class instance is often created by code within its enclosing scope, as the example does.




* While nested classes are not applicable to all situations, they are particularly helpful when handling events.


## Varargs: Variable-Length Arguments
Situations that require that a variable number of arguments be passed to a method are not unusual. For example, a method that opens an Internet connection might take a user name, password, filename, protocol, and so on, but supply defaults if some of this information is not provided. In this situation, it would be convenient to pass only the arguments to which the defaults did not apply. Another example is the printf( ) method that is part of Java’s I/O library. As you will see in Chapter 20, it takes a variable number of arguments, which it formats and then outputs.

Prior to JDK 5, variable-length arguments could be handled two ways, neither of which was particularly pleasing. First, if the maximum number of arguments was small and known, then you could create overloaded versions of the method, one for each way the method could be called. Although this works and is suitable for some cases, it applies to only a narrow class of situations.

In cases where the maximum number of potential arguments was larger, or unknowable, a second approach was used in which the arguments were put into an array, and then the array was passed to the method. This approach is illustrated by the following program:


```
class PassArray { 
  static void vaTest(int v[]) { 
    System.out.print("Number of args: " + v.length + 
                       " Contents: "); 
 
    for(int x : v) 
      System.out.print(x + " "); 
 
    System.out.println(); 
  } 
 
  public static void main(String args[])  
  { 
    // Notice how an array must be created to 
    // hold the arguments. 
    int n1[] = { 10 }; 
    int n2[] = { 1, 2, 3 }; 
    int n3[] = { }; 
 
    vaTest(n1); // 1 arg 
    vaTest(n2); // 3 args 
    vaTest(n3); // no args 
  } 
}

```



Same as above but with help of var args
```
class VarArgs { 
 
  // vaTest() now uses a vararg. 
  static void vaTest(int ... v) { 
    System.out.print("Number of args: " + v.length + 
                       " Contents: "); 
 
    for(int x : v) 
      System.out.print(x + " "); 
 
    System.out.println(); 
  } 
 
  public static void main(String args[])  
  { 
 
    // Notice how vaTest() can be called with a 
    // variable number of arguments. 
    vaTest(10);      // 1 arg 
    vaTest(1, 2, 3); // 3 args 
    vaTest();        // no args 
  } 
}


```


```
// Varargs and overloading. 
class VarArgs3 { 
 
  static void vaTest(int ... v) { 
    System.out.print("vaTest(int ...): " + 
                     "Number of args: " + v.length + 
                     " Contents: "); 
 
    for(int x : v) 
      System.out.print(x + " "); 
 
    System.out.println(); 
  } 
 
  static void vaTest(boolean ... v) { 
    System.out.print("vaTest(boolean ...) " + 
                     "Number of args: " + v.length + 
                     " Contents: "); 
 
    for(boolean x : v) 
      System.out.print(x + " "); 
 
    System.out.println(); 
  } 
 
  static void vaTest(String msg, int ... v) { 
    System.out.print("vaTest(String, int ...): " + 
                     msg + v.length + 
                     " Contents: "); 
 
    for(int x : v) 
      System.out.print(x + " "); 
 
    System.out.println(); 
  } 
 
  public static void main(String args[])  
  { 
    vaTest(1, 2, 3);  
    vaTest("Testing: ", 10, 20); 
    vaTest(true, false, false); 
  } 
}

```



















