# Object-Oriented Principles & Patterns

## 1. Understanding SOLID Principles

SOLID is a set of five principles that help create maintainable and scalable object-oriented systems.

### S - Single Responsibility Principle (SRP)

- A class should have only one reason to change.
- Each class should have only one responsibility.

**Example:**
Instead of a single `Report` class handling data storage, formatting, and printing, split these into separate classes.

**Bad Design (Violating SRP):**
```java
class Report {
    void saveToDatabase() { /* Save logic */ }
    void formatReport() { /* Formatting logic */ }
    void printReport() { /* Printing logic */ }
}
```

**Good Design (Following SRP):**
```java
class ReportFormatter { void formatReport() { /* Formatting logic */ } }
class ReportPrinter { void printReport() { /* Printing logic */ } }
class ReportRepository { void saveToDatabase() { /* Save logic */ } }
```

### O - Open/Closed Principle (OCP)

- A class should be open for extension but closed for modification.
- Instead of modifying existing code, we should extend it using inheritance or polymorphism.

**Bad Design:**
```java
class Shape {
    String type;
}
class AreaCalculator {
    double calculateArea(Shape shape) {
        if (shape.type.equals("Circle")) { /* Logic */ }
        else if (shape.type.equals("Rectangle")) { /* Logic */ }
    }
}
```

**Good Design (Following OCP using Polymorphism):**
```java
abstract class Shape { abstract double calculateArea(); }
class Circle extends Shape { double calculateArea() { return /* Area Logic */; } }
class Rectangle extends Shape { double calculateArea() { return /* Area Logic */; } }
```

### L - Liskov Substitution Principle (LSP)

- Subtypes should be replaceable for their base types without breaking the system.
- A subclass should extend behavior without altering the parent’s expected behavior.

**Bad Design (Violating LSP):**
```java
class Rectangle { void setWidth(int width); void setHeight(int height); }
class Square extends Rectangle { 
    void setWidth(int width) { this.height = width; this.width = width; }  // Unexpected behavior
}
```

**Good Design (Following LSP):**
```java
abstract class Shape { abstract void setSize(int size); }
class Rectangle extends Shape { void setSize(int size) { /* Logic */ } }
class Square extends Shape { void setSize(int size) { /* Logic */ } }
```

### I - Interface Segregation Principle (ISP)

- Avoid large interfaces that force classes to implement unnecessary methods.
- Instead, use multiple smaller interfaces.

**Bad Design (Violating ISP):**
```java
interface Worker {
    void work();
    void eat();
}
class Robot implements Worker { 
    void work() { /* OK */ }  
    void eat() { /* Robots don't eat! */ } 
}
```

**Good Design (Following ISP):**
```java
interface Workable { void work(); }
interface Eatable { void eat(); }
class Human implements Workable, Eatable { /* Implements both */ }
class Robot implements Workable { /* Only work() */ }
```

### D - Dependency Inversion Principle (DIP)

- High-level modules should not depend on low-level modules. Both should depend on abstractions.
- Use Dependency Injection (DI) to avoid direct dependencies.

**Bad Design (Violating DIP):**
```java
class Keyboard { /* Keyboard-specific code */ }
class Computer {
    private Keyboard keyboard = new Keyboard(); // Hard dependency
}
```

**Good Design (Following DIP using Abstraction & DI):**
```java
interface InputDevice { /* Abstract */ }
class Keyboard implements InputDevice { /* Implementation */ }
class Computer {
    private InputDevice inputDevice;
    Computer(InputDevice device) { this.inputDevice = device; } // Dependency Injection
}
```

## 2. Understanding Other 5 Important Principles

These additional principles further enhance OOP design:

### 1. DRY (Don't Repeat Yourself)

- Avoid code duplication by using functions, inheritance, or reusable components.

**Bad:**
```java
class Dog { void bark() { System.out.println("Bark"); } }
class Cat { void meow() { System.out.println("Meow"); } }
```

