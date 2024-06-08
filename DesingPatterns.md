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


### Q. Explain Bridge Pattern.
Answer:\
The Bridge Pattern is a structural design pattern that separates an object’s abstraction from its implementation, allowing the two to vary independently. This pattern is particularly useful when both the abstraction and the implementation may change over time but need to remain independent from each other to avoid a combinatorial explosion of classes.

**Key Concepts**
- **Abstraction**: The interface or abstract class defining the abstraction’s operations.
- **Refined Abstraction**: A subclass of the Abstraction that extends the interface defined by Abstraction.
- **Implementor**: An interface for implementation classes.
- **Concrete Implementor**: Concrete classes that implement the Implementor interface.
**Structure**
- **Abstraction**: Defines the abstraction's interface and maintains a reference to an object of type Implementor.
- **RefinedAbstraction**: Extends the Abstraction and implements its operations.
- **Implementor**: Defines the interface for implementation classes.
- **ConcreteImplementor**: Implements the Implementor interface and provides concrete implementations of the operations.

**Example**: Drawing Shapes with Different APIs\
Consider a scenario where you want to draw shapes like circles and rectangles using different drawing APIs (e.g., SVG, Canvas).

**Step 1: Define the Implementor Interface**
```java
public interface DrawingAPI {
    void drawCircle(double x, double y, double radius);
    void drawRectangle(double x, double y, double width, double height);
}
```
**Step 2: Implement Concrete Implementors**
```java

public class SvgDrawingAPI implements DrawingAPI {
    @Override
    public void drawCircle(double x, double y, double radius) {
        System.out.println("Drawing Circle in SVG at (" + x + ", " + y + ") with radius " + radius);
    }

    @Override
    public void drawRectangle(double x, double y, double width, double height) {
        System.out.println("Drawing Rectangle in SVG at (" + x + ", " + y + ") with width " + width + " and height " + height);
    }
}

public class CanvasDrawingAPI implements DrawingAPI {
    @Override
    public void drawCircle(double x, double y, double radius) {
        System.out.println("Drawing Circle on Canvas at (" + x + ", " + y + ") with radius " + radius);
    }

    @Override
    public void drawRectangle(double x, double y, double width, double height) {
        System.out.println("Drawing Rectangle on Canvas at (" + x + ", " + y + ") with width " + width + " and height " + height);
    }
}
```
**Step 3: Define the Abstraction**
```java

public abstract class Shape {
    protected DrawingAPI drawingAPI;

    protected Shape(DrawingAPI drawingAPI) {
        this.drawingAPI = drawingAPI;
    }

    public abstract void draw();
    public abstract void resize(double factor);
}
```
**Step 4: Implement Refined Abstractions**
```java
public class Circle extends Shape {
    private double x, y, radius;

    public Circle(double x, double y, double radius, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    @Override
    public void draw() {
        drawingAPI.drawCircle(x, y, radius);
    }

    @Override
    public void resize(double factor) {
        radius *= factor;
    }
}

public class Rectangle extends Shape {
    private double x, y, width, height;

    public Rectangle(double x, double y, double width, double height, DrawingAPI drawingAPI) {
        super(drawingAPI);
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
    }

    @Override
    public void draw() {
        drawingAPI.drawRectangle(x, y, width, height);
    }

    @Override
    public void resize(double factor) {
        width *= factor;
        height *= factor;
    }
}
```
**Step 5: Demonstrate the Bridge Pattern**
```java

public class BridgePatternDemo {
    public static void main(String[] args) {
        DrawingAPI svgAPI = new SvgDrawingAPI();
        DrawingAPI canvasAPI = new CanvasDrawingAPI();

        Shape circle = new Circle(5, 10, 20, svgAPI);
        Shape rectangle = new Rectangle(15, 30, 40, 50, canvasAPI);

        circle.draw(); // Drawing Circle in SVG at (5.0, 10.0) with radius 20.0
        rectangle.draw(); // Drawing Rectangle on Canvas at (15.0, 30.0) with width 40.0 and height 50.0

        circle.resize(2);
        rectangle.resize(0.5);

        circle.draw(); // Drawing Circle in SVG at (5.0, 10.0) with radius 40.0
        rectangle.draw(); // Drawing Rectangle on Canvas at (15.0, 30.0) with width 20.0 and height 25.0
    }
}
```

