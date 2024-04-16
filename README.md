# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

- In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?


In this case, the use of `RwLock<Vec<Notification>>` is necessary to synchronize access to the shared vector of notifications across multiple threads. The RwLock allows for multiple readers or a single writer at any given time, providing concurrent read access while ensuring exclusive write access when modifying the vector. This is important because multiple threads may concurrently access the repository to add or retrieve notifications, and using RwLock ensures that data integrity is maintained. We do not use `Mutex<Vec<Notification>>` because it only allows for exclusive access at any given time, meaning that only one thread can access the data, whether it's for reading or writing. This would introduce unnecessary contention in scenarios where multiple threads may want to read notifications concurrently without needing to modify the underlying vector. RwLock allows for more efficient concurrent read access while still providing exclusive write access when needed, making it a better choice for this case.

- In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

In Rust, the concept of mutability and ownership is enforced rigorously by the compiler to prevent data races and ensure thread safety. Rust's ownership model guarantees memory safety and concurrency without the need for a garbage collector. Static variables in Rust are immutable by default. This means that once a static variable is initialized, its content cannot be modified. This design choice aligns with Rust's focus on safety and prevents potentially unsafe concurrent access to static variables, which could lead to data races and undefined behavior. By disallowing mutation of static variables, Rust ensures that concurrent access to shared data is controlled through safe and explicit mechanisms such as locks (e.g., Mutex, RwLock) or atomic operations. This approach promotes safer and more predictable concurrent programming in Rust, without sacrificing performance or introducing unnecessary complexity.


#### Reflection Subscriber-2

- Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Yes, I have. In lib.rs, the foundational components for the Receiver application in Bambangshop are established. This includes static configuration, HTTP client setup, error handling, and response construction. The code initializes static instances of the reqwest client and the application configuration, enabling global access throughout the application. Additionally, custom error handling logic is defined to construct error responses with HTTP status codes and messages, enhancing the robustness of the application. On the other hand, in main.rs, the Rocket web framework is set up to handle HTTP requests and manage application state. This file serves as the entry point for the Receiver application. Inside the rocket() function, Rocket's build() method initializes the Rocket instance, attaches the reqwest client as managed state, and sets up route stages for handling incoming requests. The Rocket instance is then returned from the function, establishing the foundation for the application's web server functionality.


- Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?


The Observer pattern facilitates the addition of more subscribers in a flexible and scalable manner. Each subscriber registers with the publisher to receive updates, decoupling the publisher from its subscribers. This means that adding new subscribers to the system does not require modifications to the publisher's code. Instead, new subscriber instances can be created and registered independently, seamlessly integrating with the existing system. Similarly, spawning more than one instance of the main application does not pose significant challenges when adding new subscribers. Each instance of the main application operates independently, managing its set of subscribers and handling incoming notifications. New subscribers can be added to any instance of the main application without affecting the operation of other instances. This scalability allows the system to accommodate additional subscribers and instances with ease, ensuring flexibility and maintainability as the system grows.


- Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Yes, it is helpful and useful for my work since creating our own Tests and enhancing the documentation on my Postman collection allows me to ensure good code coverage and test all happy and unhappy paths. This results in observing how well our implementation is made, and also seeing if particular use cases (or even edge cases) are correctly handled by the program.