**Good (Avoid Repetition Using Inheritance):**
```java
class Animal { void makeSound() { /* Common Logic */ } }
class Dog extends Animal { void makeSound() { System.out.println("Bark"); } }
class Cat extends Animal { void makeSound() { System.out.println("Meow"); } }
```

### 2. KISS (Keep It Simple, Stupid)

- Avoid complex designs when a simpler solution exists.

**Bad (Overcomplicated Logic):**
```java
if (x > 10) { if (x < 20) { System.out.println("Valid"); } }
```

**Good (Simplified Logic):**
```java
if (x > 10 && x < 20) System.out.println("Valid");
```

### 3. YAGNI (You Ain't Gonna Need It)

- Don't add features until they're necessary.

### 4. Law of Demeter (LoD)

- An object should not "talk" to internal objects deeply (`objA.getB().getC().doSomething()` is bad).

### 5. Composition Over Inheritance

- Favor composition (has-a relationship) over inheritance (is-a).

## 3. Understanding Creational Design Patterns

# Creational Design Patterns

Creational design patterns are used in software development to handle object creation mechanisms in a flexible and reusable way. Instead of instantiating objects directly using the `new` keyword, these patterns provide ways to create objects while keeping the code loosely coupled.

## Types of Creational Design Patterns

- Singleton Pattern
- Factory Method Pattern
- Abstract Factory Pattern
- Builder Pattern
- Prototype Pattern

### 1. Singleton Pattern

Ensures that a class has only one instance and provides a global point of access to it.

**When to Use?**

- When exactly one instance of a class is required (e.g., database connection, logging, configuration settings).

