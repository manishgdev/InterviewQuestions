## Builder Design Pattern
The Builder Design Pattern is a creational pattern used in software design to construct a complex object step by step. It allows the construction of a product in a step-by-step fashion, where the construction process can vary based on the type of product being built. The pattern separates the construction of a complex object from its representation, allowing the same construction process to create different representation

**Structure**

- **Builder Interface**:
Defines the construction steps for building the product.
- **Concrete Builder**:
    - Implements the steps defined in the Builder interface.
    - Has a method to return the final product.
- **Product**:
The complex object that is being built.
- **Director**:
Orchestrates the building process using the builder.

Define the Product
```
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

Builder Interface
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

Builder Concrete Class
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


Director (Optional)
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
Demo
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

### Method Chaining
```
final class Student {

    // instance fields
    private int id;
    private String name;
    private String address;

    // Setter Methods
    // Note that all setters method
    // return this reference
    public Student setId(int id)
    {
        this.id = id;
        return this;
    }

    public Student setName(String name)
    {
        this.name = name;
        return this;
    }

    public Student setAddress(String address)
    {
        this.address = address;
        return this;
    }

    @Override public String toString()
    {
        return "id = " + this.id + ", name   = "+ this.name + ", address = " + this.address;
    }
}

// Driver class
public class MethodChainingDemo {
    public static void main(String args[])
    {
        Student student1 = new Student();
        Student student2 = new Student();

        student1.setId(1)
            .setName(" Ram & quot;)
            .setAddress(" Noida & quot;);
        student2.setId(2)
            .setName(" Shyam & quot;)
            .setAddress(" Delhi & quot;);

        System.out.println(student1);
        System.out.println(student2);
    }
}
```

Method chaining is a useful design pattern but however if accessed concurrently, a thread may observe some fields to contain inconsistent values. Although all setter methods in above example are atomic, but calls in the method chaining can lead to inconsistent object state when the object is modified concurrently. The below example can lead us to a Student instance in an inconsistent state, for example, a student with name Ram and address Delhi. 

Output may be:

`id = 2, name = Shyam, address = Noida` \
Another inconsistent output may be \
`id = 0, name = null, address = null`

there is **Builder pattern** to ensure the thread-safety and atomicity of object creation. 
**Implementation** : In Builder pattern, we have a inner static class named Builder inside our Server class with instance fields for that class and also have a factory method to return an new instance of Builder class on every invocation. The setter methods will now return Builder class reference. We will also have a build method to return instances of Server side class, i.e. outer class. 

```java
// Java code to demonstrate Builder Pattern
// Server Side Code
final class Student {

    // final instance fields
    private final int id;
    private final String name;
    private final String address;

    public Student(Builder builder)
    {
        this.id = builder.id;
        this.name = builder.name;
        this.address = builder.address;
    }

    // Static class Builder
    public static class Builder {

        // instance fields
        private int id;
        private String name;
        private String address;

        public static Builder newInstance()
        {
            return new Builder();
        }

        private Builder() {}

        // Setter methods
        public Builder setId(int id)
        {
            this.id = id;
            return this;
        }
        public Builder setName(String name)
        {
            this.name = name;
            return this;
        }
        public Builder setAddress(String address)
        {
            this.address = address;
            return this;
        }

        // build method to deal with outer class
        // to return outer instance
        public Student build()
        {
            return new Student(this);
        }
    }

    @Override
    public String toString()
    {
        return "id = " + this.id + ", name = " + this.name + 
                            ", address = " + this.address;
    }
}

// Client Side Code
class StudentReceiver {

    // volatile student instance to ensure visibility
    // of shared reference to immutable objects
    private volatile Student student;

    public StudentReceiver()
    {
        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run()
            {
                student = Student.Builder.newInstance()
                            .setId(1)
                            .setName("Ram")
                            .setAddress("Noida")
                            .build();
            }
        });

        Thread t2 = new Thread(new Runnable() {
            @Override
            public void run()
            {
                student = Student.Builder.newInstance()
                            .setId(2)
                            .setName("Shyam")
                            .setAddress("Delhi")
                            .build();
            }
        });

        t1.start();
        t2.start();
        
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public Student getStudent()
    {
        return student;
    }
}

// Driver class
public class BuilderDemo {
    public static void main(String args[])
    {
        StudentReceiver sr = new StudentReceiver();
        System.out.println(sr.getStudent());
    }
}

```

There is small disadvantage of code duplicacy as the same attributes are present in Builder and Product class \
Builder design pattern cannot handle dynamic requests