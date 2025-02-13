### **Exploring Bean Lifecycle Methods in Spring (XML-Based Configuration)**  

Spring provides **bean lifecycle methods** to manage initialization and destruction of beans. These methods help perform tasks like setting up resources (e.g., database connections) and cleaning up before shutdown.  

---

## **1. Understanding the Spring Bean Lifecycle**  

A bean goes through the following **lifecycle stages**:  

1️⃣ **Bean Instantiation** – Spring creates an instance of the bean.  
2️⃣ **Dependency Injection** – Dependencies are injected into the bean.  
3️⃣ **Post-Initialization Methods** – Custom logic can run after the bean is created.  
4️⃣ **Bean Ready for Use** – The bean is now available in the application.  
5️⃣ **Pre-Destroy Methods** – Cleanup tasks before the bean is removed.  
6️⃣ **Bean Destruction** – The bean is removed from the Spring container.  

Spring provides **two approaches** to define lifecycle methods:  
1. **Using XML Configuration (`init-method`, `destroy-method`)**  
2. **Implementing `InitializingBean` and `DisposableBean` Interfaces**  

---

## **2. Using XML-Based Lifecycle Methods (`init-method` & `destroy-method`)**  

Spring allows defining lifecycle methods directly in the `spring-config.xml` file.

---

### **Step 1: Create a `StudentService` Class**
```java
package com.example.service;

public class StudentService {
    private String studentName;

    public void setStudentName(String studentName) {
        this.studentName = studentName;
    }

    // Initialization method
    public void init() {
        System.out.println("StudentService: Inside init() method - Initializing bean");
    }

    // Business method
    public void displayStudentInfo() {
        System.out.println("Student Name: " + studentName);
    }

    // Destroy method
    public void destroy() {
        System.out.println("StudentService: Inside destroy() method - Cleaning up resources");
    }
}
```
- The `init()` method will be called **after** bean creation.  
- The `destroy()` method will be called **before** the bean is removed from the container.  

---

### **Step 2: Configure Lifecycle Methods in `spring-config.xml`**
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="studentService" class="com.example.service.StudentService" 
          init-method="init" destroy-method="destroy">
        <property name="studentName" value="John Doe"/>
    </bean>
</beans>
```
- **`init-method="init"`** → Calls the `init()` method **after** bean creation.  
- **`destroy-method="destroy"`** → Calls the `destroy()` method **before** the bean is removed.  

---

### **Step 3: Load and Destroy Bean in `MainApp.java`**
```java
package com.example;

import com.example.service.StudentService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
    public static void main(String[] args) {
        // Load Spring Configuration
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");

        // Retrieve the bean
        StudentService service = (StudentService) context.getBean("studentService");

        // Call method to display student information
        service.displayStudentInfo();

        // Close the context to trigger destroy method
        context.close();
    }
}
```

---

### **Step 4: Run the Application**  

#### **Expected Output**
```
StudentService: Inside init() method - Initializing bean
Student Name: John Doe
StudentService: Inside destroy() method - Cleaning up resources
```
- The **`init()` method runs** after bean creation.  
- The **`destroy()` method runs** when the context is closed.  

---

## **3. Using `InitializingBean` and `DisposableBean` Interfaces**  

Instead of using XML, you can **implement Spring’s lifecycle interfaces**:

---

### **Step 1: Modify `StudentService.java`**
```java
package com.example.service;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class StudentService implements InitializingBean, DisposableBean {
    private String studentName;

    public void setStudentName(String studentName) {
        this.studentName = studentName;
    }

    // Called after properties are set
    @Override
    public void afterPropertiesSet() {
        System.out.println("StudentService: Inside afterPropertiesSet() - Initializing bean");
    }

    // Called before the bean is destroyed
    @Override
    public void destroy() {
        System.out.println("StudentService: Inside destroy() - Cleaning up resources");
    }

    public void displayStudentInfo() {
        System.out.println("Student Name: " + studentName);
    }
}
```
- **`afterPropertiesSet()`** (like `init()`) → Called **after** bean creation.  
- **`destroy()`** → Called **before** the bean is removed.  

---

### **Step 2: Modify `spring-config.xml` (No `init-method` or `destroy-method` Needed)**
```xml
<bean id="studentService" class="com.example.service.StudentService">
    <property name="studentName" value="John Doe"/>
</bean>
```

---

### **Step 3: Run the Same `MainApp.java`**
#### **Expected Output**
```
StudentService: Inside afterPropertiesSet() - Initializing bean
Student Name: John Doe
StudentService: Inside destroy() - Cleaning up resources
```
- **No need to specify `init-method` or `destroy-method` in XML.**  

---

## **4. Choosing the Best Lifecycle Approach**
| Approach | Pros | Cons |
|----------|------|------|
| **XML (`init-method`, `destroy-method`)** | No need to modify Java classes | Less flexible if using annotations |
| **Interfaces (`InitializingBean`, `DisposableBean`)** | Keeps configuration in Java code | Tightly couples Spring with the class |

---

## **5. Alternative: Using `@PostConstruct` and `@PreDestroy` (Java Annotations)**
If you use **annotations**, Java provides:  
```java
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

public class StudentService {
    @PostConstruct
    public void init() {
        System.out.println("Inside @PostConstruct - Initializing bean");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("Inside @PreDestroy - Cleaning up resources");
    }
}
```
- Works **without implementing Spring interfaces**.  
- Requires Java EE (`javax.annotation`).  
- Works in **Spring Boot and Spring Framework 5+**.  

---

## **Next Steps**
Now that we understand **bean lifecycle methods**, we can explore:  
✅ **Autowiring in XML (`autowire="byType"`, `autowire="byName"`)**  
✅ **Bean Post Processors (`BeanPostProcessor`)**  
✅ **Factory Beans**  

Which one should we explore next? 🚀
