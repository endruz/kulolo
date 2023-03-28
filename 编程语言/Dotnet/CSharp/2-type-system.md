# Type system

- [Type system](#type-system)
  - [Value types](#value-types)
    - [struct](#struct)
    - [enum](#enum)
  - [Reference types](#reference-types)
  - [Variables \& Constants](#variables--constants)
  - [Types of literal values](#types-of-literal-values)
  - [Generic types](#generic-types)
  - [Nullable value types](#nullable-value-types)
  - [Compile-time type and run-time type](#compile-time-type-and-run-time-type)

C# is a strongly typed language. Every variable and constant has a type.

There are two kinds of types in C#: [value types]() and [reference types]():

- Variables of value types directly contain their data.
- Variables of reference types store references to their data (objects).

## Value types

There are two categories of `value types`: [struct](#struct) and [enum](#enum).

### struct

The built-in numeric types are structs, and they have fields and methods that you can access:

```C#
// constant field on type byte.
byte b = byte.MaxValue;
```

But you declare and assign values to them as if they're simple non-aggregate types:

```C#
byte num = 0xA;
int i = 5;
char c = 'Z';
```

You use the `struct` keyword to create your own custom value types. Typically, a struct is used as a container for a small set of related variables, as shown in the following example:

```C#
public struct Coords
{
    public int x, y;

    public Coords(int p1, int p2)
    {
        x = p1;
        y = p2;
    }
}
```

### enum

An `enum` defines a set of named integral constants. For example:

```C#
public enum FileMode
{
    CreateNew = 1,
    Create = 2,
    Open = 3,
    OpenOrCreate = 4,
    Truncate = 5,
    Append = 6,
}
```

## Reference types

A type that is defined as a `class`, `record`, `delegate`, `array`, or `interface` is a reference type.

When declaring a variable of a reference type, it contains the value `null` until you assign it with an instance of that type or create one using the `new` operator.

```c#
MyClass myClass = new MyClass();
MyClass myClass2 = myClass;
```

An interface cannot be directly instantiated using the `new` operator. Instead, create and assign an instance of a class that implements the interface. Consider the following example:

```C#
MyClass myClass = new MyClass();

// Declare and assign using an existing value.
IMyInterface myInterface = myClass;

// Or create and assign a value in a single statement.
IMyInterface myInterface2 = new MyClass();
```

All arrays are reference types, even if their elements are value types.

Arrays implicitly derive from the [System.Array](https://learn.microsoft.com/en-us/dotnet/api/system.array?view=net-7.0) class. You declare and use them with the simplified syntax that is provided by C#, as shown in the following example:

```C#
// Declare and initialize an array of integers.
int[] nums = { 1, 2, 3, 4, 5 };

// Access an instance property of System.Array.
int len = nums.Length;
```

## Variables & Constants

When you declare a variable or constant in a program, you must either specify its type or use the `var` keyword to let the compiler infer the type.

```C#
// Declaration only:
float temperature;
string name;
MyClass myClass;

// Declaration with initializers (four examples):
char firstLetter = 'C';
var limit = 3;
int[] source = { 0, 1, 2, 3, 4, 5 };
var query = from item in source
            where item <= limit
            select item;
```

Constants are immutable values which are known at compile time and do not change for the life of the program.

Constants are declared with the `const` keyword. Only the C# built-in types (excluding [System.Object](https://learn.microsoft.com/en-us/dotnet/api/system.object?view=net-7.0)) may be declared as `const`. User-defined types, including classes, structs, and arrays, cannot be `const`.

Constants must be initialized as they are declared. For example:

```C#
const int Months = 12;
const int Months = 12, Weeks = 52, Days = 365;
const double DaysPerWeek = (double) Days / (double) Weeks;
const double DaysPerMonth = (double) Days / (double) Months;
```

## Types of literal values

In C#, literal values receive a type from the compiler.

You can specify how a numeric literal should be typed by appending a letter to the end of the number.

For example, to specify that the value `4.56` should be treated as a `float`, append an "f" or "F" after the number: `4.56f`.

Because literals are typed, and all types derive ultimately from [System.Object](https://learn.microsoft.com/en-us/dotnet/api/system.object?view=net-7.0), you can write and compile code such as the following code:

```C#
string s = "The answer is " + 5.ToString();
// Outputs: "The answer is 5"
Console.WriteLine(s);

Type type = 12345.GetType();
// Outputs: "System.Int32"
Console.WriteLine(type);·
```

> Consider Chapter 2 of *C# Notes for Professionals*.

## Generic types

A type can be declared with one or more type parameters that serve as a placeholder for the actual type (the concrete type). Client code provides the concrete type when it creates an instance of the type. Such types are called generic types.

For example, the .NET type [System.Collections.Generic.List<T>](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-7.0) has one type parameter that by convention is given the name `T`. When you create an instance of the type, you specify the type of the objects that the list will contain, for example, `string`:

```C#
List<string> stringList = new List<string>();
stringList.Add("String example");
// compile time error adding a type other than a string:
stringList.Add(4);
```

## Nullable value types

Ordinary value types can't have a value of null. However, you can create nullable value types by appending a `?` after the type.

For example, `int?` is an `int` type that can also have the value `null`. Nullable value types are instances of the generic struct type [System.Nullable<T>](https://learn.microsoft.com/en-us/dotnet/api/system.nullable-1?view=net-7.0).

## Compile-time type and run-time type

A variable can have different compile-time and run-time types:

- The compile-time type is the declared or inferred type of the variable in the source code.
- The run-time type is the type of the instance referred to by that variable.

Often those two types are the same, as in the following example:

```C#
string message = "This is a string of characters";
```

In other cases, the compile-time type is different, as shown in the following two examples:

```C#
object anotherMessage = "This is another string of characters";
IEnumerable<char> someCharacters = "abcdefghijklmnopqrstuvwxyz";
```

In both of the preceding examples, the run-time type is a `string`. The compile-time type is `object` in the first line, and `IEnumerable<char>` in the second.
