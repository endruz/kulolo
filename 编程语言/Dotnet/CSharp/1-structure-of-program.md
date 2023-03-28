# Structure of Program

- [Structure of Program](#structure-of-program)
  - [top-level statements](#top-level-statements)
    - [Only one top-level file](#only-one-top-level-file)
    - [No other entry points](#no-other-entry-points)
    - [using directives](#using-directives)
    - [Global namespace](#global-namespace)
    - [Namespaces and type definitions](#namespaces-and-type-definitions)
    - [args](#args)
    - [await](#await)
    - [Exit code for the process](#exit-code-for-the-process)

C# programs consist of one or more files.

Each file contains zero or more namespaces.

A namespace contains types such as classes, structs, interfaces, enumerations, and delegates, or other namespaces.

```C#
using System;

// Your program starts here:
Console.WriteLine("Hello world!");

namespace YourNamespace
{
    class YourClass
    {
    }

    struct YourStruct
    {
    }

    interface IYourInterface
    {
    }

    delegate int YourDelegate();

    enum YourEnum
    {
    }

    namespace YourNestedNamespace
    {
        struct YourStruct
        {
        }
    }
}
```

The preceding example uses *[top-level statements](#top-level-statements)* for the program's entry point. This feature was added in C# 9.

Prior to C# 9, the entry point was a static method named `Main`, as shown in the following example:

```C#
using System;
namespace YourNamespace
{
    class YourClass
    {
    }

    struct YourStruct
    {
    }

    interface IYourInterface
    {
    }

    delegate int YourDelegate();

    enum YourEnum
    {
    }

    namespace YourNestedNamespace
    {
        struct YourStruct
        {
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            //Your program starts here...
            Console.WriteLine("Hello world!");
        }
    }
}
```
## top-level statements

### Only one top-level file

An application must have only one entry point.

A project can have only one file with top-level statements.

Putting top-level statements in more than one file in a project results in the following compiler error:

> CS8802 Only one compilation unit can have top-level statements.

### No other entry points

You can write a Main method explicitly, but it can't function as an entry point. The compiler issues the following warning:

> CS7022 The entry point of the program is global code; ignoring 'Main()' entry point.

### using directives

If you include using directives, they must come first in the file, as in this example:

```C#
using System.Text;

StringBuilder builder = new();
builder.AppendLine("Hello");
builder.AppendLine("World!");

Console.WriteLine(builder.ToString());
```

### Global namespace

Top-level statements are implicitly in the global namespace.

### Namespaces and type definitions

A file with top-level statements can also contain namespaces and type definitions, but they must come after the top-level statements. For example:

```C#
MyClass.TestMethod();
MyNamespace.MyClass.MyMethod();

public class MyClass
{
    public static void TestMethod()
    {
        Console.WriteLine("Hello World!");
    }

}

namespace MyNamespace
{
    class MyClass
    {
        public static void MyMethod()
        {
            Console.WriteLine("Hello World from MyNamespace.MyClass.MyMethod!");
        }
    }
}
```

### args
Top-level statements can reference the args variable to access any command-line arguments that were entered.

The args variable is never null but its Length is zero if no command-line arguments were provided. For example:

```C#
if (args.Length > 0)
{
    foreach (var arg in args)
    {
        Console.WriteLine($"Argument={arg}");
    }
}
else
{
    Console.WriteLine("No arguments");
}
```

### await

You can call an async method by using await. For example:

```C#
Console.Write("Hello ");
await Task.Delay(5000);
Console.WriteLine("World!");
```

### Exit code for the process

To return an int value when the application ends.

the code returns zero, the batch file will report success.

However, if you return a non-zero value, the batch file will report failure.

For example:

```C#
string? s = Console.ReadLine();

int returnValue = int.Parse(s ?? "-1");
return returnValue;
```
