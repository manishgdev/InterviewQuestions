## Java Concepts
### Q. What is the difference between == and equals() in Java?
 Answer:\
 == checks for reference equality (whether both references point to the same object), while equals() checks for value equality (whether the values of two objects are the same).
   
### Q. Explain the purpose and usage of the final keyword in Java.
Answer:
- **Final Class**: Prevents the class from being subclassed.
- **Final Method**: Prevents the method from being overridden.
- **Final Variable**: Makes the variable immutable (constant).

### Q. What are the main principles of Object-Oriented Programming (OOP)?
Answer: 
- **Encapsulation**: Hiding the internal state and requiring all interaction to be performed through an object's methods.
- **Inheritance**: Mechanism to create a new class using the properties and methods of an existing class.
- **Polymorphism**: Ability to process objects differently based on their data type or class.
- **Abstraction**: Hiding the complex implementation details and showing only the necessary features of an object.

### Q. What is method overloading and method overriding?
Answer: 

- Method Overloading: Multiple methods in the same class with the same name but different parameters.
- Method Overriding: A subclass provides a specific implementation for a method already defined in its superclass.

### Q. Explain the concept of polymorphism with an example.
Answer:\
Polymorphism allows objects to be treated as instances of their parent class rather than their actual class. 
Example:
```java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("Dog barks");
    }
}

public class TestPolymorphism {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound(); // Outputs: Dog barks
    }
}

```

## Concurrency
### Q. What is a deadlock and how can it be prevented?
Answer: \
A deadlock occurs when two or more threads are blocked forever, each waiting on the other. It can be prevented by:

- Avoiding nested locks.
- Using a timeout for lock attempts.
- Acquiring locks in a consistent order.
- Using a deadlock detection algorithm.

### Q. What is the volatile keyword in Java?
Answer: \
The volatile keyword ensures that a variable's updates are propagated predictably to other threads, avoiding caching issues.

### Q. What are the differences between Runnable and Callable?
Answer: 

- **Runnable**: Does not return a result and cannot throw checked exceptions.
- **Callable**: Returns a result and can throw checked exceptions.

### Q. Explain the concept of thread safety and how to achieve it in Java.
Answer:\
Thread safety ensures that code can be safely executed by multiple threads at the same time. It can be achieved by:

- Synchronization (using synchronized keyword).
- Using thread-safe classes (e.g., ConcurrentHashMap, CopyOnWriteArrayList).
- Avoiding shared mutable state.
- Using atomic classes from java.util.concurrent.atomic.

### Q. What is the `ExecutorService` and how is it used? 
Answer: \
ExecutorService is an interface for managing and controlling thread execution. It is used to execute, schedule, and manage asynchronous tasks.
```java
ExecutorService executorService = Executors.newFixedThreadPool(10);
executorService.submit(() -> {
    // Task logic here
});
executorService.shutdown();
```

## Java Collections
### Q. What are the differences between ArrayList and LinkedList?
Answer: 

- **ArrayList**: Backed by a dynamic array, offers fast random access but slow insertions/deletions in the middle.
- **LinkedList**: Doubly linked list, slower random access but faster insertions/deletions in the middle.

### Q. Explain the concept of a HashMap. How does it work internally?
Answer: \
HashMap stores key-value pairs and uses hashing for quick access. Internally, it uses an array of buckets and each bucket is a linked list (or tree in case of high collision). The key’s hashCode determines the bucket location.

### Q. What are ConcurrentHashMap advantages over HashMap?
Answer: \
**ConcurrentHashMap** is thread-safe and allows concurrent read/write operations. It uses lock striping to reduce contention, ensuring high performance in a multithreaded environment.

### Q. What is the difference between Set and List in Java Collections?
Answer: 

- **Set**: Does not allow duplicate elements and does not maintain any order.
- **List**: Allows duplicate elements and maintains insertion order.

