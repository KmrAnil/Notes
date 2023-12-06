## 1. What is stream API?
- Java 8 provides a new additional package called java.util.stream This package consists of classes, interfaces and enums to allow functional-style operations on the elements.<br/>
- We can use stream API using the java.util.stream package.<br/>
- We can use a stream to filter, collect, print and convert from one data structure to another.<br/>
- Stream APIs do not store elements. It is functional. Operations performed using stream do not modify its source.<br/>
## 2. What is the difference between Collection and Stream?

The main difference between a Collection and Stream is that Collection contains its elements but Stream doesn’t.<br/>
The stream works on a view where elements are stored by collection or array, but unlike other views, any change made on the stream does not reflect the original collection.

## 3. What does the map() function do? why you use it? 
The map() function perform map functional operation in Java. This means it can transform one type of object to others by applying a function.

For example, if you have a List of String and you want to convert that to a List of Integer, you can use map() to do so.

```java
import java.util.*;
import java.util.stream.*;
public class Main {
    public static void main(String args[]) {
         List<String> list = new ArrayList<>(Arrays.asList("1", "2", "3"));
         List<Integer> i = list.stream()
                .map(Integer::parseInt).collect(Collectors.toList());
        System.out.println(i);
    }
}
```

## 4. [What is the difference between intermediate and terminal operations on Stream? ](https://javagoal.com/intermediate-operation-in-java-8/)
The intermediate Stream operation returns another Stream, which means you can further call other methods of Stream class to compose a pipeline.

For example after calling map() or flatMap() you can still call filter() method on Stream.

On the other hand, the terminal operation produces a result other than Streams like a value or a Collection.

Once a terminal method like forEach() or collect() is called, you cannot call any other method of Stream or reuse the Stream.

## 5. What does the peek() method do in Stream API?

The peek() method of the Stream class allows us to see through a Stream pipeline.

The peek() method returns a stream consisting of the elements of the stream after performing the provided action on each element. This is useful when we want to print values after each intermediate operation

We can peek through each step and print meaningful messages on the console. It is generally used for debugging issues related to lambda expression and Stream processing.

```java
public class Main {
    public static void main(String[] args) {
        final List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

        final List<Integer> ans = list.stream()
                .filter(value -> value % 2 == 0)
                .peek(value -> System.out.println("Filtered " + value))
                .map(value -> value * 10)
                .collect(Collectors.toList());

        System.out.println(Arrays.toString(ans.toArray()));
    }
}


Output:
Filtered 2
Filtered 4
[20, 40]
```

## 6. What is predicate Interface in Java stream?

A Predicate is a functional interface that represents a function, which takes an Object and returns a boolean.

It is used in several Stream methods like filter(), which uses Predicate to filter unwanted elements.

## 7. What distinguishes the findFirst() method from the findAny() method?
The findAny() function will return any element fitting the criterion, which is particularly helpful when working with a parallel stream. In contrast, the findFirst() method will return the first element meeting the condition, i.e., Predicate. 

## 8. Can an array be converted to a stream? How?
Yes, you can use Java to transform an array into a stream. The Stream class offers a factory method to make a Stream from an array, such as Stream.of(T...), which accepts a variable parameter, meaning you may also supply an array to it, as demonstrated in the example below:
```java
String[] languages = {"Java", "Python", "JavaScript"}; 
Stream numbers = Stream.of(languages); 
numbers.forEach(System.out::println); 
```

## 9. What does the filter() method do? when you use it? 
The filter method is used to filter elements that satisfy a certain condition that is specified using a Predicate function.

## 10. What does the flatmap() function do? why you need it? 
The flatmap function is an extension of the map function. Apart from transforming one object into another, it can also flatten it.

For example, if you have a list of the list but you want to combine all elements of lists into just one list. In this case, you can use flatMap() for flattening. At the same time, you can also transform an object like you do use map() function.

## 11. What do you mean by saying Stream is lazy? 
When we say Stream is lazy, we mean that most of the methods are defined on Java .util.stream.Stream class is lazy i.e. they will not work by just including them on the Stream pipeline.

They only work when you call a terminal method on the Stream and finish as soon as they find the data they are looking for rather than scanning through the whole set of data.

## 12.What is a parallel stream? How to convert the list into a parallel stream?

- One of the prominent features of Java 8 is Java Parallel Stream. It is meant for utilizing the various cores of the processor.

- By default, all stream operations are sequential in Java unless explicitly specified as parallel. Parallel streams are created in Java in two ways.

1. Calling the parallel() method on a sequential stream.
2. Calling parallelStream() method on a collection.


- Parallel streams are useful when we have many independent tasks that can be processed simultaneously to minimize the running time of the program.

- All the Java code will usually have only one processing stream, where it is sequentially executed. But by using parallel streams, one can separate the Java code into more than one stream, which is executed in parallel on their separate cores, and the result is the combination of the individual results.


- The order in which they are executed is not in our control. Hence, it is suggested to use a parallel stream when the order of execution of individual items does not affect the final result.


Example:

parallel(): parallel() method is called on the existing sequential stream to make it parallel.

```java
public class Main {
    public static void main(String[] args) {
        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
        
        stream.parallel().forEach(System.out::println);
    }
}

Output:
3
5
4
1
2
```

parallelStream(): parallelStream() is called on Java collections like List, Set, etc to make it a parallel stream.

```java
public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

        list.parallelStream().forEach(System.out::println);
    }
}

Output:
3
1
5
4
2
```
