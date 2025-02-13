### **Exploring Bean Scopes in Spring (XML-Based Configuration)**  

Bean scope in Spring defines how the container creates and manages bean instances.  

Spring provides **five scopes**, but we'll focus on the two most commonly used:  
1. **Singleton** (Default) – One instance per Spring container.  
2. **Prototype** – New instance every time it's requested.  

Other less common scopes (specific to web applications):  
- **Request** – One instance per HTTP request.  
- **Session** – One instance per HTTP session.  
- **Application** – One instance per web application lifecycle.  

---

## **1. Understanding Singleton Scope** (Default)  

### **Behavior**  
- **One shared instance** of the bean for the entire Spring container.  
- If multiple beans request the same `id`, they **get the same instance**.  

### **Example**  

#### **Step 1: Create a `StudentService` Class**  
```java
package com.example.service;

public class StudentService {
    public StudentService() {
        System.out.println("StudentService instance created!");
    }

    public void displayMessage() {
        System.out.println("Welcome to the Student Management System");
    }
}
```
- Constructor prints a message when an instance is created.  

---

#### **Step 2: Configure Singleton Scope in `spring-config.xml`**  
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Singleton Scope (Default) -->
    <bean id="studentService" class="com.example.service.StudentService" scope="singleton"/>
</beans>
```
- **`scope="singleton"` is the default**, so this is optional.  

---

#### **Step 3: Load the Bean in `MainApp.java`**  
```java
package com.example;

import com.example.service.StudentService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
    public static void main(String[] args) {
        // Load Spring Configuration
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");

        // Retrieve StudentService bean multiple times
        StudentService service1 = (StudentService) context.getBean("studentService");
        StudentService service2 = (StudentService) context.getBean("studentService");

        // Check if they refer to the same instance
        System.out.println("Are both instances same? " + (service1 == service2));

        // Close context
        ((ClassPathXmlApplicationContext) context).close();
    }
}
```

---

#### **Step 4: Run the Application**  

Output:
```
StudentService instance created!
Are both instances same? true
```
- The **bean is created only once**, and both references point to the same object.  

---

## **2. Understanding Prototype Scope**  

### **Behavior**  
- **Every time a bean is requested, a new instance is created**.  
- Useful when you need independent objects (e.g., user sessions, temporary objects).  

### **Example**  

#### **Step 1: Modify `spring-config.xml` to Use Prototype Scope**  
```xml
<bean id="studentService" class="com.example.service.StudentService" scope="prototype"/>
```

---

#### **Step 2: Run the Same `MainApp.java`**  

Output:
```
StudentService instance created!
StudentService instance created!
Are both instances same? false
```
- **Two different instances** were created when `getBean()` was called.  

---

## **3. Singleton vs. Prototype: When to Use Which?**  

| Feature         | Singleton (Default) | Prototype |
|----------------|---------------------|-----------|
| **Instance Count** | Only **one** per container | **New** instance each time |
| **Best Used For** | Shared services (e.g., logging, database connection) | Independent objects (e.g., forms, user input) |
| **Memory Usage** | Lower (one object) | Higher (multiple objects) |
| **Lifecycle** | Managed by Spring | Created and forgotten |

---

## **4. Other Scopes (For Web Applications)**  

If you're building a **Spring MVC** or **Spring Boot** application, you can also use:  
- **Request Scope** (`scope="request"`) → New bean per HTTP request.  
- **Session Scope** (`scope="session"`) → New bean per user session.  
- **Application Scope** (`scope="application"`) → One bean for the entire web app.  

---

## **Next Steps**  
Now that we understand **bean scopes**, we can explore:  
✅ **Bean Lifecycle Methods (`init-method`, `destroy-method`)**  
✅ **Autowiring Beans in XML**  
✅ **Bean Post Processors**  

Which one would you like to explore next? 🚀