### Q. How do you convert a List to a Set in Java?
Answer: 
```java
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
Set<String> set = new HashSet<>(list);

```
## Java Streams and Functional Programming
### Q. What are the key features of Java Streams API?
Answer: 

- Functional-style operations (map, filter, reduce).
- Lazy evaluation.
- Parallel processing.
- Can be infinite (e.g., Stream.generate()).

### Q. How do you convert a List to a Stream?
Answer: 

```java
List<String> list = Arrays.asList("A", "B", "C");
Stream<String> stream = list.stream();
```
**18. Explain the use of filter and map in Streams.**\
Answer:

- **filter**: Used to select elements based on a condition.
```
Stream<String> filteredStream = stream.filter(s -> s.startsWith("A"));
```
- **map**: Used to transform each element.
```java
Stream<Integer> lengthStream = stream.map(String::length);
```

### Q. What is a lambda expression and provide an example?
Answer: \
A lambda expression provides a concise way to represent a method using an expression.\
Example:

```java
Comparator<String> comparator = (s1, s2) -> s1.compareTo(s2);
```

### Q. What is a functional interface? Give an example.
Answer: \
A functional interface has a single abstract method but can have multiple default or static methods. Example:

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod();
}
```

## Design Patterns
### Q. Explain the Singleton pattern and how to implement it in Java.
Answer: \
The Singleton Pattern is a creational design pattern that ensures a class has only one instance and provides a global point of access to that instance. This pattern is particularly useful when exactly one object is needed to coordinate actions across the system. It ensures that the class itself controls the instantiation of its object, which helps in managing shared resources, configurations, or any other operations that need a single point of control.

**Key Concepts**
- **Single Instance**: Ensures that a class has only one instance.
- **Global Access Point**: Provides a global access point to that instance.
- **Lazy Initialization**: The instance is created only when it is needed for the first time.
- **Thread Safety**: Ensures that the instance is created safely when accessed by multiple threads.

**Structure**
- **Private Constructor**: Prevents the instantiation of the class from outside.
- **Static Variable**: Holds the single instance of the class.
- **Public Static Method**: Provides global access to the instance.

**Example**: Singleton Logger\
Consider a scenario where you want to create a logging utility that ensures only one instance is used throughout the application to handle logging.

**Step 1: Define the Singleton Class**
```java

public class Logger {
    // The single instance of the Logger
    private static Logger instance;

    // Private constructor to prevent instantiation
    private Logger() {
    }

    // Public method to provide access to the instance
    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger();
        }
        return instance;
    }

    // Example method to log a message
    public void log(String message) {
        System.out.println("Log message: " + message);
    }
}
```

**Step 2: Use the Singleton Class**
```java

public class SingletonPatternDemo {
    public static void main(String[] args) {
        // Get the single instance of Logger
        Logger logger = Logger.getInstance();

        // Use the logger to log messages
        logger.log("This is the first log message.");
        logger.log("This is the second log message.");
    }
}
```

**Thread-Safe Singleton**\
To ensure that the Singleton instance is created safely when accessed by multiple threads, you can use synchronized methods or other thread-safe mechanisms.

**Synchronized Method Example**
```java

public class ThreadSafeLogger {
    private static ThreadSafeLogger instance;

    private ThreadSafeLogger() {
    }

    public static synchronized ThreadSafeLogger getInstance() {
        if (instance == null) {
            instance = new ThreadSafeLogger();
        }
        return instance;
    }

    public void log(String message) {
        System.out.println("Log message: " + message);
    }
}
```
**Double-Checked Locking**\
Double-checked locking can be used to reduce the overhead of synchronized methods by only locking the critical section of the code.

```java

public class DoubleCheckedLockingLogger {
    private static volatile DoubleCheckedLockingLogger instance;

    private DoubleCheckedLockingLogger() {
    }

    public static DoubleCheckedLockingLogger getInstance() {
        if (instance == null) {
            synchronized (DoubleCheckedLockingLogger.class) {
                if (instance == null) {
                    instance = new DoubleCheckedLockingLogger();
                }
            }
        }
        return instance;
    }

