# Dependency Injection

## SOLID Principles

1. **S**olid Responsibility Principle –  *one class - one role*.

   > Just because you can, doesn't mean you should.

2. **O**pen Closed Principle – *open for extension, closed for modification.* 

   - Ability to extend classes behavior without modification.
   - Use private variable with getters and setters – ONLY when you need them.
   - Use abstract base classes.

   > Open chest surgery is not needed when just putting on a coat.

3. **L**iskov Substitution Principle – *objects in a program should be replaceable with instances of their subtypes WITHOUT altering the correctness of the program.*

   - Violations will often fail the `IS A` test.

   > If it looks like a duck, quacks like a duck, but needs batteries, you probably have the wrong abstraction

4. **I**nterface Segregation Principle – *make client-specific, fine grained interfaces.*
   - Always better than one *general purpose interface*.
   - Keep focused components and the inter-component dependencies minimized.
5. **D**ependency Inversion Principle – *abstractions should not depend upon details, rather the details should depend on abstractions*.
   - Higher level and lower level object depend on the same abstract interaction.
   - This is different from *Dependency Injection*.

## Dependency Injection for 5 year kids

> When you go and get things out of the refrigerator for yourself, you can cause problems. You might leave the door open, you might get something Mommy or Daddy doesn't want you to have. You might even be looking for something we don't even have or which has expired. What you should be doing is stating a need, "I need something to drink with lunch," and then we will make sure you have something when you sit down to eat.

### Spring Context

The Spring Context is all the stuff that is going to be around your application when Spring starts up. That means all the configuration files, annotated dependencies, and all of these are going to be brought as objects and be available in Spring Context.

Let us create a controller `MyController.java` in `sfg-di` project.

```java
package io.github.sfgdi.controllers;

import org.springframework.stereotype.Controller;

@Controller
public class MyController {
    public String sayHello() {
        System.out.println("hello World");
        return "Hi Folks !";
    }
}
```

Let us now use Context in `SfgDiApplication` class.

```java
package io.github.sfgdi;

import io.github.sfgdi.controllers.MyController;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class SfgDiApplication {

	public static void main(String[] args) {
            // SpringApplication.run(SfgDiApplication.class, args);
            ApplicationContext ctx = SpringApplication.run(SfgDiApplication.class, args);

            MyController myController = (MyController) ctx.getBean("myController");
            String greeting = myController.sayHello();
            System.out.println(greeting);
	}
}
```

The `ApplicationContext` gets the instance of our Spring-Boot Application. Here, you can see that we did not instantiate `MyController` class. The context `ctx` object instantiates the `MyController` class for us.

On running the `SfgDiApplication`,  we see the following output:

```powershell
2021-03-20 20:35:27.594  INFO 12452 --- [main] io.github.sfgdi.SfgDiApplication : Started SfgDiApplication in 0.634 seconds (JVM running for 1.258)
hello World
Hi Folks !
```

> One of the fundamental principles of **Dependency Injection**, is let the framework do all the management of it.

### Dependency Injection

- Dependency Injection is where a needed dependency is injected by another object.
- The class being injected has no responsibility in instantiating the object being injected.

### Types of Dependency Inversion

1. By class properties (*least preferred*)
2. By setters
3. By constructors (*most preferred*)

### Concrete Classes vs Interfaces

- Dependency Injection can be done with Concrete Classes (*avoid at all times*) or with Interfaces (*highly preferred*).
- Generally DI with Concrete Classes should be avoided.
- Dependency Injection via Interfaces is highly preferred.
  - Allows runtime to decide implementation to inject.
  - Follows Interface Segregation Principle of SOLID.
  - Makes your code more testable.

### Injection of Control (IoC)

- Dependencies are not predetermined.
- It is a technique to allow dependencies to be injected at runtime.

> One important characteristic of a framework is that the methods defined by the user to tailor the framework will often be called from within the framework itself, rather than from the user's application code. 
>
> The framework often plays the role of the main program in coordinating and sequencing application activity. 
>
> This inversion of control gives the frameworks the power to serve as extensible skeletons. 
>
> The methods supplied by the user tailor the generic algorithms defined in the framework for a particular application.

| Inversion of Control                                        | Dependency Injection                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| Runtime environment of your code.                           | It refers much to the composition of your classes.           |
| It means Spring Framework's Inversion of Control container. | It means you compose your classes with Dependency Injection. |
|                                                             | Spring is in control of the injection of dependencies.       |

