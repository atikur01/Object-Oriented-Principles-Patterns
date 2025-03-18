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

These patterns help create objects efficiently.

### 1. Singleton

- Ensures only one instance of a class exists.

```java
class Singleton {
    private static Singleton instance;
    private Singleton() {} // Private constructor
    public static Singleton getInstance() {
        if (instance == null) instance = new Singleton();
        return instance;
    }
}
```

### 2. Factory Method

- Creates objects without exposing the logic.

```java
interface Shape { void draw(); }
class Circle implements Shape { void draw() { System.out.println("Circle"); } }
class ShapeFactory { 
    static Shape getShape(String type) { 
        if (type.equals("Circle")) return new Circle(); 
        return null; 
    }
}
```

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
