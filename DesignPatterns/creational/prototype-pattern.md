## Prototype Design Pattern

The Prototype Design Pattern is a creational pattern that enables the creation of new objects by copying an existing object. Prototype allows us to hide the complexity of making new instances from the client. The concept is to copy an existing object rather than create a new instance from scratch, something that may include costly operations. The existing object acts as a prototype and contains the state of the object.

- The newly copied object may change the same properties only if required. This approach saves costly resources and time, especially when object creation is a heavy process.
- The prototype pattern is a creational design pattern. Prototype patterns are required when object creation is a time-consuming, and costly operation, so we create objects with the existing object itself.
- One of the best available ways to create an object from existing objects is the clone() method. Clone is the simplest approach to implementing a prototype pattern. However, it is your call to decide how to copy existing objects based on your business model.

**Example**: Suppose a user creates a document with a specific layout, fonts, and styling, and wishes to create similar documents with slight modifications.

![alt text](./prototype-pattern.png)

Prototype Interface
```java
public interface Shape {
    Shape clone();  // Make a copy of itself
    void draw();    // Draw the shape
}
```

Concrete Prototype
```java
public class Circle implements Shape {
    private String color;
 
    // When you create a circle, you give it a color.
    public Circle(String color) {
        this.color = color;
    }
 
    // This creates a copy of the circle.
    @Override
    public Shape clone() {
        return new Circle(this.color);
    }
 
    // This is how a circle draws itself.
    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " circle.");
    }
}
```

Client
```java
public class ShapeClient {
    private Shape shapePrototype;
 
    // When you create a client, you give it a prototype (a shape).
    public ShapeClient(Shape shapePrototype) {
        this.shapePrototype = shapePrototype;
    }
 
    // This method creates a new shape using the prototype.
    public Shape createShape() {
        return shapePrototype.clone();
    }
}
```

Example
```java
public class PrototypeExample {
    public static void main(String[] args) {
        // Create a concrete prototype (a red circle).
        Shape circlePrototype = new Circle("red");
 
        // Create a client and give it the prototype.
        ShapeClient client = new ShapeClient(circlePrototype);
 
        // Use the prototype to create a new shape (a red circle).
        Shape redCircle = client.createShape();
 
        // Draw the newly created red circle.
        redCircle.draw();
    }
}
```