    public void log(String message) {
        System.out.println("Log message: " + message);
    }
}
```
**Advantages of the Singleton Pattern**
- **Controlled Access to Single Instance**: Ensures controlled access to the sole instance.
- **Reduced Memory Footprint**: Prevents multiple instances, thus saving memory.
- **Global Point of Access**: Provides a global access point to the instance.
- **Consistency**: Ensures consistent behavior across the application.

**Disadvantages of the Singleton Pattern**
- **Global State**: Introduces a global state into the application, which can make it harder to test and debug.
- **Hidden Dependencies**: Can lead to hidden dependencies between classes.
- **Thread Safety Issues**: Requires careful implementation to ensure thread safety.
- **Difficult to Extend**: Hard to subclass or extend a singleton class because the constructor is private.

**Real-World Examples**
- **Configuration** Management: Managing configurations and settings.
- **Logging**: Handling logging throughout an application.
- **Cache**: Implementing a shared cache to store frequently accessed data.
- **Resource Management**: Managing shared resources like database connections or thread pools.

### Q. What is the Factory Method pattern?
Answer: \
The Factory Method Pattern is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. Instead of calling a constructor directly to create an object, the client calls a factory method, which is defined in the interface and implemented by the subclasses. This pattern is useful when the exact type of object to be created is determined by the subclasses.

**Key Concepts**
- **Product**: This is the interface or abstract class that defines the type of object that the factory method will create.
- **ConcreteProduct**: These are the concrete implementations of the Product interface.
- **Creator**: This is the abstract class that declares the factory method. This class may also provide some default implementation of the factory method.
- **ConcreteCreator**: These are the subclasses that implement the factory method to create instances of ConcreteProduct.

**Structure**
- **Product Interface**: Defines the interface of objects the factory method creates.
- **ConcreteProduct**: Implements the Product interface.
- **Creator** (Abstract Class): Declares the factory method which returns an object of type Product.
- **ConcreteCreator**: Implements the factory method to return an instance of a ConcreteProduct.

**Example**: Creating Different Types of Buttons
Let's consider an example where we have different types of buttons (e.g., WindowsButton and MacButton) and we want to create them using the Factory Method Pattern.

**Step 1: Define the Product Interface**
```java

public interface Button {
    void render();
    void onClick();
}
```
**Step 2: Implement Concrete Products**
```java

public class WindowsButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering a Windows button.");
    }

    @Override
    public void onClick() {
        System.out.println("Windows button clicked.");
    }
}

public class MacButton implements Button {
    @Override
    public void render() {
        System.out.println("Rendering a Mac button.");
    }

    @Override
    public void onClick() {
        System.out.println("Mac button clicked.");
    }
}
```
**Step 3: Create the Creator Abstract Class**
```java

public abstract class Dialog {
    public void renderWindow() {
        // Call the factory method to create a Button object
        Button okButton = createButton();
        okButton.render();
        okButton.onClick();
    }

    // Factory method
    protected abstract Button createButton();
}
```
**Step 4: Implement Concrete Creators**
```java

public class WindowsDialog extends Dialog {
    @Override
    protected Button createButton() {
        return new WindowsButton();
    }
}

public class MacDialog extends Dialog {
    @Override
    protected Button createButton() {
        return new MacButton();
    }
}
```
**Step 5: Demonstrate the Factory Method Pattern**
```java

