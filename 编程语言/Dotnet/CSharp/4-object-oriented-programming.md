# Object-oriented programming

- [Object-oriented programming](#object-oriented-programming)
  - [Encapsulation](#encapsulation)
    - [Members](#members)
      - [The difference between fields and properties](#the-difference-between-fields-and-properties)
    - [Accessibility](#accessibility)
  - [Inheritance](#inheritance)
    - [sealed](#sealed)
    - [Multiple inheritance](#multiple-inheritance)
  - [Polymorphism](#polymorphism)
    - [abstract](#abstract)
    - [virtual](#virtual)
    - [Hide base class members with new members](#hide-base-class-members-with-new-members)
    - [Access base class virtual members from derived classes](#access-base-class-virtual-members-from-derived-classes)

## Encapsulation

### Members

In C#, there are no global variables or methods as there are in some other languages.

The following list includes all the various kinds of members that may be declared in a `class`, `struct`, or `record`:

- Fields
- Constants
- Properties
- Methods
- Constructors
- Events
- Finalizers
- Indexers
- Operators
- Nested Types

#### The difference between fields and properties

a `field` is a variable (that can be of any type) that is defined inside a class.

a `property` is a member of the class that provides an abstraction to set (write) and get (read) the value of a private field.

```C#
public class Person{
  private int age; // Field "age" declared

  public int Age{ // Property for age used inside the class

    get{        // getter to get the person's age
      return age;
    }

    set{        // setter to set the person's age
      age = value;
    }
  }
}
```

### Accessibility

keyword:

- `public`
- `protected`
- `internal`
- `private`
- `file`

accessibility levels:

| accessibility levels | comment                                                                                                                   |
| :------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| `public`             | Access isn't restricted.                                                                                                  |
| `protected`          | Access is limited to the containing class or types derived from the containing class.                                     |
| `internal`           | Access is limited to the current assembly.                                                                                |
| `protected internal` | Access is limited to the current assembly or types derived from the containing class.                                     |
| `private`            | Access is limited to the containing type.                                                                                 |
| `private protected`  | Access is limited to the containing class or types derived from the containing class within the current assembly.         |
| `file`               | The declared type is only visible in the current source file. File scoped types are generally used for source generators. |

The default accessibility is `private`.

## Inheritance

Classes (but not structs) support the concept of inheritance. A class that derives from another class, called the base class, automatically contains all the public, protected, and internal members of the base class except its constructors and finalizers.

### sealed

Classes can also be declared as `sealed` to prevent other classes from inheriting from them.

In the following example, class B inherits from class A, but no class can inherit from class B.

```C#
class A {}
sealed class B : A {}
```

You can also use the `sealed` modifier on a method or property that overrides a virtual method or property in a base class. This enables you to allow classes to derive from your class and prevent them from overriding specific virtual methods or properties.

In the following example, Z inherits from Y but Z cannot override the virtual function F that is declared in X and sealed in Y.

```C#
class X
{
    protected virtual void F() { Console.WriteLine("X.F"); }
    protected virtual void F2() { Console.WriteLine("X.F2"); }
}

class Y : X
{
    sealed protected override void F() { Console.WriteLine("Y.F"); }
    protected override void F2() { Console.WriteLine("Y.F2"); }
}

class Z : Y
{
    // Attempting to override F causes compiler error CS0239.
    // protected override void F() { Console.WriteLine("Z.F"); }

    // Overriding F2 is allowed.
    protected override void F2() { Console.WriteLine("Z.F2"); }
}
```

### Multiple inheritance

C# does not support multiple inheritance. However, you can use interfaces to implement multiple inheritance.

## Polymorphism

### abstract

The `abstract` modifier indicates that the thing being modified has a missing or incomplete implementation.

The `abstract` modifier can be used with:

- classes
- methods
- properties
- indexers
- events

Use the abstract modifier in a class declaration to indicate that a class is intended only to be a base class of other classes, not instantiated on its own.

Members marked as abstract must be implemented by non-abstract classes that derive from the abstract class.

```C#
abstract class Shape
{
    public abstract int GetArea();
}

class Square : Shape
{
    private int _side;

    public Square(int n) => _side = n;

    // GetArea method is required to avoid a compile-time error.
    public override int GetArea() => _side * _side;

    static void Main()
    {
        var sq = new Square(12);
        Console.WriteLine($"Area of the square = {sq.GetArea()}");
    }
}
```

> The `override` modifier is required to extend or modify the `abstract` or `virtual` implementation of an inherited method, property, indexer, or event.

### virtual

The implementation of a `virtual` member can be changed by an `override` member in a derived class.

```C#
using App;

double r = 3.0, h = 5.0;
Shape c = new Circle(r);
Shape s = new Sphere(r);
Shape l = new Cylinder(r, h);
// Display results.
Console.WriteLine("Area of Circle   = {0:F2}", c.Area());
Console.WriteLine("Area of Sphere   = {0:F2}", s.Area());
Console.WriteLine("Area of Cylinder = {0:F2}", l.Area());

namespace App
{
    public class Shape
    {
        public const double Pi = Math.PI;
        protected double X, Y;

        public Shape(double x, double y)
        {
            X = x;
            Y = y;
        }

        public virtual double Area()
        {
            return X * Y;
        }
    }

    public class Circle : Shape
    {
        // Notice that the inherited classes Circle, Sphere, and Cylinder all use constructors that initialize the base class
        public Circle(double r) : base(r, 0) { }

        public override double Area()
        {
            return Pi * X * Y;
        }
    }

    public class Sphere : Shape
    {
        public Sphere(double r) : base(r, 0) { }

        public override double Area()
        {
            return 4 * Pi * X * X;
        }
    }

    public class Cylinder : Shape
    {
        public Cylinder(double r, double h) : base(r, h) { }

        public override double Area()
        {
            return 2 * Pi * X * X + 2 * Pi * X * Y;
        }
    }
}
```

The output is:

```log
Area of Circle   = 0.00
Area of Sphere   = 113.10
Area of Cylinder = 150.80
```

### Hide base class members with new members

If you want your derived class to have a member with the same name as a member in a base class, you can use the `new` keyword to hide the base class member. The `new` keyword is put before the return type of a class member that is being replaced. The following code provides an example:

```C#
public class BaseClass
{
    public void DoWork() { WorkField++; }
    public int WorkField;
    public int WorkProperty
    {
        get { return 0; }
    }
}

public class DerivedClass : BaseClass
{
    public new void DoWork() { WorkField++; }
    public new int WorkField;
    public new int WorkProperty
    {
        get { return 0; }
    }
}
```

Hidden base class members may be accessed from client code by casting the instance of the derived class to an instance of the base class. For example:

```C#
DerivedClass B = new DerivedClass();
B.DoWork();  // Calls the new method.

BaseClass A = (BaseClass)B;
A.DoWork();  // Calls the old method.
```

### Access base class virtual members from derived classes

A derived class that has replaced or overridden a method or property can still access the method or property on the base class using the `base` keyword.

The `base` keyword is used to access members of the base class from within a derived class. Use it if you want to:

- Call a method on the base class that has been overridden by another method.
- Specify which base-class constructor should be called when creating instances of the derived class.

The following code provides an example:

```C#
public class Base
{
    public Shape(double x, double y)
    {
        X = x;
        Y = y;
    }

    public virtual void DoWork() {/*...*/ }
}
public class Derived : Base
{
    public Derived(double x, double y) : base(x, y) { }

    public override void DoWork()
    {
        //Perform Derived's work here
        //...
        // Call DoWork on base class
        base.DoWork();
    }
}
```