**Advantages of the Bridge Pattern**
- **Decoupling Abstraction and Implementation**: Allows the abstraction and implementation to vary independently.
- **Enhanced Extensibility**: Both abstractions and implementations can be extended independently.
- **Reduced Code Duplication**: Common code can be shared among different abstractions and implementations.

**Disadvantages of the Bridge Pattern**
- **Increased Complexity**: Introduces additional layers of abstraction, which can make the code more complex.
- **Overhead**: The pattern might introduce unnecessary overhead if the flexibility it provides is not needed.

**Real-World Examples**
- **GUI Frameworks**: Separating the window system interface from the window system implementation.
- **File Systems**: Abstracting file system operations from the underlying file system implementation.
- **Networking**: Abstracting communication protocols from the underlying network layer.

### Q. Explain Proxy Pattern
Answer:\
The Proxy Pattern is a structural design pattern that provides a surrogate or placeholder for another object to control access to it. This pattern is useful in situations where an object is resource-intensive to create or where additional functionality (like access control, logging, caching, etc.) is needed without changing the original object’s code.

**Key Concepts**
- **Proxy**: The proxy object that controls access to the real object.
- **RealSubject**: The real object that the proxy represents and controls access to.
- **Subject**: An interface that both the Proxy and RealSubject implement, ensuring they can be used interchangeably.

**Types of Proxies**
- **Remote Proxy**: Manages interaction with a remote object.
- **Virtual Proxy**: Manages access to a resource that is expensive to create or access.
- **Protection Proxy**: Controls access to the original object based on access rights.
- **Smart Proxy**: Adds additional behavior when accessing the object, such as reference counting or logging.

**Structure**
- **Subject Interface**: Declares the common interface for RealSubject and Proxy.
- **RealSubject**: Implements the Subject interface and represents the actual object.
- **Proxy**: Implements the Subject interface and holds a reference to the RealSubject. It controls access to the RealSubject and may handle instantiation, access control, or additional functionalities.

**Example**: Image Viewer with Virtual Proxy\
Consider a scenario where you want to display an image in an application, but loading the image from disk is resource-intensive. You can use a virtual proxy to delay the loading of the image until it's actually needed.

**Step 1: Define the Subject Interface**
```java

public interface Image {
    void display();
}
```
**Step 2: Implement the RealSubject**
```java

public class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading " + fileName);
    }

    @Override
    public void display() {
        System.out.println("Displaying " + fileName);
    }
}
```
**Step 3: Implement the Proxy**
```java

public class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;

    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName);
        }
        realImage.display();
    }
}
```
**Step 4: Demonstrate the Proxy Pattern**
```java

public class ProxyPatternDemo {
    public static void main(String[] args) {
        Image image = new ProxyImage("test_image.jpg");

        // Image will be loaded from disk
        image.display();

        // Image will not be loaded from disk again
        image.display();
    }
}
```

**Advantages of the Proxy Pattern**
- **Control Access**: Can control access to the real object, useful for security and logging.
- **Lazy Initialization**: Delays the creation and initialization of the resource until it is needed.
- **Performance Optimization**: Can cache results to enhance performance for expensive operations.
- **Separation of Concerns**: Keeps additional functionality separate from the real object.

**Disadvantages of the Proxy Pattern**
- **Complexity**: Introduces additional classes and complexity.
- **Latency**: May introduce latency in the response time of the real subject.

**Real-World Examples**
- **Java RMI (Remote Method Invocation)**: Uses proxy objects to interact with remote objects.
- **Spring AOP (Aspect-Oriented Programming)**: Uses proxies to apply cross-cutting concerns such as logging, transaction management, etc.
- **Security Proxies**: Used to control access to sensitive objects based on user permissions.