public class FactoryMethodDemo {
    public static void main(String[] args) {
        Dialog dialog;

        // Configuration setting can be used to determine which dialog to use
        String osType = "Windows"; // This can be dynamically determined

        if (osType.equals("Windows")) {
            dialog = new WindowsDialog();
        } else {
            dialog = new MacDialog();
        }

        dialog.renderWindow();
    }
}
```
**Advantages of the Factory Method Pattern**
- **Single Responsibility Principle**: Factory methods eliminate the need to bind application-specific classes into your code. The code deals with interfaces, so it can work with any user-defined classes that implement these interfaces.
- **Open/Closed Principle**: The factory method allows the addition of new products without changing the creator's code.
- **Flexibility**: Factory methods provide flexibility in deciding the type of objects to create. This is useful when the exact type of object is not known until runtime.

**Disadvantages of the Factory Method Pattern**
- **Complexity**: The pattern introduces a level of abstraction that can make the code more complex and harder to understand.
- **Dependency on Subclasses**: Requires creating a new subclass for each type of product, which can lead to an increase in the number of classes.

**Real-World Examples**
- **Java Collections Framework**: Methods like java.util.Collections#synchronizedList and synchronizedMap use the factory method pattern.
- **GUI Toolkits**: Creating different UI components for different platforms (e.g., Windows, macOS).
- **Document Creation**: Different document editors (e.g., Word, Excel) using the same interface for creating different types of documents.

### Q. Explain the Decorator pattern with an example.
Answer: \
The Decorator Design Pattern is a structural pattern that allows behavior to be added to individual objects, dynamically, without affecting the behavior of other objects from the same class. This pattern is particularly useful for adhering to the Open/Closed Principle, as it allows you to extend the functionality of objects without modifying their code.

**Key Concepts**
- **Component**: An interface or abstract class representing the core functionality.
- **ConcreteComponent**: A class that implements the Component interface and represents the core object whose functionality can be dynamically extended.
- **Decorator**: An abstract class that implements the Component interface and contains a reference to a Component object. It acts as a base for concrete decorators.
- **ConcreteDecorator**: A class that extends the Decorator and adds additional behavior.

**Structure**
- **Component**: Defines the interface for objects that can have responsibilities added to them.
- **ConcreteComponent**: Defines an object to which additional responsibilities can be attached.
- **Decorator**: Maintains a reference to a Component object and defines an interface that conforms to the Component's interface.
- **ConcreteDecorator**: Adds responsibilities to the component.

**Example**: Enhancing a Coffee Object\
Consider a scenario where you want to create a coffee ordering system. You start with a simple coffee and add various ingredients like milk, sugar, etc.

**Step 1: Define the Component Interface**
```java

public interface Coffee {
    String getDescription();
    double getCost();
}
```
**Step 2: Implement the ConcreteComponent**
```java

public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}
```
**Step 3: Create the Decorator Abstract Class**
```java

public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```
**Step 4: Implement Concrete Decorators**
```java

public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 1.5;
    }
}

public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 0.5;
    }
}
```
**Step 5: Demonstrate the Decorator Pattern**
```java

public class DecoratorPatternDemo {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " $" + coffee.getCost());

        coffee = new MilkDecorator(coffee);
        System.out.println(coffee.getDescription() + " $" + coffee.getCost());

        coffee = new SugarDecorator(coffee);
        System.out.println(coffee.getDescription() + " $" + coffee.getCost());
    }
}
```
**Advantages of the Decorator Pattern**
- **Flexibility**: Adds behavior to individual objects without affecting other objects of the same class.
- **Adheres to Open/Closed Principle**: New functionality can be added without modifying existing code.
- **Combines Behaviors**: Multiple decorators can be combined to create complex behaviors from simple ones.
- 
**Disadvantages of the Decorator Pattern**
- **Complexity**: Can lead to many small classes, making the system harder to understand and maintain.
- **Debugging**: Can be difficult due to the layered nature of decorators.
- **Identity**: Wrapped objects may not be easily identifiable as their original type, complicating instance checks and equality tests.

**Real-World Examples**
- **Java I/O Streams**: The java.io package uses decorators extensively, such as BufferedReader, InputStreamReader, FileInputStream, etc.
- **GUI Toolkits**: Toolkits like Java Swing and AWT use decorators to add behaviors to GUI components, such as adding borders, scrollbars, etc.
- **Logger Enhancements**: Adding additional behaviors to logging, such as formatting, filtering, or routing logs to different destinations.

### Q. What is the Observer pattern? Provide an example.
Answer:\
The Observer pattern defines a one-to-many dependency between objects, where one object (subject) notifies its dependents (observers) of state changes.

```java
interface Observer {
    void update(String message);
}

