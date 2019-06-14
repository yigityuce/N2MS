
- [Introduction](#introduction)
- [Install](#install)
  - [Maven](#maven)
- [Frequently Used](#frequently-used)
  - [@Builder](#builder)
  - [@Getter and @Setter](#getter-and-setter)
  - [@ToString](#tostring)
  - [@NoArgsConstructor](#noargsconstructor)
  - [@AllArgsConstructor](#allargsconstructor)


# Introduction
* This document written for **Lombok version 1.18.8**
* It is an helper library.
* Provides very useful annotations.

# Install
## Maven
* Add dependency to your pom.xml
```xml
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.8</version>
  <scope>provided</scope>
</dependency>
```

# Frequently Used

```java
package com.yigityuce.javatutorials.library;

import lombok.*;

@Builder
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @Getter @Setter
    private String name;

    @Getter @Setter
    private String email;
}

```

## @Builder
* Adds builder method that returns chainable builder class instance.
* Generates new class instance via builder design patter.
* [https://projectlombok.org/features/Builder](https://projectlombok.org/features/Builder)

```java
import com.yigityuce.javatutorials.library.User;

public class App
{
    public static void main( String[] args )
    {
        User user = User.builder()
                .name("Yigit Yuce")
                .email("ygtyce@gmail.com")
                .build();
    }
}
```



## @Getter and @Setter
* Adds getter/setter automatically to your variable.
* Getter/Setter method's access modifier type can be modified.
* [https://projectlombok.org/features/GetterSetter](https://projectlombok.org/features/GetterSetter)

```java
import com.yigityuce.javatutorials.library.User;

public class App
{
    public static void main( String[] args )
    {
        User user = User.builder()
                .name("Yigit Yuce")
                .email("ygtyce@gmail.com")
                .build();

        System.out.println(user.getEmail());
        user.setName("Yigit");
    }
}
```

## @ToString
* Overrides object's toString method to print object's variable types and values.
* [https://projectlombok.org/features/ToString](https://projectlombok.org/features/ToString)

```java
import com.yigityuce.javatutorials.library.User;

public class App
{
    public static void main( String[] args )
    {
        User user = User.builder()
                .name("Yigit Yuce")
                .email("ygtyce@gmail.com")
                .build();
        System.out.println(user);
    }
}
```

## @NoArgsConstructor
* Generates constructor with no arguments.
* [https://projectlombok.org/features/constructor](https://projectlombok.org/features/constructor)

```java
import com.yigityuce.javatutorials.library.User;

public class App
{
    public static void main( String[] args )
    {
        // valid beacuse of @NoArgsConstructor
        User user = new User(); 
        System.out.println(user);
    }
}
```

## @AllArgsConstructor
* Generates constructor with all arguments.
* [https://projectlombok.org/features/constructor](https://projectlombok.org/features/constructor)

```java
import com.yigityuce.javatutorials.library.User;

public class App
{
    public static void main( String[] args )
    {
        // valid beacuse of @AllArgsConstructor
        User user = new User("Yuce Yigit", "ygtyce@gmail.com");
        System.out.println(user);
    }
}
```