### Q. Explain Command Pattern
Answer:\
The Command Pattern is a behavioral design pattern that turns a request into a stand-alone object containing all the information about the request. This transformation enables parameterization of methods with different requests, queuing of requests, logging the requests, and supporting undoable operations.

**Key Concepts**
- **Command**: An interface or abstract class that declares a method for executing a command.
- **ConcreteCommand**: Implements the Command interface and defines the relationship between the receiver and the action.
- **Client**: Creates a ConcreteCommand object and sets its receiver.
- **Invoker**: Asks the command to carry out the request.
- **Receiver**: Knows how to perform the operations associated with carrying out the request.

**Structure**
- **Command Interface**: Declares the interface for executing operations.
- **ConcreteCommand**: Binds a receiver object to an action, invoking the receiver's operations.
- **Client**: Creates the ConcreteCommand object and sets its receiver.
- **Invoker**: Holds a command and at some point asks it to carry out a request.
- **Receiver**: Performs the actual work when the execute method in the command is called.

**Example**: Simple Remote Control\
Consider a scenario where you have a remote control with different buttons to perform various actions like turning on/off a light and turning on/off a fan.

**Step 1: Define the Command Interface**
```java
public interface Command {
    void execute();
    void undo();
}
```
**Step 2: Implement Concrete Commands**
```java
public class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.on();
    }

    @Override
    public void undo() {
        light.off();
    }
}

public class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.off();
    }

    @Override
    public void undo() {
        light.on();
    }
}

public class FanOnCommand implements Command {
    private Fan fan;

    public FanOnCommand(Fan fan) {
        this.fan = fan;
    }

    @Override
    public void execute() {
        fan.on();
    }

    @Override
    public void undo() {
        fan.off();
    }
}

public class FanOffCommand implements Command {
    private Fan fan;

    public FanOffCommand(Fan fan) {
        this.fan = fan;
    }

    @Override
    public void execute() {
        fan.off();
    }

    @Override
    public void undo() {
        fan.on();
    }
}
```
**Step 3: Implement the Receiver Classes**
```java

public class Light {
    public void on() {
        System.out.println("The light is on");
    }

    public void off() {
        System.out.println("The light is off");
    }
}

public class Fan {
    public void on() {
        System.out.println("The fan is on");
    }

    public void off() {
        System.out.println("The fan is off");
    }
}
```
**Step 4: Create the Invoker Class**
```java

public class RemoteControl {
    private Command[] onCommands;
    private Command[] offCommands;
    private Command undoCommand;

    public RemoteControl() {
        onCommands = new Command[2];
        offCommands = new Command[2];
    }

    public void setCommand(int slot, Command onCommand, Command offCommand) {
        onCommands[slot] = onCommand;
        offCommands[slot] = offCommand;
    }

    public void onButtonWasPushed(int slot) {
        onCommands[slot].execute();
        undoCommand = onCommands[slot];
    }

    public void offButtonWasPushed(int slot) {
        offCommands[slot].execute();
        undoCommand = offCommands[slot];
    }

    public void undoButtonWasPushed() {
        undoCommand.undo();
    }
}
```
**Step 5: Demonstrate the Command Pattern**
```java
public class CommandPatternDemo {
    public static void main(String[] args) {
        RemoteControl remote = new RemoteControl();

        Light livingRoomLight = new Light();
        Fan ceilingFan = new Fan();

        LightOnCommand lightOn = new LightOnCommand(livingRoomLight);
        LightOffCommand lightOff = new LightOffCommand(livingRoomLight);

        FanOnCommand fanOn = new FanOnCommand(ceilingFan);
        FanOffCommand fanOff = new FanOffCommand(ceilingFan);

        remote.setCommand(0, lightOn, lightOff);
        remote.setCommand(1, fanOn, fanOff);

        remote.onButtonWasPushed(0);
        remote.offButtonWasPushed(0);
        remote.undoButtonWasPushed();

        remote.onButtonWasPushed(1);
        remote.offButtonWasPushed(1);
        remote.undoButtonWasPushed();
    }
}
```
**Advantages of the Command Pattern**
- **Decoupling**: Decouples the object that invokes the operation from the one that knows how to perform it.
- **Extensibility**: Adding new commands is easy without changing existing code.
- **Undo/Redo Functionality**: Easily supports undo and redo operations.
- **Macro Commands**: Supports combining multiple commands into a single command.

