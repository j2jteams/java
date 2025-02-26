We'll now explore **Java-based configuration** step by step, comparing it with **XML-based and annotation-based configurations**.  

### **1. What is Java-Based Configuration?**
Java-based configuration uses **Java classes and `@Configuration` annotations** instead of XML or component scanning to define beans.  

### **2. Advantages of Java-Based Configuration**
✅ **Type-Safety** – No XML parsing errors.  
✅ **Refactoring Friendly** – Easy to rename and manage beans.  
✅ **Better Control** – Explicit bean definitions.  
✅ **Encapsulation** – Configurations are modular and reusable.  

---

## **Step 1: Creating Beans (XML vs Annotations vs Java-Based)**
### **A) XML-Based Configuration**
```xml
<beans xmlns="http://www.springframework.org/schema/beans">
    <bean id="studentService" class="com.example.StudentService"/>
</beans>
```

### **B) Annotation-Based Configuration**
```java
@Component
public class StudentService {
    public void display() {
        System.out.println("Student Service Running...");
    }
}
```

### **C) Java-Based Configuration (`@Bean` Approach)**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    @Bean
    public StudentService studentService() {
        return new StudentService();
    }
}
```

---

## **Step 2: Initializing Spring Context**
### **A) XML-Based Context Initialization**
```java
ApplicationContext context = new ClassPathXmlApplicationContext("app-config.xml");
StudentService studentService = context.getBean(StudentService.class);
```

### **B) Annotation-Based Context Initialization**
```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
StudentService studentService = context.getBean(StudentService.class);
```

---

## **Step 3: Dependency Injection (Constructor & Setter)**
Let's assume `StudentService` depends on `StudentRepository`.

### **A) XML-Based Configuration**
```xml
<bean id="studentRepository" class="com.example.StudentRepository"/>
<bean id="studentService" class="com.example.StudentService">
    <constructor-arg ref="studentRepository"/>
</bean>
```

### **B) Annotation-Based (`@Autowired`)**
```java
@Component
public class StudentService {
    private final StudentRepository repository;

    @Autowired
    public StudentService(StudentRepository repository) {
        this.repository = repository;
    }
}
```

### **C) Java-Based (`@Bean` with Explicit Wiring)**
```java
@Configuration
public class AppConfig {
    @Bean
    public StudentRepository studentRepository() {
        return new StudentRepository();
    }

    @Bean
    public StudentService studentService(StudentRepository repository) {
        return new StudentService(repository);
    }
}
```

---

## **Next Steps**
Would you like to explore:  
1️⃣ **Bean Scopes (`singleton`, `prototype`)?**  
2️⃣ **Bean Lifecycle (`@PostConstruct`, `@PreDestroy`)?**  
3️⃣ **Profiles and Property Placeholders in Java-Based Config?**
