# SOLID

## S - Single Responsibility Principle
Every software component (can be class, method or module) should have one and only one responsibility

#### Cohesion - degree to which various parts of a software component are related

Example of Low level of Cohesion - draw & rotate methods are unrelated to calculate methods
```java
public class Square {

    int side = 5;

    public int calculateArea() {
        return side * side;
    }

    public int calculatePerimeter() {
        return side * 4;
    }

    public void draw() {
        if (HighResolutionMonitor) {
            // Render a high resolution image of a square
        } else {
            // Render a normal image of a square
        }
    }

    public void rotate(int degree) {
        // Rotate the image of the square clockwise to
        // the required degree and re-render
    }
}
```

The cohesion can be improved in above by taking out the unrelated methods into their own separate class
```java
public class Square {

    int side = 5;

    public int calculateArea() {
        return side * side;
    }

    public int calculatePerimeter() {
        return side * 4;
    }
}


public class SquareUI {

    public void draw() {
        if (HighResolutionMonitor) {
            // Render a high resolution image of a square
        } else {
            // Render a normal image of a square
        }
    }

    public void rotate(int degree) {
        // Rotate the image of the square clockwise to
        // the required degree and re-render
    }
}

```

#### Coupling - level of inter-dependency between various software components

