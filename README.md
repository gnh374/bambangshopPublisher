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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [v] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [v] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [v] Commit: `Implement publish function in Program service and Program controller.`
    -   [v] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [v] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
- Penggunaan interface Subscriber dalam design pattern Observer akan diperlukan jika kita memiliki Subscriber dengan berbagai jenis. Penggunaan interface akan memudahkan kita jika kita ingin menambahkan Subscriber jenis baru. Hal ini mengikuti Open-Closed Principle. Namun untuk kasus BambangShop saat ini penggunaan interface belum diperlukan karena seluruh Subscribersnya sejenis (berasal dari kelas yang sama)

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

- Menurut saya DashMap tetap lebih cocok digunakan. Hal ini dikarenakan DashMap menyimpan data dengan pasangan key dan value, dimana kita bisa mengakses data hanya dengan key. Hal ini dapat mempercepat dalam proses pencarian data, insertion, dan deletetion dibandingkan dengan menggunakan Vec dimana kita harus melakukan iterasi.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

- Menurut saya menggunakan DashMap sudah lebih tepat dibandingkan dengan HashMap. Hal ini dikarenakan HashMap tidak dirancang untuk digunakan secara konkuren sehingga tanpa mekanisme konkurensi yang tepat, program dapat mengalami race condition atau deadlock sedangkan DashMap sudah dirancang untuk digunakan secara konkuren sehingga lebih aman digunakan oleh beberapa thread.
- Penggunaan Singleton pattern juga tetap dibutuhkan untuk memastikan bahwa dashmap `SUBSCRIBERS` hanya ada 1. Hal ini untuk memastikan bahwa data hanya disimpan di dashmap tersebut dan program hanya mengakses serta mengedit dashmap tersebut
#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

- Pemisahan service dan repository dibutuhkan untuk memenuhi Single Responsibility Principle (SRP). Service akan berfokus pada logika bisnis, yaitu mengolah data yang didapat dari repository sedangkan repository akan berfokus pada logika pengaksesan data di database

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

- Jika demikian maka akan tercipta program yang memiliki coupling tinggi sehingga akan sulit untuk dimaintain dan sulit untuk diubah

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

- Postman dapat membantu saya untuk melakukan test terhadap endpoint API saya apakah sudah mengembalikan response sesuai yang saya harapkan. Fitur Collection pada postman sangat membantu saya dalam meng-organize  beberapa request.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

- Pada kasus ini kita menggunakan push model karena jika terjadi  create, delete atau update pada suatu produk, maka seluruh subscriber yang berlangganan product_type tersebut akan dikirimkan notifikasi

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

- Keuntungan pull methode: informasi subscriber tidak ter-expose oleh publisher. Kerugian pull methode: memperbesar potensi keterlambatan pembaruan karena subscriber harus secara aktif meminta pembaruan dari publisher

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

- Proses notifikasi akan menjadi sangat lambat karena pengiriman notifikasi pada tiap-tiap subscriber akan dijalankan secara sinkronus.