class ConcreteObserver implements Observer {
    public void update(String message) {
        System.out.println("Received message: " + message);
    }
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    public void addObserver(Observer observer) {
        observers.add(observer);
    }
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}
```

### Q. What is the Adapter pattern? Provide an example.
Answer:\
The Adapter Design Pattern is a structural pattern that allows incompatible interfaces to work together. It acts as a bridge between two incompatible interfaces, converting the interface of a class into another interface that a client expects. This pattern is useful when you want to use an existing class but its interface does not match the one you need.

**Key Concepts**
- **Target**: This is the interface that the client expects.
- **Client**: This is the class that interacts with the target interface.
- **Adaptee**: This is the existing class that needs to be adapted.
- **Adapter**: This class implements the target interface and holds an instance of the Adaptee. It translates the requests from the target interface to the Adaptee.

**Structure**
- **Target Interface**: Defines the domain-specific interface that the Client uses.
- **Adaptee**: An existing interface that needs adapting.
- **Adapter**: Implements the Target interface and translates the requests from the Target to the Adaptee.
- **Client**: Uses the Target interface to interact with the Adapter.

**Example**: Using a Square Peg in a Round Hole\
Consider a scenario where you have a RoundHole that accepts RoundPegs, but you have a SquarePeg that you want to fit into the RoundHole.

**Step 1: Define the Target Interface and Client**
```java

public interface RoundPeg {
    double getRadius();
}

public class RoundHole {
    private double radius;

    public RoundHole(double radius) {
        this.radius = radius;
    }

    public boolean fits(RoundPeg peg) {
        return this.radius >= peg.getRadius();
    }
}
```
**Step 2: Define the Adaptee**
```java
public class SquarePeg {
    private double width;

    public SquarePeg(double width) {
        this.width = width;
    }

    public double getWidth() {
        return width;
    }
}
```
**Step 3: Implement the Adapter**
```java

public class SquarePegAdapter implements RoundPeg {
    private SquarePeg squarePeg;

    public SquarePegAdapter(SquarePeg squarePeg) {
        this.squarePeg = squarePeg;
    }

    @Override
    public double getRadius() {
        // Convert the width of the square peg to the radius of a round peg that would fit
        return squarePeg.getWidth() * Math.sqrt(2) / 2;
    }
}
```
**Step 4: Demonstrate the Adapter Pattern**
```java
public class AdapterPatternDemo {
    public static void main(String[] args) {
        RoundHole hole = new RoundHole(5.0);
        RoundPeg roundPeg = new RoundPeg() {
            @Override
            public double getRadius() {
                return 5.0;
            }
        };
        SquarePeg smallSquarePeg = new SquarePeg(5.0);
        SquarePeg largeSquarePeg = new SquarePeg(10.0);

        SquarePegAdapter smallSquarePegAdapter = new SquarePegAdapter(smallSquarePeg);
        SquarePegAdapter largeSquarePegAdapter = new SquarePegAdapter(largeSquarePeg);

        System.out.println("Round peg fits: " + hole.fits(roundPeg)); // true
        System.out.println("Small square peg fits: " + hole.fits(smallSquarePegAdapter)); // true
        System.out.println("Large square peg fits: " + hole.fits(largeSquarePegAdapter)); // false
    }
}
```
**Advantages of the Adapter Pattern**
- **Reuse of Existing Code**: Allows the use of existing classes even if they don’t have the necessary interface.
- **Flexibility**: Can work with many incompatible interfaces, making it highly adaptable.
- **Separation of Concerns**: The adapter separates the client from the adaptee, which keeps the system modular and easy to maintain.

**Disadvantages of the Adapter Pattern**
- **Complexity**: Adds extra layers, which can increase complexity.
- **Performance Overhead**: Can introduce a performance overhead due to the additional level of indirection.

**Types of Adapters**
- **Class Adapter**: Uses inheritance to adapt one interface to another. This requires multiple inheritance, which is not supported in Java, hence less common.
- **Object Adapter**: Uses composition to adapt one interface to another. This is more flexible and widely used in Java.

**Real-World Examples**
- **Legacy Code Integration**: Integrating a new system with legacy code that has different interfaces.
- **Third-Party Library Integration**: Using third-party libraries with interfaces that do not match the ones used in your application.
- **User Interface Components**: Adapting different UI components to work with a common interface

### Q. What is Builder Pattern?
Answer\
The Builder Design Pattern is a creational design pattern that allows for the step-by-step construction of complex objects. Unlike other creational patterns, the Builder Pattern does not require that products have a common base class or interface. This pattern is especially useful when an object needs to be created with many possible configurations, which can make constructors with multiple parameters unwieldy and hard to read.

**Key Concepts**
- **Builder**: The builder interface specifies the methods to create different parts of the product.
- **Concrete Builder**: This class implements the Builder interface and provides specific implementations of the construction methods. It keeps track of the product being built.
- **Product**: The complex object that is being built.
- **Director**: (Optional) This class controls the construction process. It uses the builder interface to construct the product.

**Structure**
- Builder Interface:
  - Defines the construction steps for building the product.
- Concrete Builder:
  - Implements the steps defined in the Builder interface.
  - Has a method to return the final product.
- Product:
  - The complex object that is being built.
- Director:
  - Orchestrates the building process using the builder.

**Example: Building a House**
**Step 1: Define the Product**
```java

