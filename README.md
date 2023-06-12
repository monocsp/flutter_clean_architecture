# flutter_riverpod_mvvm

This project created for Flutter clean architecture

Implemented Flutter clean architecture using MVVM Riverpod and Freezed

#References material

[resocoder.com](https://resocoder.com/2019/08/27/flutter-tdd-clean-architecture-course-1-explanation-project-structure/)

#Dependency

[Riverpod](https://pub.dev/packages/flutter_riverpod)

[Freezed](https://pub.dev/packages/freezed)

## Flutter Clean Architecture

**Clean Architecture provides architectural principles that help in developing maintainable and testable code by separating a software system into multiple layers. In this architecture, the software system follows a layering structure where dependencies flow from the inner layers to the outer layers.**

![Uncle Bob's Clean Architecture Proposal](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdJe5zA%2FbtsjtAwiCNo%2F45Q53e7BDExHjbnMkXO8pK%2Fimg.webp)

![dependency flow-data&call flow](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqDGl2%2Fbtsjx20kFK4%2F3Y6ntaIH7vaYNC0ZrXwSv1%2Fimg.webp)


* **Presentation Layer (UI Layer)**: It includes the code related to the user interface. In this layer, you work on tasks such as screen layout, widgets, routing, animations, and other UI-related operations.


* **Presentation Logic Holders** : This layer acts as a mediator between the UI and the business logic, managing the data and state required by UI components. In Flutter, it is common to implement Presentation Logic Holders using state management libraries such as Provider, Riverpod, or MobX. like **ViewModel**

* **Domain Layer** : The domain layer is where the core business logic of the application resides. It includes essential concepts, business entities, and domain services.

* **Data Layer** : This layer handles the interaction with external data sources. It deals with tasks like communication with data sources (e.g., APIs, databases), data transformation, caching, and local storage. It provides abstractions for external data and defines interfaces for fetching and storing data.

## MVVM



