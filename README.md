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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. No. Our single Model struct (`Subscriber`) represents subscribers, and operations are performed directly on instances of this struct. If all subscribers are expected to have the same behavior and interface, then this approach is sufficient.
2. No, using **Vec** (list) is not sufficient. Considering the need for uniqueness based on **id** for **Program** and **url** for **Subscriber**, using a `DashMap` seems necessary. It provides efficient lookup and insertion based on unique keys, ensuring that duplicates are not allowed.
3. Singleton pattern could be used to create a single instance of the `DashMap` for storing subscribers, but it does not inherently provide thread safety. Using DashMap directly ensures thread safety and simplifies the implementation, as it already handles concurrent access gracefully.

#### Reflection Publisher-2
1. The separation of “Service” and “Repository” from the Model in MVC is primarily about adhering to the **Single Responsibility Principle (SRP)** and the **Separation of Concerns (SoC)** principle. By separating out the “Service” (business logic) and “Repository” (data storage), each class has a single, well-defined responsibility. This makes the system easier to understand, test, and maintain. Furthermore, a “Service” class can focus on business logic, while a “Repository” class can focus on data storage. This separation allows for better modularity and easier code management.
2. If we only use the models without any additional design patterns or architectural considerations, the interactions between the models (`Product`, `Subscriber`, and `Notification`) will likely be direct and tightly coupled. The `Product` model represents a product in the system. Without any additional architecture, the `Product` model would not have direct knowledge of other models such as `Subscriber` or `Notification`. This works for the other two models. Overall, without additional architectural patterns or design considerations, the code complexity for each model may increase as the application grows and more interactions between models are required.
3. Here's how it helps me in my work and some features I find particularly useful:
- **Collections**: Collections allow me to organize related requests into groups, making it easy to manage and execute them as a sequence.
- **API Testing**: Postman provides a user-friendly interface for sending HTTP requests to APIs, making it easy to test different endpoints, methods, and payloads. I can quickly create requests, specify parameters, headers, and authentication details, and examine the responses.

#### Reflection Publisher-3
1. Push model
2. **Advantages of Pull Model:** In the pull model, subscribers actively request updates from the subject when they are ready to process them. This reduces coupling between the subject and its subscribers, as the subject doesn't need to have direct knowledge of its subscribers.
**Disadvantages of Pull Model:**
Subscribers in the pull model need to implement logic for polling the subject and handling updates. This can increase complexity for subscribers, especially if they need to manage multiple subjects or prioritize updates from different sources.
3. **Reduced Concurrency:** Without multi-threading, the program will have reduced concurrency capabilities. Each request will be processed sequentially, one after the other, instead of being processed concurrently in multiple threads. This can limit the program's ability to handle multiple requests simultaneously and may result in lower responsiveness under heavy load.
**Potential Deadlocks or Race Conditions:** If the notification process involves shared resources or mutable state, not using multi-threading can increase the risk of deadlocks or race conditions. Without proper synchronization mechanisms, concurrent access to shared resources may lead to unexpected behavior and data corruption.