public class House {
    private String foundation;
    private String structure;
    private String roof;
    private boolean furnished;
    private boolean painted;

    // Getters and setters

    @Override
    public String toString() {
        return "House [foundation=" + foundation + ", structure=" + structure +
               ", roof=" + roof + ", furnished=" + furnished + ", painted=" + painted + "]";
    }
}
```

**Step 2: Create the Builder Interface**
```java
public interface HouseBuilder {
    void buildFoundation();
    void buildStructure();
    void buildRoof();
    void furnishHouse();
    void paintHouse();
    House getHouse();
}
```
**Step 3: Implement the Concrete Builder**
```java
public class ConcreteHouseBuilder implements HouseBuilder {
    private House house;

    public ConcreteHouseBuilder() {
        this.house = new House();
    }

    @Override
    public void buildFoundation() {
        house.setFoundation("Concrete, brick, and stone");
        System.out.println("Foundation completed.");
    }

    @Override
    public void buildStructure() {
        house.setStructure("Wood and brick");
        System.out.println("Structure completed.");
    }

    @Override
    public void buildRoof() {
        house.setRoof("Concrete and reinforced steel");
        System.out.println("Roof completed.");
    }

    @Override
    public void furnishHouse() {
        house.setFurnished(true);
        System.out.println("House furnished.");
    }

    @Override
    public void paintHouse() {
        house.setPainted(true);
        System.out.println("House painted.");
    }

    @Override
    public House getHouse() {
        return this.house;
    }
}
```
**Step 4: Use the Director (Optional)**
```java

public class Director {
    private HouseBuilder houseBuilder;

    public Director(HouseBuilder houseBuilder) {
        this.houseBuilder = houseBuilder;
    }

    public void constructHouse() {
        houseBuilder.buildFoundation();
        houseBuilder.buildStructure();
        houseBuilder.buildRoof();
        houseBuilder.furnishHouse();
        houseBuilder.paintHouse();
    }
}
```
**Step 5: Demonstrate the Builder Pattern**
```java

