# Learning Java in 5 Days

## Syntax and fundamentals

#### Data Types and Primitives

* Java is strongly typed
* There are 8 primitives in Java
  * Four `ints` - `int`, `short`, `long`, `byte`
  * Two `floats` - `float` and `double`
    * There is a `BigDecimal` class for more precise calculations
  * `char`
  * `boolean` 
    * cannot convert from `ints`. So `if x == 0` is not a thing in Java.
* You have to initialize a variable before using it, or it won't compile.
* Use the keyword `final` to denote a constant, as in `final double CM_PER_INCH = 2.54;`
  * More commonly this might be available to a whole class. Note that since this is `public`, other classes can use it as well.

```java
public class ConstantExample
{
	public static final double CM_PER_INCH = 2.54;
}
```

* Arithmetic operators `+-*/%`
* Relational operators `== != < > <= >=`
* Logical operators `&& || !`
* Ternary `condition ? expression1 : expression2`
* Casting `double x = 9.997; int nx = (int) x;` 
  * In this example `nx` will have the value `9`
  * Often more useful is to round to nearest with `Math.round()`
* Enumerated types `enum Size { SMALL, MEDIUM, LARGE };`
* Strings in Java are sequences of unicode characters. Java does not contain a built-in string type, instead we use the standard Java library `String` class
  * Strings are immutable. You have to assign that variable to a whole new string
  * Do not use `==` to compare if strings are equal

#### Input and Output

* Input is done with `Scanner`s 
  * To read console input, attach a `Scanner` to `System.in` - `Scanner in = new Scanner(System.in);`
  * To read from a file, `Scanner fin = new Scanner(new File("myfile.txt"));`
* Output is done with `System.out`
  * To output to console, `System.out.print(x);`
  * There's also `printf` for formatting: `System.out.printf("%8.2f" x);`
  * To write to file, `PrintWriter four = new PrintWriter("myfile.txt");`

#### Control Flow

* Like C++ we can use braces `{}` for block scope
* If statements `if (condition) block else block`
* Switch statements
  * case labels must be integers or enumerated constants. No strings.

```java
switch (choice)
{
	case 1:
		...
		break;
	case 2:
		...
		break;
	default:
		...
		break;
}
```

* While loops `while (condition) block`
* Do-While loops `do block while (condition)`
* For loops `for (int i = 1; i < 10; i++) block`
* For each `for (variable: collection) block`

#### Basic Data Structures

* Arrays
  * `int[] a = new int[100];`
  * `int[] smallPrimes = { 2, 3, 5, 7 };`
  * `students = new String[10];`
    * Notice that `new String[10]` makes an anonymous array and assigns it to `students`
  * `balances = new double[NYEARS][NRATES]`
  * No pointer arithmetic to get to elements
* ArrayLists
  * Basically an array that grows as you add to it. So no need to keep requesting memory.
  * `ArrayList<T> name = new ArrayList<T>();`
  * `ArrayList<T>(int inititalCapacity) name = ...`
  * Add things to it: `name.add(new T(...));`
  * Remove stuff: `name.remove(n);`
* 