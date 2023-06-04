# [Design Pattern](https://youtu.be/_Ac7CTHOFMg)
Design patterns are the well proved solution of commonly occurring problems in software design.

## [Singleton Design Pattern](https://youtu.be/fs6ZUcF-nuc)
The Singleton design pattern is a creational design pattern that restricts the instantiation of a class to a single object and provides global access to that instance throughout the application. This pattern ensures that only one instance of a class is created and provides a global point of access to it.
## Singleton Object
Singleton object are the object which are instantiated only once for project (jvm). If we try to get the object then we get same object again and again.


>lets create singleton pattern using java<br/>

Lazy way of creating singleton object
```java
class Example{
    private static Example ob;
    public static Example getExample(){
        if(ob==null){
            ob=new Example();
        }
        return ob;
    }
}
```

**calling the singleton object**
```java
class Main {
    public static void main(String args[]){ 
        Example ob=Example.getExample() //using the object
    }
}
```

Eager way of creating Singleton object
```java
class Example{
    private static Example ob=new Example();
    public static Example getExample(){ 
        return ob;
    }
}
```
Accessing object
```java
class Main {
    public static void main(String args[]){ 
        Example ob=Example.getExample() //using the object
    }
}
```
Note: for multithreaded environment we use syncronized block for creating singleton object.
```java
class Example{
    private static Example ob;
    public static Example getExample(){ 
        if(ob==null){
            syncronized(Example.class){ 
                if(ob==null){
                    ob=new Example(); 
                }
            }
        }
        return ob; 
    }
}
```

## [Breaking Singleton Design Pattern](https://youtu.be/zHWusHi9Nt0)
There are three ways to break singleton design pattern . Lets talk about the these way and i am also going to tell you about the solution of these problems.
### 1. Using Reflection API
With the help of relfection api we can call private constructor as well and create multiple object by calling private constructor.<br/>
lets see how we can call private constructor
```java
Constructor<Example> constructor=Example.class.getDeclaredConstructor(); 
//changing the accessibility to true
constructor.setAccessible(true);
Example example=constructor.newInstance();
```
**Solution**
we can do the soultion in two ways.
- using ENUM
```java
public enum Example{
    INSTANCE
}
```
- check the object in private constructor if the object exists then throw exception to terminate the execution.
```java
private Exmaple(){
    if(ob!=null) {
        throw new RuntimeExcepiton("you are trying to break singleton pattern") ;
    }
}
```

### 2. Using Deserialization
when we serialze and deserialze the singleton object then singleton automatically got destroyed and provide us different object.
```java
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("abc.ob")); 
oos.writeObject(ob);
System.out.println("serialization done..");
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("abc.ob")); 
Example s2 = (Example) ois.readObject();
System.out.println(s2.hashCode());
```
**Solution:**
just implement readResolve() method
```java
public Object readResolve() { 
    return ob;
}
```
### 3. Using cloning
when we clone then also we get different object.
solution
just override clone method and return the same instance.
```java
@Override
public Object clone() throws CloneNotSupportedException { 
    return samosa;
}
```
## [Factory Design Pattern](https://youtu.be/YIB3u0KYrUI)
Factory Method Design Pattern
When there is superclass and multiple subclasses and we want to get object of subclasses based on input and requirement.
Then we create factory class which takes the responsibility of creating object of class based on input.