public class BuilderPatternDemo {
    public static void main(String[] args) {
        HouseBuilder builder = new ConcreteHouseBuilder();
        Director director = new Director(builder);

        director.constructHouse();
        House house = builder.getHouse();

        System.out.println("House is built:\n" + house);
    }
}
```
**Advantages of the Builder Pattern**
- **Improves Readability**: Clearly separates the construction process from the representation.
- **Encapsulation**: Hides the internal representation and details of the product.
- **Flexibility**: Allows different representations of the product to be created using the same construction process.
- **Step-by-Step Construction**: Helps in constructing complex objects incrementally.
**Disadvantages of the Builder Pattern**
- **Complexity**: Can add complexity if only a simple object construction is needed.
- **Multiple Builders**: For different types of products, multiple builder classes need to be created, which can increase the codebase.

## Spring Framework
### Q. What is Spring Framework and what are its main modules?
Answer:\
Spring Framework is a comprehensive framework for enterprise Java development. Main modules include:

- Core Container (Spring Core, Spring Context, Spring Beans, Spring Expression Language).
- Data Access/Integration (JDBC, ORM, OXM, JMS, Transactions).
- Web (Web, Web-Servlet, Web-Struts, Web-Portlet).
- AOP (Aspect-Oriented Programming).
- Testing (Mocking, TestContext, Spring MVC Test).

### Q. Explain the concept of dependency injection in Spring.
Answer:\
**Dependency Injection** (DI) is a design pattern where an object’s dependencies are injected by a container rather than being created by the object itself. In Spring, DI can be achieved through constructor injection, setter injection, and field injection.

### Q. What is Spring Boot and how does it differ from the Spring Framework?
Answer:\
Spring Boot is a project built on top of the Spring Framework. It simplifies the setup, configuration, and development of new Spring applications by providing defaults and auto-configuration. Unlike the core Spring Framework, Spring Boot allows creating stand-alone applications with embedded servers.

### Q. How do you handle transactions in Spring?
Answer:\
Transactions in Spring can be managed using the @Transactional annotation or through the Spring PlatformTransactionManager API. This ensures that a sequence of operations are executed as a single unit of work.

### Q. What is Spring AOP and what are its use cases?
Answer:\
Spring AOP (Aspect-Oriented Programming) is a programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns (e.g., logging, security). It involves defining aspects, advice, pointcuts, and join points to manage these concerns.

## Java EE and Web Technologies
### Q. What is the difference between Servlets and JSP?
Answer:\

- **Servlets**: Java programs that handle client requests and generate dynamic content.
- **JSP (JavaServer Pages)**: A technology that simplifies the creation of dynamic web pages by allowing HTML to be mixed with Java code.

### Q. Explain the MVC pattern in the context of a Java web application.
Answer:\
The MVC (Model-View-Controller) pattern separates an application into three main components:

- **Model**: Represents the application's data.
- **View**: Presents the data (UI).
- **Controller**: Handles user input, updates the model, and selects the view for rendering.

### Q. What is the purpose of the ServletContext and ServletConfig?
Answer:\

- **ServletContext**: Provides information about the web application and allows interaction with the web container.
- **ServletConfig**: Contains configuration information for a single servlet.

### Q. How do you handle sessions in a Java web application?
Answer:\
Sessions in Java web applications can be managed using HttpSession, which allows storing and retrieving user-specific data for the duration of the user's interaction with the web application.

### Q. What is RESTful web service and how do you create one in Java?
Answer:\
A **RESTful** web service adheres to REST principles, using standard HTTP methods (GET, POST, PUT, DELETE) for operations. In Java, you can create RESTful services using frameworks like JAX-RS (e.g., Jersey, RESTEasy) or Spring MVC.

```java
@Path("/example")
public class ExampleResource {
    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Response getExample() {
        return Response.ok(new Example("Hello, world!")).build();
    }
}
```

## Database and Persistence
### Q. What is JPA and how does it relate to Hibernate?
Answer:
**JPA** (Java Persistence API) is a specification for object-relational mapping (ORM) in Java. **Hibernate** is a popular implementation of the JPA specification, providing the actual ORM functionality.

### Q. How do you configure and use a DataSource in a Spring application?
Answer:\
You can configure a DataSource bean in a Spring application using Java configuration or XML:

```java
@Bean
public DataSource dataSource() {
    DriverManagerDataSource dataSource = new DriverManagerDataSource();
    dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
    dataSource.setUrl("jdbc:mysql://localhost:3306/mydb");
    dataSource.setUsername("user");
    dataSource.setPassword("password");
    return dataSource;
}
```

### Q. Explain the concept of a transaction and how it is managed in JPA.
Answer:\
A transaction in JPA represents a unit of work that is performed atomically. Transactions are managed using the EntityTransaction interface in a Java SE environment or using @Transactional annotation in a Spring environment.

### Q. What is a NamedQuery in JPA?
Answer:\
A NamedQuery is a static query expressed in the JPA query language, defined using the @NamedQuery annotation and can be reused across the application.

```java
@NamedQuery(
    name = "findAllCustomers",
    query = "SELECT c FROM Customer c"
)
```

### Q. What are the benefits of using connection pooling?
Answer:\
Connection pooling improves performance by reusing database connections, reducing the overhead of establishing new connections, and controlling resource usage by limiting the number of active connections.

## Testing and Debugging
### Q. How do you write unit tests for a Java application?
Answer:\
Unit tests in Java can be written using frameworks like JUnit or TestNG. A typical unit test includes setting up test data, invoking methods, and asserting expected outcomes.

```java
@Test
public void testAddition() {
    Calculator calculator = new Calculator();
    assertEquals(5, calculator.add(2, 3));
}
```

### Q. What is mocking and how do you use it in testing?
Answer:\
Mocking is the process of creating mock objects to simulate the behavior of real objects. This allows testing of code in isolation. Frameworks like Mockito are commonly used for mocking in Java.

```java
Mockito.when(mockedList.get(0)).thenReturn("mocked value");
```

### Q. Explain the concept of integration testing.
Answer:\
Integration testing involves testing the integration of multiple components or systems to ensure they work together as expected. It typically includes testing data access, network communication, and interactions between modules.

### Q. What are the common tools for profiling and monitoring Java applications?
Answer:\
Common tools include VisualVM, JProfiler, YourKit, and Java Mission Control (JMC). These tools help monitor performance, memory usage, and identify bottlenecks.

### Q. How do you debug a Java application?
Answer:\
Debugging can be done using IDEs (like Eclipse, IntelliJ IDEA) which provide breakpoints, watches, and step-by-step execution. Logging frameworks (like SLF4J, Log4j) also aid in debugging by capturing runtime information.

## Miscellaneous
### Q. What are the differences between JDK, JRE, and JVM?
Answer:\

- **JDK** (Java Development Kit): Includes tools for developing Java applications (compiler, debugger) and the JRE.
- **JRE** (Java Runtime Environment): Includes the JVM and libraries for running Java applications.
- **JVM** (Java Virtual Machine): Executes Java bytecode and provides platform independence.

### Q. What is the purpose of the transient keyword?
Answer:\
The transient keyword is used to indicate that a field should not be serialized. During serialization, the value of a transient field is not saved and is set to its default value when deserialized.

### Q. What is reflection in Java and how is it used?
Answer:\
Reflection is the ability of a program to examine and modify its structure and behavior at runtime. It is used for inspecting classes, methods, fields, and invoking methods dynamically.

```java
Method method = obj.getClass().getMethod("myMethod");
method.invoke(obj);
```
### Q. Explain the difference between a checked exception and an unchecked exception.

Answer:\

- **Checked Exception**: Subclasses of Exception (excluding RuntimeException) that must be either caught or declared in the method signature using throws.
- **Unchecked Exception**: Subclasses of RuntimeException that do not need to be explicitly handled.

### Q. How do you create and use annotations in Java?
Answer:\
Annotations are created using the @interface keyword and can be applied to classes, methods, fields, etc. They can be processed at runtime using reflection or at compile time using annotation processors.

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    String value();
}

@MyAnnotation(value = "example")
public void myMethod() {
    // Method logic
}
```
