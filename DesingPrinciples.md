# SOLID

## S - Single Responsibility Principle
Every software component (can be class, method or module) should have one and only one responsibility
Every software component (can be class, method or module) should have one and only one reason to change
#### Cohesion
"Cohesion is the degree to which the various parts of a software component are related"\
**Aim for High Cohesion** 

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

#### Coupling
“Coupling is defined as the level of interdependency between various software components”\
**Aim for Loose Coupling**

Example of tight coupling - save method is tighly coupled with Student Class
```
public class Student {

    private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        new StudentRepository().save(this);
    }

    public String getStudentId() {
        return studentId;
    }

    public void setStudentId(String studentId) {
        this.studentId = studentId;
    }

    public void save(Student student) {
        // Serialize object into a string representation
        String objectStr = MyUtils.serializeIntoAString(student);
        Connection connection = null;
        Statement stmt = null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MyDB", "root", "password");
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}

```

**Loose coupling helps attains Single Responsibility**\
above can be written into two separate classes
```java
public class Student {

    private String studentId;
    private Date studentDOB;
    private String address;

    public void save() {
        new StudentRepository().save(this);
    }

    public String getStudentId() {
        return studentId;
    }

    public void setStudentId(String studentId) {
        this.studentId = studentId;
    }

    ...
    ...
    ......
}

public class StudentRepository {
    public void save(Student student) {
        // Serialize object into a string representation
        String objectStr = MyUtils.serializeIntoAString(student);
        Connection connection = null;
        Statement stmt = null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/MyDB", "root", "password");
            stmt = connection.createStatement();
            stmt.execute("INSERT INTO STUDENT VALUES (" + objectStr + ")");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

## O - Open Closed Principle
Software components should be Closed for modification but open for extension

**Example** Closed for modification
let's take an example of Insurance company which started with Health Insurance and gives discount to its loyal customers based on Customer profile
```java
public class InsurancePremiumDiscountCalculator {

    public int calculatePremiumDiscountPercent(HealthInsuranceCustomerProfile customer) {
        if (customer.isLoyalCustomer()) {
            return 20;
        }
        return 0;
    }

}

public class HealthInsuranceCustomerProfile {
    public boolean isLoyalCustomer() {
        return true; // or false
    }
}

```
Going forward some years it adds Vehicle Insurance also to its business, now to give discount to its customer it will have to modify the `calculatePremiumDiscountPercent` method or overload the `calculatePremiumDiscountPercent` method

```java
public class InsurancePremiumDiscountCalculator {

    public int calculatePremiumDiscountPercent(HealthInsuranceCustomerProfile customer) {
        if (customer.isLoyalCustomer()) {
            return 20;
        }
        return 0;
    }

    public int calculatePremiumDiscountPercent(VehicleInsuranceCustomerProfile customer) {
        if (customer.isLoyalCustomer()) {
            return 20;
        }
        return 0;
    }

}

public class HealthInsuranceCustomerProfile {
    public boolean isLoyalCustomer() {
        return true; // or false
    }
}

public class VehicleInsuranceCustomerProfile {
    public boolean isLoyalCustomer() {
        return true; // or false
    }
}

```

Above can be solved by Creating a `CustomerProfile` Interface which will be implemented by `HealthInsuranceCustomerProfile` & `VehicleInsuranceCustomerProfile` and the `calculatePremiumDiscountPercent` method in `InsurancePremiumDiscountCalculator` class takes CustomerProfile argument\

![image](https://github.com/manishgdev/InterviewQuestions/assets/10989007/7283c48b-4433-4d13-b408-c4ec20ecdd7b)



