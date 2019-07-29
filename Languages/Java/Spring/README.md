
# Introduction
* This document written for **Spring**
* **Spring Framework** is a mature, powerful and highly flexible framework focused on building web applications in Java.

# Architecture 
* Spring is modular, allowing you to pick and choose which modules are applicable to you, without having to bring in the rest.
* The Spring Framework provides about 20 modules which can be used based on an application requirement.

![spring_architecture.png](./spring_architecture.png)

## Core Container
* **Core**
  * provides the fundamental parts of the framework, including the **IoC** and **Dependency Injection** features.
* **Bean**
  * provides **BeanFactory**, which is a sophisticated implementation of the factory pattern.
* **Context**
  * builds on the solid base provided by the Core and Beans modules and it is a medium to access any objects defined and configured. 
  * The **ApplicationContext** interface is the focal point of the Context module.
* SpEL
  * provides a powerful expression language for querying and manipulating an object graph at runtime.


## Data Access/Integration

* **JDBC**
  * provides a JDBC-abstraction layer that removes the need for tedious JDBC related coding.
* **ORM**
  * provides integration layers for popular object-relational mapping APIs, including JPA, JDO, **Hibernate**.
* **OXM**
  * provides an abstraction layer that supports Object/XML mapping implementations for JAXB, Castor, XMLBeans, JiBX and XStream.
* **JMS**
  * Java Messaging Service
  * contains features for producing and consuming messages.
* **Transactions**
  * supports programmatic and declarative transaction management for classes that implement special interfaces and for all your POJOs.

## Web
* **Web**
  * provides basic web-oriented integration features such as multipart file-upload functionality and the initialization of the IoC container using servlet listeners and a web-oriented application context.
* **Web-MVC**
  * contains Spring's Model-View-Controller (MVC) implementation for web applications.
* **Web-Socket**
  * provides support for WebSocket-based, two-way communication between the client and the server in web applications.
* **Web-Portlet**
  * provides the MVC implementation to be used in a portlet environment and mirrors the functionality of Web-Servlet module.

## Miscellaneous
* **AOP**
  * provides an **aspect-oriented programming** implementation allowing you to define method-interceptors and pointcuts to cleanly decouple code that implements functionality that should be separated.
* **Aspects**
  * provides integration with AspectJ, which is again a powerful and mature AOP framework.
* **Instrumentation**
  * provides class instrumentation support and class loader implementations to be used in certain application servers.
* **Messaging**
  * provides support for STOMP as the WebSocket sub-protocol to use in applications. 
  * It also supports an annotation programming model for routing and processing STOMP messages from WebSocket clients.
* **Test**
  * supports the testing of Spring components with JUnit or TestNG frameworks.