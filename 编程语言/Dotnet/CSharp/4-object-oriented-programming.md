# Object-oriented programming

- [Object-oriented programming](#object-oriented-programming)
  - [Encapsulation](#encapsulation)
    - [Members](#members)
      - [The difference between fields and properties](#the-difference-between-fields-and-properties)
    - [Accessibility](#accessibility)
  - [Inheritance](#inheritance)
  - [Polymorphism](#polymorphism)

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

Classes may be declared as `abstract`, which means that one or more of their methods have no implementation. Although abstract classes cannot be instantiated directly, they can serve as base classes for other classes that provide the missing implementation.

Classes can also be declared as `sealed` to prevent other classes from inheriting from them.

C# does not support multiple inheritance. However, you can use interfaces to implement multiple inheritance.

## Polymorphism
