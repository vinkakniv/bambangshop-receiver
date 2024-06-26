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
> In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we don’t use Mutex<> instead?

Dalam tutorial ini, penggunaan `RwLock<>` dibutuhkan untuk memungkinkan akses baca bersamaan oleh beberapa thread ke `Vec Notifications` sekaligus memastikan penulisan dilakukan secara eksklusif. Ini efektif untuk struktur data yang lebih sering dibaca daripada ditulis, yang juga meningkatkan konkurensi dan kinerja. `RwLock<>` memperbolehkan banyak pembaca atau satu penulis pada suatu waktu, berbeda dengan `Mutex<>` yang membatasi akses ke satu thread untuk semua operasi, mengakibatkan pemblokiran yang tidak perlu ketika operasi baca lebih dominan. Oleh karena itu, `RwLock<>` adalah pilihan yang lebih baik untuk skenario di mana frekuensi baca tinggi dan tulis rendah, sedangkan `Mutex<>` lebih cocok untuk situasi dengan frekuensi baca dan tulis yang seimbang.

> In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why didn't Rust allow us to do so?

Rust memberlakukan beberapa aturan ketat terhadap variabel statis, yang berbeda dari bahasa pemrograman seperti Java, untuk meningkatkan keamanan dan keandalan program. Dalam Rust, variabel statis bersifat _immutable_ secara default untuk menghindari _data race_, situasi di mana beberapa operasi mencoba mengakses dan memodifikasi data yang sama secara bersamaan, yang bisa mengganggu integritas data. Selain itu, variabel statis di Rust harus diinisialisasi dengan nilai yang diketahui saat kompilasi, tidak memungkinkan inisialisasi atau modifikasi pada _runtime_ melalui fungsi statis, seperti yang mungkin dilakukan di Java. Sebagai solusi atas keterbatasan ini, Rust menyediakan `crate lazy_static` yang memungkinkan definisi variabel statis dengan inisialisasi pada _runtime_, namun tetap mempertahankan jaminan keamanan Rust. 

#### Reflection Subscriber-2

> Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you didn’t do so. If yes, explain things that you’ve learned from those other parts of code.

Saya baru saja mengeksplorasi beberapa bagian. Untuk `lib.rs`, digunakan _crates_ seperti `lazy_static` untuk variabel statis, `dotenvy` untuk _environment variables_, `getset` untuk metode _getter/setter_, `rocket` untuk fungsionalitas _web framework_, dan `reqwest` untuk permintaan HTTP. Struktur `AppConfig` didefinisikan dengan atribut `#[derive(Debug, Deserialize, Serialize, Getters)]`, yang secara otomatis mengimplementasikan _traits_ `Debug`, `Deserialize`, `Serialize`, dan `Getters` untuk struktur tersebut. Ini berarti sistem dapat mencetak struktur tersebut untuk tujuan debugging, mengonversikannya ke dan dari JSON, serta menggunakan metode getter yang dihasilkan untuk mengakses _field_-nya. Selain itu, `ErrorResponse` struct dan fungsi `compose_error_response` didefinisikan untuk _error handling_. Bagian tersebut membuat respons JSON khusus dengan kode status dan pesan kesalahan.

> Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than 1 instance of Main app, will it still be easy enough to add to the system?

_Observer pattern_ memudahkan penambahan _subscriber_ baru ke dalam sistem notifikasi dengan menyediakan arsitektur yang fleksibel dan mudah diperluas. Dalam pola ini, setiap _subscriber_ berfungsi sebagai _observer_ yang menerima notifikasi dari aplikasi utama, atau _subject_, tanpa memerlukan modifikasi pada fungsi inti sistem. Selain itu, menjalankan lebih dari satu _instance_ dari aplikasi utama juga efisien karena masing-masing _instance_ dapat mengelola daftar _subscriber_-nya sendiri secara independen, memungkinkan pemrosesan notifikasi secara paralel. Pendekatan ini menjamin sistem dapat menyesuaikan diri dengan pertumbuhan dan beban kerja yang meningkat, sekaligus menjaga fleksibilitas untuk ekspansi di masa mendatang.

> Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Saya baru saja mulai menjelajahi fitur-fitur seperti pembuatan test dan peningkatan dokumentasi dalam _collelction_ Postman, lebih kepada melihat-lihat daripada membuat secara aktif. Meskipun saya belum sering menggunakan fitur tersebut secara intensif, saya sudah mulai memahami bagaimana mereka dapat mempengaruhi proses pengembangan. Misalnya, melihat bagaimana test dibuat di Postman memberikan gambaran tentang bagaimana API harus berfungsi dan bagaimana responsnya diuji secara otomatis untuk konsistensi. Sementara itu, eksplorasi dokumentasi membantu saya mengerti pentingnya memiliki dokumentasi yang baik untuk memudahkan referensi dan kolaborasi. Meskipun pengalaman praktis saya masih terbatas, pengalaman awal ini cukup membantu, dan saya berharap dapat segera menerapkan penggunaan fitur ini secara lebih mendalam dalam proyek-proyek mendatang.
