### **Exploring Constructor & Setter Injection in XML**  

Now that we have a basic Spring setup, let's understand how **Dependency Injection (DI)** works in XML-based configuration using **constructor injection** and **setter injection**.

---

## **1. Understanding Dependency Injection (DI)**  
Dependency Injection is a design pattern where dependencies (objects) are **injected** into a class rather than being created inside the class.  

There are **two ways** to inject dependencies using XML:  
1. **Constructor Injection** (Passing dependencies via the constructor).  
2. **Setter Injection** (Passing dependencies via setter methods).  

We'll create a `Student` class that depends on a `Course` class.

---

## **2. Implementing Constructor Injection in XML**  

### **Step 1: Define the `Course` Class**
```java
package com.example.service;

public class Course {
    private String courseName;

    public Course(String courseName) {
        this.courseName = courseName;
    }

    public String getCourseName() {
        return courseName;
    }
}
```

---

### **Step 2: Define the `Student` Class**
```java
package com.example.service;

public class Student {
    private String name;
    private int age;
    private Course course; // Dependency

    // Constructor for Dependency Injection
    public Student(String name, int age, Course course) {
        this.name = name;
        this.age = age;
        this.course = course;
    }

    public void displayStudentInfo() {
        System.out.println("Student: " + name + ", Age: " + age);
        System.out.println("Enrolled Course: " + course.getCourseName());
    }
}
```
- The `Student` class requires a `Course` object to be **injected** via the constructor.

---

### **Step 3: Configure Constructor Injection in `spring-config.xml`**
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Define Course Bean -->
    <bean id="course" class="com.example.service.Course">
        <constructor-arg value="Spring Framework Basics"/>
    </bean>

    <!-- Define Student Bean with Constructor Injection -->
    <bean id="student" class="com.example.service.Student">
        <constructor-arg value="John Doe"/>
        <constructor-arg value="20"/>
        <constructor-arg ref="course"/>
    </bean>
</beans>
```
- The `constructor-arg` tag is used to pass values to the constructor parameters.
- The `ref="course"` links the `course` bean to the `Student` bean.

---

### **Step 4: Load the Application Context in `MainApp.java`**
```java
package com.example;

import com.example.service.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
    public static void main(String[] args) {
        // Load Spring Configuration
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");

        // Get Student Bean
        Student student = (Student) context.getBean("student");

        // Display Student Information
        student.displayStudentInfo();

        // Close context
        ((ClassPathXmlApplicationContext) context).close();
    }
}
```
---

### **Step 5: Run the Application**
When you run `MainApp.java`, the output will be:
```
Student: John Doe, Age: 20
Enrolled Course: Spring Framework Basics
```

---

## **3. Implementing Setter Injection in XML**  

Instead of using constructor arguments, **setter injection** uses setter methods to inject dependencies.

---

### **Step 1: Modify the `Course` and `Student` Classes**  
#### **Course.java (No Change)**
```java
package com.example.service;

public class Course {
    private String courseName;

    public String getCourseName() {
        return courseName;
    }

    public void setCourseName(String courseName) {
        this.courseName = courseName;
    }
}
```
- Added a `setCourseName()` method for setter injection.

#### **Student.java (Updated)**
```java
package com.example.service;

public class Student {
    private String name;
    private int age;
    private Course course;

    // Setter Methods for Injection
    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setCourse(Course course) {
        this.course = course;
    }

    public void displayStudentInfo() {
        System.out.println("Student: " + name + ", Age: " + age);
        System.out.println("Enrolled Course: " + course.getCourseName());
    }
}
```
- Added `setName()`, `setAge()`, and `setCourse()` methods.

---

### **Step 2: Configure Setter Injection in `spring-config.xml`**
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Define Course Bean -->
    <bean id="course" class="com.example.service.Course">
        <property name="courseName" value="Spring Framework Basics"/>
    </bean>

    <!-- Define Student Bean with Setter Injection -->
    <bean id="student" class="com.example.service.Student">
        <property name="name" value="John Doe"/>
        <property name="age" value="20"/>
        <property name="course" ref="course"/>
    </bean>
</beans>
```
- The `<property>` tag is used instead of `<constructor-arg>`.
- The `ref="course"` links the `Course` bean.

---

### **Step 3: Run the Same `MainApp.java`**
Since the XML configuration is updated, running `MainApp.java` will give the **same output**:
```
Student: John Doe, Age: 20
Enrolled Course: Spring Framework Basics
```

---

## **4. Constructor vs. Setter Injection: When to Use What?**
| Injection Type | Use Case |
|---------------|---------|
| **Constructor Injection** | When dependencies are **mandatory** and should not change. Best for immutable fields. |
| **Setter Injection** | When dependencies are **optional** or can be changed at runtime. Best for configurations. |

---

## **Next Steps**
Now that you understand **constructor & setter injection**, we can explore:  
✅ **Bean Scopes (`singleton`, `prototype`, etc.)**  
✅ **Bean Lifecycle Methods (`init-method`, `destroy-method`)**  
✅ **Autowiring Beans in XML**  