**Disadvantages of the Command Pattern**
- **Complexity**: Introduces additional classes and objects, which can make the system more complex.
- **Overhead**: Can lead to unnecessary memory and processing overhead if not used judiciously.

**Real-World Examples**
- **GUI Button Actions**: Buttons in a graphical user interface triggering actions.
- **Transaction Management**: Encapsulating all the information needed for a transaction to be executed.
- **Macro Recording**: Recording and executing a sequence of commands as a macro.


### Q. What is the Observer pattern? Provide an example.
Answer:\
The Observer Pattern is a behavioral design pattern that defines a one-to-many dependency between objects, so that when one object (the subject) changes state, all its dependents (observers) are notified and updated automatically. This pattern is particularly useful for implementing distributed event handling systems, where an event triggers a notification to multiple objects.

**Key Concepts**
- **Subject**: The object that holds the state and sends notifications to observers when the state changes.
- **Observer**: An interface or abstract class that defines the method to be called when the subject's state changes.
- **ConcreteObserver**: A class that implements the Observer interface and updates its state based on notifications from the subject.
- **ConcreteSubject**: A class that implements the Subject interface, maintains state, and sends notifications to its observers.

**Structure**
- **Subject Interface**: Defines methods to attach, detach, and notify observers.
- **Observer Interface**: Defines the update method that will be called when the subject's state changes.
- **ConcreteSubject**: Implements the Subject interface, holds the state of interest to ConcreteObserver objects, and sends notifications when the state changes.
- **ConcreteObserver**: Implements the Observer interface and updates its state based on the subject's state.

**Example**: Weather Monitoring System\
Let's implement a simple weather monitoring system where different displays (observers) get updated when the weather data (subject) changes.

**Step 1: Define the Observer Interface**
```java

public interface Observer {
    void update(float temperature, float humidity, float pressure);
}
```
**Step 2: Define the Subject Interface**
```java

public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```
**Step 3: Implement the ConcreteSubject**
```java

import java.util.ArrayList;
import java.util.List;

public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }

    private void measurementsChanged() {
        notifyObservers();
    }
}
```
**Step 4: Implement the ConcreteObservers**
```java

public class CurrentConditionsDisplay implements Observer {
    private float temperature;
    private float humidity;

    @Override
    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}

public class StatisticsDisplay implements Observer {
    private float maxTemp = 0.0f;
    private float minTemp = 200;
    private float tempSum = 0.0f;
    private int numReadings;

    @Override
    public void update(float temperature, float humidity, float pressure) {
        tempSum += temperature;
        numReadings++;

        if (temperature > maxTemp) {
            maxTemp = temperature;
        }

        if (temperature < minTemp) {
            minTemp = temperature;
        }

        display();
    }

    public void display() {
        System.out.println("Avg/Max/Min temperature = " + (tempSum / numReadings) + "/" + maxTemp + "/" + minTemp);
    }
}
```

**Step 5: Demonstrate the Observer Pattern**
```java
public class WeatherStation {
    public static void main(String[] args) {
        WeatherData weatherData = new WeatherData();

        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay();
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay();

        weatherData.registerObserver(currentDisplay);
        weatherData.registerObserver(statisticsDisplay);

        weatherData.setMeasurements(80, 65, 30.4f);
        weatherData.setMeasurements(82, 70, 29.2f);
        weatherData.setMeasurements(78, 90, 29.2f);
    }
}
```

**Advantages of the Observer Pattern**
- **Loose Coupling**: The subject and observers are loosely coupled, meaning the subject doesn't need to know the details of the observers.
- **Scalability**: Allows adding or removing observers at runtime without modifying the subject.
- **Automatic Updates**: Observers are automatically notified and updated when the subject's state changes.

