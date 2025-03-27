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

1. Why do we use RwLock<> instead of Mutex<>?

We use RwLock<> in this tutorial to manage access to the Vec of Notifications because it allows multiple threads to read the data at the same time, while still ensuring that only one thread can write to it when needed. This is ideal for situations where reading happens more often than writing, as it avoids unnecessary blocking and improves performance.

In contrast, Mutex<> only allows one thread at a time, whether it’s reading or writing. So even if multiple threads just want to read, they’d have to wait, which isn’t efficient in a read-heavy scenario like this one.

2. Why doesn't Rust allow direct mutation of static variables like Java?

Rust has strict rules around ownership and borrowing to guarantee memory safety and prevent data races. If it allowed direct mutation of static variables like Java, it could lead to unsafe behavior in multi-threaded programs.

Instead, Rust requires you to use tools like RwLock<> or Mutex<> to safely manage changes to static variables. This approach ensures that all shared data access is properly synchronized, making the code more robust and preventing unpredictable bugs.


#### Reflection Subscriber-2

1. Did you explore anything beyond the tutorial steps, like src/lib.rs? If not, why? If yes, what did you learn?

Yes, I looked into src/lib.rs to understand how the app's configuration is handled. I learned that the AppConfig struct, along with the dotenvy crate and Figment, is used to load and manage environment variables. This setup makes the application flexible, allowing it to adapt to different environments without hardcoding values.

2. Now that you've tested your notification system with multiple Receiver instances, how does the Observer pattern help with adding more subscribers? What about running more than one Main app instance?

The Observer pattern makes it easy to add more subscribers because each Receiver can register itself independently with the Publisher. This keeps the system loosely coupled and scalable. Running multiple instances of the Main app (Publisher), however, adds complexity. It would require coordination to ensure all notifications are sent properly. Still, as long as Receivers are linked to the right Publisher, the setup remains manageable.

3. Have you written your own tests or improved the documentation in your Postman collection? Was it helpful?

Yes, I wrote custom tests to check the functionality of NotificationRepository and NotificationService. These tests helped catch edge cases and made sure everything worked as expected. I also improved the Postman collection by adding clear descriptions and example requests, which made testing and debugging easier. This was especially helpful during group work where clarity and consistency are important.

