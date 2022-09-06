---
title: "java 基础知识点"
date: 2020-02-22T13:05:19+08:00
#series: ["Go Web Dev"]
categories: ["JavaBase"]
tags: ["java基础", "java面试", "深拷贝,浅拷贝"]
---
# java基础
- 深拷贝和浅拷贝区别了解吗？什么是引用拷贝
- 泛型
- 注解
- 序列化和反序列化
- 语法糖
- java只有值传递



## 1.深拷贝和浅拷贝区别了解吗？什么是引用拷贝？
- **浅拷贝**: 浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
- **深拷贝**: 深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。  
  **浅拷贝**  
  实例代码如下:
  clone() 方法的实现很简单，直接调用的是父类 Object 的 clone() 方法
```java
    public class Address implements Cloneable{
        private String name;
        // 省略构造函数、Getter&Setter方法
        @Override
        public Address clone() {
            try {
                return (Address) super.clone();
            } catch (CloneNotSupportedException e) {
                throw new AssertionError();
            }
        }
    }
    
    public class Person implements Cloneable {
        private Address address;
        // 省略构造函数、Getter&Setter方法
        @Override
        public Person clone() {
            try {
                Person person = (Person) super.clone();
                return person;
            } catch (CloneNotSupportedException e) {
                throw new AssertionError();
            }
        }
    }

```
  测试
```bash
        Person person1 = new Person(new Address("武汉"));
        Person person1Copy = person1.clone();
        // true
        System.out.println(person1.getAddress() == person1Copy.getAddress());    
``` 
   从输出结构就可以看出， person1 的克隆对象和 person1 使用的仍然是同一个 Address 对象。  
   **深拷贝**  
   这里我们简单对 Person 类的 clone() 方法进行修改，连带着要把 Person 对象内部的 Address 对象一起复制  
```java
    public class Person implements Cloneable {
    private Address address;
    // 省略构造函数、Getter&Setter方法
    @Override
    public Person clone() {
        try {
            Person person = (Person) super.clone();
            person.setAddress(person.getAddress().clone());
            return person;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
    }
```
测试 ：
```bash
    Person person1 = new Person(new Address("武汉"));
    Person person1Copy = person1.clone();
    // false
    System.out.println(person1.getAddress() == person1Copy.getAddress());
  
```
从输出结构就可以看出，虽然 person1 的克隆对象和 person1 包含的 Address 对象已经是不同的了
   **拷贝示意图**
![cloneable](../assert/cloneable.png)

## 2.什么是泛型？有什么作用？
**Java 泛型（Generics）** 是 JDK 5 中引入的一个新特性。使用泛型参数，可以增强代码的可读性以及稳定性。  
### 2.1 泛型的使用方式有哪几种？
泛型一般有三种使用方式: **泛型类**、**泛型接口**、**泛型方法**。  
1.泛型类：
```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{

    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
}
```
2.泛型接口
```java
public interface Generator<R,K,T> {
    public R method(T t,K t);
}
```
实现泛型接口，不指定类型：
```java
class GeneratorImpl<R,K,T> implements Generator<R,K,T>{
    @Override
    public T method() {
        return null;
    }
}
```
实现泛型接口，指定类型：
```java
public class GeneratorImpl<R, T, K> implements Generator<String,Integer,Integer> {


    @Override
    public String apply(Integer integer, Integer integer2) {
        return null;
    }
}
//
public class Generator1Impl implements Generator<String,Integer,Integer> {


    @Override
    public String apply(Integer integer, Integer integer2) {
        return null;
    }
}
```
3.泛型方法 
```java
public class Generator{
    public static < E > void printArray( E[] inputArray )
    {
        for ( E element : inputArray ){
            System.out.printf( "%s ", element );
        }
        System.out.println();
    }
}
```
使用：
```bash
// 创建不同类型数组: Integer, Double 和 Character
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray  );
printArray( stringArray  );
```
>注意: public static < E > void printArray( E[] inputArray ) 一般被称为静态泛型方法;
>在 java 中泛型只是一个占位符，必须在传递类型后才能使用。类在实例化时才能真正的传递类型参数，由于静态方法的加载先于类的实例化，
>也就是说类中的泛型还没有传递真正的类型参数，静态的方法的加载就已经完成了，
>所以静态泛型方法是没有办法使用类上声明的泛型的。只能使用自己声明的 <E>

