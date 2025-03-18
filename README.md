# Object-Oriented Principles & Patterns

# 1. Understanding SOLID Principles

The SOLID principles are five design principles that help developers create more maintainable, scalable, and flexible software. These principles are essential for object-oriented programming and are widely used in C# development.

---

## **1. Single Responsibility Principle (SRP)**
**Definition**: A class should have only one reason to change, meaning it should have only one responsibility.

### **Example in C# (Bad Design)**
```csharp
public class Report
{
    public void GenerateReport()
    {
        Console.WriteLine("Generating Report...");
    }

    public void SaveToFile(string fileName)
    {
        Console.WriteLine($"Saving report to {fileName}");
    }
}
```
**Problem**: The `Report` class has two responsibilities:  
1. Generating the report.  
2. Saving the report to a file.  

### **Better Approach (SRP Applied)**
```csharp
public class ReportGenerator
{
    public string GenerateReport()
    {
        return "Report Data";
    }
}

public class ReportSaver
{
    public void SaveToFile(string fileName, string reportData)
    {
        Console.WriteLine($"Saving report to {fileName}");
    }
}
```
Now, `ReportGenerator` and `ReportSaver` handle their own responsibilities separately.

---

## **2. Open/Closed Principle (OCP)**
**Definition**: A class should be open for extension but closed for modification.

### **Example in C# (Bad Design)**
```csharp
public class AreaCalculator
{
    public double CalculateArea(object shape)
    {
        if (shape is Circle)
            return Math.PI * ((Circle)shape).Radius * ((Circle)shape).Radius;

        if (shape is Square)
            return ((Square)shape).Side * ((Square)shape).Side;

        return 0;
    }
}
```
**Problem**: Every time a new shape is added, we need to modify `CalculateArea`, violating OCP.

### **Better Approach (OCP Applied)**
```csharp
public interface IShape
{
    double CalculateArea();
}

public class Circle : IShape
{
    public double Radius { get; set; }
    public double CalculateArea() => Math.PI * Radius * Radius;
}

public class Square : IShape
{
    public double Side { get; set; }
    public double CalculateArea() => Side * Side;
}

public class AreaCalculator
{
    public double CalculateArea(IShape shape) => shape.CalculateArea();
}
```
Now, we can extend the system by adding new shapes without modifying `AreaCalculator`.

---

## **3. Liskov Substitution Principle (LSP)**
**Definition**: Subtypes should be replaceable by their base types without altering correctness.

### **Example in C# (Bad Design)**
```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
    public int GetArea() => Width * Height;
}

public class Square : Rectangle
{
    public override int Width 
    { 
        set { base.Width = base.Height = value; } 
    }

    public override int Height 
    { 
        set { base.Width = base.Height = value; } 
    }
}
```
**Problem**: If we assign a `Rectangle` object to a `Square`, it breaks expected behavior.

### **Better Approach (LSP Applied)**
```csharp
public interface IShape
{
    int GetArea();
}

public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }
    public int GetArea() => Width * Height;
}

public class Square : IShape
{
    public int Side { get; set; }
    public int GetArea() => Side * Side;
}
```
Now, both `Rectangle` and `Square` can be used interchangeably via `IShape`.

---

## **4. Interface Segregation Principle (ISP)**
**Definition**: A class should not be forced to implement interfaces it does not use.

### **Example in C# (Bad Design)**
```csharp
public interface IWorker
{
    void Work();
    void Eat();
}

public class Robot : IWorker
{
    public void Work() { Console.WriteLine("Robot working..."); }
    public void Eat() { throw new NotImplementedException(); }  // Not applicable to robots!
}
```
**Problem**: `Robot` does not need `Eat()` but is forced to implement it.

### **Better Approach (ISP Applied)**
```csharp
public interface IWorkable
{
    void Work();
}

public interface IEatable
{
    void Eat();
}

public class Human : IWorkable, IEatable
{
    public void Work() { Console.WriteLine("Human working..."); }
    public void Eat() { Console.WriteLine("Human eating..."); }
}

public class Robot : IWorkable
{
    public void Work() { Console.WriteLine("Robot working..."); }
}
```
Now, `Robot` implements only the `IWorkable` interface, following ISP.

---

## **5. Dependency Inversion Principle (DIP)**
**Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions.

### **Example in C# (Bad Design)**
```csharp
public class MySQLDatabase
{
    public void Save(string data) => Console.WriteLine("Saving data to MySQL");
}

public class DataManager
{
    private MySQLDatabase _database = new MySQLDatabase();
    
    public void SaveData(string data)
    {
        _database.Save(data);
    }
}
```
**Problem**: `DataManager` depends directly on `MySQLDatabase`, making it hard to switch databases.

### **Better Approach (DIP Applied)**
```csharp
public interface IDatabase
{
    void Save(string data);
}

public class MySQLDatabase : IDatabase
{
    public void Save(string data) => Console.WriteLine("Saving data to MySQL");
}

public class DataManager
{
    private readonly IDatabase _database;
    
    public DataManager(IDatabase database)
    {
        _database = database;
    }

    public void SaveData(string data)
    {
        _database.Save(data);
    }
}
```
Now, `DataManager` depends on `IDatabase`, allowing different implementations like SQL Server, MongoDB, etc.

---

## **Summary of SOLID Principles**
| Principle | Description | Benefit |
|-----------|------------|---------|
| **S**ingle Responsibility | A class should have only one reason to change | Improves maintainability |
| **O**pen/Closed | Classes should be open for extension, but closed for modification | Increases flexibility |
| **L**iskov Substitution | Subtypes must be replaceable by their base types | Prevents unexpected behaviors |
| **I**nterface Segregation | Don't force classes to implement unused methods | Increases code clarity |
| **D**ependency Inversion | Depend on abstractions, not concretions | Improves scalability |

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