**Disadvantages of the Observer Pattern**
- **Memory Leaks**: If observers are not properly deregistered, it can lead to memory leaks.
- **Notification Overhead**: Can cause a lot of notifications if not managed properly, leading to performance issues.
- **Complexity**: Can introduce additional complexity in ensuring all observers are properly updated.

**Real-World Examples**
- **Event Handling Systems**: Used in GUI frameworks to handle events like button clicks.
- **Model-View-Controller (MVC)**: The View (Observer) updates when the Model (Subject) changes.
-** Publish-Subscribe Systems**: Used in messaging systems where messages are broadcast to multiple subscribers.

### Q. Explain Iterator Pattern
The Iterator Pattern is a behavioral design pattern that allows sequential access to elements of a collection without exposing its underlying representation. It provides a way to access the elements of an aggregate object (like an array or list) in a systematic manner.

**Key Concepts**
- **Iterator**: An interface or abstract class that declares methods for accessing and traversing elements.
- **Concrete Iterator**: A class that implements the Iterator interface for a specific collection.
- **Aggregate (or Collection)**: An interface that declares methods for creating an iterator object.
- **Concrete Aggregate**: A class that implements the Aggregate interface and returns an instance of the Concrete Iterator.

**Structure**
- **Iterator Interface**: Defines the methods required for traversing a collection, typically including next(), hasNext(), and sometimes remove().
- **Concrete Iterator**: Implements the Iterator interface and keeps track of the current position in the traversal.
- **Aggregate Interface**: Defines a method to create an iterator, often named createIterator().
- **Concrete Aggregate**: Implements the Aggregate interface and returns an instance of the Concrete Iterator.

**Example**: Iterating Over a List of Names\
Let's implement a simple iterator pattern to traverse a list of names.

**Step 1: Define the Iterator Interface**
```java

public interface Iterator<T> {
    boolean hasNext();
    T next();
}
```
**Step 2: Implement the Concrete Iterator**
```java

public class NameIterator implements Iterator<String> {
    private String[] names;
    private int position;

    public NameIterator(String[] names) {
        this.names = names;
        this.position = 0;
    }

    @Override
    public boolean hasNext() {
        return position < names.length;
    }

    @Override
    public String next() {
        if (this.hasNext()) {
            return names[position++];
        }
        return null;
    }
}
```
**Step 3: Define the Aggregate Interface**
```java

public interface Container<T> {
    Iterator<T> createIterator();
}
```
**Step 4: Implement the Concrete Aggregate**
```java

public class NameRepository implements Container<String> {
    private String[] names = {"John", "Jane", "Jack", "Jill"};

    @Override
    public Iterator<String> createIterator() {
        return new NameIterator(names);
    }
}
```
**Step 5: Demonstrate the Iterator Pattern**
```java

public class IteratorPatternDemo {
    public static void main(String[] args) {
        NameRepository nameRepository = new NameRepository();

        Iterator<String> iterator = nameRepository.createIterator();
        while (iterator.hasNext()) {
            String name = iterator.next();
            System.out.println("Name: " + name);
        }
    }
}
```

**Advantages of the Iterator Pattern**
- **Single Responsibility Principle**: Separates the traversal logic from the collection, simplifying both the iterator and the collection.
- **Uniform Interface**: Provides a standard way to traverse different collections without exposing their internal structure.
- **Flexibility**: Supports different types of iterations (e.g., forward, backward) without modifying the collection.

**Disadvantages of the Iterator Pattern**
- **Increased Complexity**: Introduces additional classes for each type of collection.
- **Performance Overhead**: Can add a performance overhead due to the abstraction layer.

**Real-World Examples**
- **Java Collections Framework**: The java.util.Iterator and java.util.ListIterator interfaces.
- **C++ Standard Template Library (STL)**: Iterators used in the STL for traversing containers.
- **Python Iterators**: The __iter__() and __next__() methods in Python's iterator protocol.

**Variations of the Iterator Pattern**
- **External Iterator**: The client controls the iteration.
- **Internal Iterator**: The collection controls the iteration.
- **Reverse Iterator**: Traverses the collection in reverse order.