## 3.何谓注解
Annotation （注解） 是 Java5 开始引入的新特性，可以看作是一种特殊的注释，主要用于修饰类、方法或者变量，提供某些信息供程序在编译或者运行时使用   
- 注解本质是一个继承了Annotation 的特殊接口：
- 注解只有被解析之后才会生效，常见的解析方法有两种：
>编译期直接扫描 ：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用@Override 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
>运行期通过反射处理 ：像框架中自带的注解(比如 Spring 框架的 @Value 、@Component)都是通过反射来进行处理的。

## 4.序列化和反序列化
如果我们需要持久化 Java 对象比如将 Java 对象保存在文件中，或者在网络传输 Java 对象，这些场景都需要用到序列化。  
简单来说:
- **序列化**: 将数据结构或对象转换成二进制字节流的过程
- **序列化**: 将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程  
 > 序列化（serialization）在计算机科学的数据处理中，是指将数据结构或对象状态转换成可取用格式（例如存成文件，
 > 存于缓冲，或经由网络中发送），以留待后续在相同或另一台计算机环境中，能恢复原先状态的过程。
 > 依照序列化格式重新获取字节的结果时，可以利用它来产生与原始对象相同语义的副本。
 > 对于许多对象，像是使用大量引用的复杂对象，这种序列化重建的过程并不容易。
 > 面向对象中的对象序列化，并不概括之前原始对象所关系的函数。这种过程也称为对象编组（marshalling）。
 > 从一系列字节提取数据结构的反向操作，是反序列化（也称为解编组、deserialization、unmarshalling）。  
 ![serialization](../assert/serialization.png)  
- 如果有些字段不想进行序列化怎么办？  
  对于不想进行序列化的变量，使用 transient 关键字修饰。
transient 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，
被 transient 修饰的变量值不会被持久化和恢复。
### 关于 transient 还有几点注意：
- transient 只能修饰变量，不能修饰类和方法。
- transient 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 int 类型，那么反序列后结果就是 0。
- static 变量因为不属于任何对象(Object)，所以无论有没有 transient 关键字修饰，均不会被序列化
 
## 5.语法糖
**语法糖（Syntactic sugar）** 代指的是编程语言为了方便程序员开发程序而设计的一种特殊语法，这种语法对编程语言的功能并没有影响。
实现相同的功能，基于语法糖写出来的代码往往更简单简洁且更易阅读。  
> JVM 其实并不能识别语法糖，Java 语法糖要想被正确执行，需要先通过编译器进行解糖，
> 也就是在程序编译阶段将其转换成 JVM 认识的基本语法。这也侧面说明， 
> Java 中真正支持语法糖的是 Java 编译器而不是 JVM。
> 如果你去看com.sun.tools.javac.main.JavaCompiler的源码，
> 你会发现在compile()中有一个步骤就是调用desugar()，这个方法就是负责解语法糖的实现的。
- java 中最常用的语法糖主要有泛型、自动拆装箱、变长参数、枚举、内部类、增强 for 循环、try-with-resources 语法、lambda 表达式等。

## 6.值传递和引用传递
**值传递**: 方法接收的是实参值的拷贝，不会改变原本的实参值。（创建一个副本）
**引用传递**: 方法接收的直接是实参所引用的**对象在堆中的地址**。（不会创建副本）
> 对于java来说方法接受的参数是基本数据类型毋庸置疑实际接收是基本变量值的一个拷贝值，不会影响值的本身。
> 当方法接受的是引用类型时，传递的不是堆中对象的实际地址。而是对象在堆中地址的一个拷贝值，他们指向同一个对象。
> 修改会改变对象本身但是不影响实际地址值。