```java
public interface Employee {
    String name();
    int salary();
    String location();
}

public abstract class Common implements Employee {
    @Override
    public String location() {
        return "Pune";
    }
}

public class WebDeveloper extends Common {
    @Override
    public String name() {
        return "Kumar";
    }

    @Override
    public int salary() {
        return 40000;
    }
}

public class AndroidDeveloper extends Common {
    @Override
    public String name() {
        return "Anil";
    }

    @Override
    public int salary() {
        return 20000;
    }
}

public class Main {
    public static void main(String[] args) {
        EmployeeFactory factoryDesign = new EmployeeFactory();
        Employee region = factoryDesign.getCountry("Web Developer");
        System.out.println(region.location());
        System.out.println(region.salary());
        System.out.println(region.name());

        System.out.println("___________");

        Employee region1 = factoryDesign.getCountry("Android Developer");
        System.out.println(region1.location());
        System.out.println(region1.salary());
        System.out.println(region1.name());
    }
}
```
### Advantages of Factory Design Pattern
1. Focus on creating object for Interface rather than implementation.
2. Loose coupling, more robust code
## [Abstract Factory Design Pattern](https://youtu.be/D0d2TsfGY2E)
Similar to Factory Pattern
It provide the concept of Factory of Factories.
```java
public interface Employee {
    String name();
    int salary();
}

public class EmployeeFactory {
    public static Employee getEmployee(EmployeeAbstractFactory employeeAbstractFactory){
        return employeeAbstractFactory.createEmployee();
    }
}

public abstract class EmployeeAbstractFactory {
    public abstract Employee createEmployee();
}

public class AndroidDevFactory extends EmployeeAbstractFactory{
    @Override
    public Employee createEmployee() {
        return new AndroidDeveloper();
    }
}

public class WebDevFactory extends EmployeeAbstractFactory{
    @Override
    public Employee createEmployee() {
        return new WebDeveloper();
    }
}

public class AndroidDeveloper implements Employee{

    @Override
    public String name() {
        return "Android Developer";
    }

    @Override
    public int salary() {
        return 50000;
    }
}

public class WebDeveloper implements Employee{
    @Override
    public String name() {
        return "Web Developer";
    }

    @Override
    public int salary() {
        return 60000;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee employee = EmployeeFactory.getEmployee(new AndroidDevFactory());
        System.out.println(employee.name());
        System.out.println(employee.salary());
        System.out.println("_____________");

        Employee employee1 = EmployeeFactory.getEmployee(new WebDevFactory());
        System.out.println(employee1.name());
        System.out.println(employee1.salary());
    }
}

```
## [Builder Design Pattern](https://youtu.be/7x8iQUv5lcM)
while creating object when object contain may attributes there are many problem exists :
1. we have to pass many arguments to create object.
2. some parameters might be optional
3. factory class takes all responsibility for creating object. If the object is
heavy then all complexity is the part of factory class.<br/>
**So in builder pattern be create object step by step and finally return final object with desired values of attributes.**
```java
public class User {
    private final String name;
    private final String userName;
    private final String email;

    public User(UserBuilder userBuilder) {
        this.name = userBuilder.name;
        this.userName = userBuilder.userName;
        this.email = userBuilder.email;
    }

    public String getName() {
        return name;
    }

    public String getUserName() {
        return userName;
    }

    public String getEmail() {
        return email;
    }

    static class UserBuilder{
        private String name;
        private String email;

        private String userName;

        public UserBuilder(){

        }

        public UserBuilder setName(String name) {
            this.name = name;
            return this;
        }

        public UserBuilder setEmail(String email) {
            this.email = email;
            return this;
        }

        public UserBuilder setUserName(String userName) {
            this.userName = userName;
            return this;
        }


        public User build() {
            User user = new User(this);
            return user;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        User user1 = new User.UserBuilder().
                setUserName("anil123").
                setName("Anil").
                setEmail("anil123@gmail.com").build();

        System.out.println("User 1- Name:"+ user1.getName());

        User user2 = new User.UserBuilder().
                setUserName("ak123").
                setName("A.K.").
                setEmail("ak123@gmail.com").build();
        System.out.println("User 2- Name:"+ user2.getName());
    }
}
```

## [Prototype Design Pattern](https://youtu.be/rriiXRdc0HQ)
The concept is to copy an existing object rather than creating a new instance from scratch. because creating new object may be costly.<br/>
This approach saves costly resources and time, especially when object creation is a heavy process.
```java
public class NetworkConnection implements Cloneable {
    private String ip;
    private String importantData;

    public void setIp(String ip) {
        this.ip = ip;
    }

    @Override
    public String toString() {
        return "NetworkConnection{" +
                "ip='" + ip + '\'' +
                ", importantData='" + importantData + '\'' +
                '}';
    }

    public void loadData() throws InterruptedException {
        this.importantData="This is important Data";
        Thread.sleep(2000);
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}


public class Main {
    public static void main(String[] args) throws CloneNotSupportedException, InterruptedException {
        NetworkConnection networkConnection = new NetworkConnection();
        networkConnection.setIp("192:12:14:111");
        networkConnection.loadData();
        System.out.println(networkConnection);

        NetworkConnection networkConnection1 = (NetworkConnection) networkConnection.clone();
        System.out.println(networkConnection1);
    }
}

```

