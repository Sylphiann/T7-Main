# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
**Q1**: In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or **trait** in Rust) in this BambangShop case, or a single Model **struct** is enough?

**A1**: Recalling the whole purpose of implementing a Design Pattern twoards out whole codebase is to improve Readability, Maintainability and Extensibility, and in this case, our main focus here is Extensibility. Therefore, using interface (or trait) should be necessary if we want to develop BambangShop even further.
Let's just say if there were another case where there would be another unique case of Subscriber which notification would behave differently than the original subscriber behavior, said implementation would be much easier to be done if a trait was used in the creation of the original subscriber implementation.

**Q2**: **id** in **Program** and **url** in **Subscriber** is intended to be unique. Explain based on your understanding, is using **Vec** (list) sufficient or using **DashMap** (map/dictionary) like we currently use is necessary for this case?

**A2**: If both of those attributes are unique, then it should be necessary to use DashMap in that case, since it's possible to have duplicates in a Vec (or List). On the other hand, if we treat those unique attributes as a key to a DashMap, duplcate insertion would raise an Error, which can be handled to prevent an actual duplication.

**Q3**: When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

**A3**: I haven't done a further research on how compiler constraints on Rust works. But if my understanding is right, it should not be necessary to use DashMap, even more since it's an external library. Dealing with mutability might work if we would work with a Singleton pattern.

#### Reflection Publisher-2
**Q1**: In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

**A1**: It goes around the concept of achieving low coupling with separation of concern (remember Single Responsibility Principle), that is with a reason to improve our code's maintainability, Extensibility and Readability. Further explanation would be like this; Model should only focus on how its instance behave without any special interference that wouldn't happen by it's original nature, like what attributes it had and what type each of those attributes are, to the more complex behavior like value restrictions and special value formatting. The rest of the logic, that is the *Business* logic of those instances should be handled on the "Service" and "Repository" part of the application, which is the place on how those instances were to be handled within the application and stored to the database.

**Q2**: What happens if we only use the Model? Explain your imagination on how the interactions between each model (**Program**, **Subscriber**, **Notification**) affect the code complexity for each model?

**A2**: Unecessarily high-coupling, that is. I can't imagine how bloated the Model section of the codebase would be. There would be way less files in the directory while the model files would be full of all kind of logic mixed in together on the model level, repository/database level, to business logic level. It should still work, but forget trying to develop the code further, even it would be hard for anyone alse (or even ourselves) to understand what our code is doing.

**Q3**: Have you explored more about **Postman**? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

**A3**: In my own experience; The interface is neat and tidy, it's nice to have an access to an API endpoint without having to use a browser (which is pretty hard to navigate what kind of information we want to get). But the best feature for me personally is the collection system. One can import an already created Postman Collection which would skip the part where we have to manually structurize directories and make the approriate Requests (which is a painstakingly repetitive procedure). But here's the best part of the collection system, it is that we can export our own collection, which includes saved Requests that would tremendously help API documentation to cover all kinds of possible responses from our application.

#### Reflection Publisher-3
**Q1**: Observer Pattern has two variations: **Push model** (publisher pushes data to subscribers) and *Pull model** (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

**A1**: With the logs show by the Response given by the program as shown by the module, a notification is sent to the subscriber everytime a Product was `CREATED` or `DELETED`, which meant the Observation Pattern uses the **Push model** (publisher pushes data to subscribers).

**Q2**: What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

**A2**: If we were to use the **Pull model**, the advantages would be we won't expose ourselves to the publishers that we are subscribed to them, ensuring our own data safety, which mostly concerns our preferences in this case. Although the disadvantage is not a light one either, we would only get the notification manually whenever we wonted to, which meant we won't get the notification in real time and not the exact time when the `CREATED` or `DELETED` product was done.

**Q3**: Explain what will happen to the program if we decide to not use multi-threading in the notification process.

**A3**: It would mean that the notification being sent flows in a single-threadded program, and with that, there would also be a whole lot chronological issues involved with the notification that are being sent. This should not expose ourselves to any other risks outside this issue since Rust ensures our program runs properly during compilation. Although it would be extremely inefficient for program usage in a much bigger scale, as one could imagine thousands of users waiting for millions of notifications on a sigle-threadded program.