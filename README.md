# flutter_riverpod_mvvm

This project created for Flutter clean architecture

Implemented Flutter clean architecture using MVVM Riverpod and Freezed

#References material

[resocoder.com](https://resocoder.com/2019/08/27/flutter-tdd-clean-architecture-course-1-explanation-project-structure/)

[medium.com_Andreas Widijargo](https://medium.com/ruangguru/an-introduction-to-flutter-clean-architecture-ae00154001b0)

[Geeksforgeeks_RISHU_MISHRA](https://www.geeksforgeeks.org/mvvm-model-view-viewmodel-architecture-pattern-in-android/)

#Dependency

[Riverpod](https://pub.dev/packages/flutter_riverpod)

[Freezed](https://pub.dev/packages/freezed)

[DIO](https://pub.dev/packages/dio)

## Flutter Clean Architecture

**Clean Architecture provides architectural principles that help in developing maintainable and testable code by separating a software system into multiple layers. In this architecture, the software system follows a layering structure where dependencies flow from the inner layers to the outer layers.**

![Uncle Bob's Clean Architecture Proposal](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdJe5zA%2FbtsjtAwiCNo%2F45Q53e7BDExHjbnMkXO8pK%2Fimg.webp)

![dependency flow-data&call flow](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqDGl2%2Fbtsjx20kFK4%2F3Y6ntaIH7vaYNC0ZrXwSv1%2Fimg.webp)

![The Clean Architecture Diagram of Flutter](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FntBxn%2FbtsjRg3Y3r7%2FLHkxbRwFN0wJdohsXcNHO0%2Fimg.webp)




* **Presentation Layer (UI Layer)**: It includes the code related to the user interface. In this layer, you work on tasks such as screen layout, widgets, routing, animations, and other UI-related operations.


* **Presentation Logic Holders** : This layer acts as a mediator between the UI and the business logic, managing the data and state required by UI components. In Flutter, it is common to implement Presentation Logic Holders using state management libraries such as Provider, Riverpod, or MobX. like **ViewModel**

* **Domain Layer** : The domain layer is where the core business logic of the application resides. It includes essential concepts, business entities, and domain services.

* **Data Layer** : This layer handles the interaction with external data sources. It deals with tasks like communication with data sources (e.g., APIs, databases), data transformation, caching, and local storage. It provides abstractions for external data and defines interfaces for fetching and storing data.

## MVVM

**Model — View — ViewModel (MVVM)** is the industry-recognized software architecture pattern that overcomes all drawbacks of MVP and MVC design patterns. MVVM suggests separating the data presentation logic(Views or UI) from the core business logic part of the application. 

![MVVM Schema](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcfEq3G%2FbtsjS4231bw%2FC67w1sT1jkcqrNlQYKFjP1%2Fimg.png)

The separate code layers of MVVM are:

* **Model**: This layer is responsible for the abstraction of the data sources. Model and ViewModel work together to get and save the data.
* **View**: The purpose of this layer is to inform the ViewModel about the user’s action. This layer observes the ViewModel and does not contain any kind of application logic.
* **ViewModel**: It exposes those data streams which are relevant to the View. Moreover, it serves as a link between the Model and the View.

MVVM Following ways : 

1. ViewModel does not hold any kind of reference to the View.
2. Many to-1 relationships exist between View and ViewModel.
3. No triggering methods to update the View.



**In this Github repository show how to make flutter project structure using Riverpod(Statenotifier),DIO , Freezed and MVVM**



