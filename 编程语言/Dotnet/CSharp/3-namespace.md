# Namespace

- [Namespace](#namespace)
  - [Namespace overview](#namespace-overview)
  - [Declare namespaces to organize types](#declare-namespaces-to-organize-types)
  - [Nested Namespaces](#nested-namespaces)

## Namespace overview

Namespaces have the following properties:

- They organize large code projects.
- They're delimited by using the `.` operator.
- The `using` directive obviates the requirement to specify the name of the namespace for every class.
- The `global` namespace is the "root" namespace: `global::System` will always refer to the .NET [System](https://learn.microsoft.com/en-us/dotnet/api/system) namespace.

## Declare namespaces to organize types

Namespaces are heavily used in C# programming in two ways.

First, .NET uses namespaces to organize its many classes, as follows:

```C#
System.Console.WriteLine("Hello World!");
```

System is a namespace and Console is a class in that namespace.

The `using` keyword can be used so that the complete name isn't required, as in the following example:

```C#
using System;

Console.WriteLine("Hello World!");
```

Second, declaring your own namespaces can help you control the scope of class and method names in larger programming projects. Use the `namespace` keyword to declare a namespace, as in the following example:

```C#
namespace SampleNamespace
{
    class SampleClass
    {
        public void SampleMethod()
        {
            System.Console.WriteLine(
                "SampleMethod inside SampleNamespace");
        }
    }
}
```

Beginning with C# 10, you can declare a namespace for all types defined in that file, as shown in the following example:

```C#
namespace SampleNamespace;

class AnotherSampleClass
{
    public void AnotherSampleMethod()
    {
        System.Console.WriteLine(
            "SampleMethod inside SampleNamespace");
    }
}
```

The advantage of this new syntax is that it's simpler, saving horizontal space and braces. That makes your code easier to read.

## Nested Namespaces

Namespaces can be nested. You can define another namespace within a namespace. And also can use the `.` operator to access members of a nested namespace, as follows:

```C#
using MyNameSpace;
using MyNameSpace.Nested;

NestedNameSpaceClass.SayHello();
MyClass.SayHello();

namespace MyNameSpace
{
    public class MyClass
    {
        public static void SayHello()
        {
            Console.WriteLine("In SomeNameSpace");
            Nested.NestedNameSpaceClass.SayHello();
        }
    }

    namespace Nested
    {
        public class NestedNameSpaceClass
        {
            public static void SayHello()
            {
                Console.WriteLine("In Nested");
            }
        }
    }
}
```

The output is:

```log
In Nested
In SomeNameSpace
In Nested
```