**Implementation (C#)**

```csharp
public class Singleton
{
    private static Singleton instance = null;
    private static readonly object padlock = new object();

    Singleton() { }

    public static Singleton Instance
    {
        get
        {
            lock (padlock)
            {
                if (instance == null)
                {
                    instance = new Singleton();
                }
                return instance;
            }
        }
    }
}
```

**Key Points**

- ✅ Private constructor to restrict direct object creation
- ✅ A static method to provide the single instance
- ✅ Ensures only one instance exists throughout the program

### 2. Factory Method Pattern

Defines an interface for creating an object, but lets subclasses decide which class to instantiate.

**When to Use?**

- When the object creation logic is complex and should not be exposed.
- When different types of objects need to be created dynamically.

**Implementation (C#)**

```csharp
public abstract class Product
{
    public abstract void Use();
}

public class ConcreteProductA : Product
{
    public override void Use()
    {
        Console.WriteLine("Using Product A");
    }
}

public class ConcreteProductB : Product
{
    public override void Use()
    {
        Console.WriteLine("Using Product B");
    }
}

public class Factory
{
    public static Product CreateProduct(string type)
    {
        if (type == "A")
        {
            return new ConcreteProductA();
        }
        else if (type == "B")
        {
            return new ConcreteProductB();
        }
        else
        {
            throw new ArgumentException("Invalid type");
        }
    }
}
```

**Key Points**

- ✅ Abstract class defines the interface
- ✅ Concrete classes implement the interface
- ✅ Factory method decides which object to create

### 3. Abstract Factory Pattern

Provides an interface for creating families of related objects without specifying their concrete classes.

**When to Use?**

- When multiple related objects need to be created without specifying their exact class.
- When different groups of objects need to be instantiated based on configuration.

**Implementation (C#)**

```csharp
// Abstract Factory
public interface IGUIFactory
{
    IButton CreateButton();
    ICheckbox CreateCheckbox();
}

// Concrete Factories
public class WindowsFactory : IGUIFactory
{
    public IButton CreateButton()
    {
        return new WindowsButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new WindowsCheckbox();
    }
}

public class MacOSFactory : IGUIFactory
{
    public IButton CreateButton()
    {
        return new MacButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new MacCheckbox();
    }
}

// Products
public interface IButton
{
    void Render();
}

public interface ICheckbox
{
    void Check();
}

public class WindowsButton : IButton
{
    public void Render()
    {
        Console.WriteLine("Rendering Windows Button");
    }
}

public class WindowsCheckbox : ICheckbox
{
    public void Check()
    {
        Console.WriteLine("Checking Windows Checkbox");
    }
}

public class MacButton : IButton
{
    public void Render()
    {
        Console.WriteLine("Rendering Mac Button");
    }
}

public class MacCheckbox : ICheckbox
{
    public void Check()
    {
        Console.WriteLine("Checking Mac Checkbox");
    }
}

// Client Code
public class Application
{
    private readonly IGUIFactory factory;

    public Application(IGUIFactory factory)
    {
        this.factory = factory;
    }

    public void CreateUI()
    {
        var button = factory.CreateButton();
        var checkbox = factory.CreateCheckbox();
        button.Render();
        checkbox.Check();
    }
}
```

**Key Points**

- ✅ Defines an interface for creating related objects
- ✅ Concrete factories implement the interface
- ✅ Ensures related objects are used together

### 4. Builder Pattern

Separates object construction from its representation, allowing the same construction process to create different representations.

**When to Use?**

- When an object has many optional parameters.
- When constructing an object step-by-step is necessary.

**Implementation (C#)**

```csharp
public class Product
{
    public string PartA { get; set; }
    public string PartB { get; set; }

    public void Show()
    {
        Console.WriteLine($"Product with: {PartA}, {PartB}");
    }
}

public interface IBuilder
{
    void SetPartA(string value);
    void SetPartB(string value);
    Product GetResult();
}

public class ConcreteBuilder : IBuilder
{
    private Product product = new Product();

    public void SetPartA(string value)
    {
        product.PartA = value;
    }

    public void SetPartB(string value)
    {
        product.PartB = value;
    }

    public Product GetResult()
    {
        return product;
    }
}

public class Director
{
    private readonly IBuilder builder;

    public Director(IBuilder builder)
    {
        this.builder = builder;
    }

    public Product Construct()
    {
        builder.SetPartA("Engine");
        builder.SetPartB("Wheels");
        return builder.GetResult();
    }
}
```

**Key Points**

- ✅ Separates complex object construction from the representation
- ✅ Ensures step-by-step creation of objects
- ✅ Provides different configurations of objects

### 5. Prototype Pattern

Creates new objects by copying an existing object (cloning) instead of creating from scratch.

**When to Use?**

- When object creation is expensive (e.g., deep-copying, cloning).
- When you need exact copies of existing objects.

**Implementation (C#)**

```csharp
public class Prototype
{
    public string Data { get; set; }

    public Prototype Clone()
    {
        return (Prototype) this.MemberwiseClone();
    }
}

// Client Code
var original = new Prototype { Data = "Hello" };
var copy = original.Clone();
Console.WriteLine(copy.Data); // Output: Hello
```

**Key Points**

- ✅ Reduces object creation cost
- ✅ Supports cloning instead of new instantiation
- ✅ Ensures same structure with different instances

### Summary Table

| Pattern         | Purpose                             |
| --------------- | ----------------------------------- |
| Singleton       | Ensures only one instance exists    |
| Factory Method  | Lets subclasses decide object creation |
| Abstract Factory| Creates families of related objects |
| Builder         | Constructs complex objects step-by-step |
| Prototype       | Creates clones of objects instead of new instances |

## 4. UML Diagrams (Use Case, Class, and Sequence Diagrams)

### 1. Use Case Diagram

- Shows interactions between actors (users) and the system.

```
[User] ---> (Login)
[User] ---> (Search Item)
[User] ---> (Checkout)
```

### 2. Class Diagram

- Shows relationships between classes.

```java
class Animal {
+name: String
+makeSound(): void
}
class Dog extends Animal {
+makeSound(): void
}
```

### 3. Sequence Diagram

- Shows interaction between objects over time.

```
User --> LoginHandler: Enter credentials
LoginHandler --> Database: Validate user
Database --> LoginHandler: Return success
LoginHandler --> User: Login success
```
