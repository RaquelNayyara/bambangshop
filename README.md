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
1. Intinya, kita perlu atau tidaknya menggunakan interface atau trait dalam pola Observer tergantung pada beragam atau tidaknya jenis observer. Untuk `bambangshop`, jika cuma ada satu tipe observer, yaitu `Subscriber`, kita tidak perlu pakai interface atau trait. Tapi, kalau ke depannya ada tipe observer lain, menggunakan trait bisa lebih memudahkan.

2. Lebih baik pakai Dashmap sebab memudahkan kita menghubungkan produk ke subscriber yang berlangganan. Dengan DashMap, kita bisa gampang kaitkan id program dengan url subscriber. Kalau pakai Vec, kita butuh tambahan seperti dua daftar terpisah buat simpan id program dan url, yang bisa bikin repot saat atur atau update data.

3. Untuk keamanan pemrograman dengan multithreading Rust, DashMap sudah pilihan bagus karena sudah disesuaikan untuk lingkungan dengan banyak multithread. DashMap memastikan kita bisa atur daftar SUBSCRIBERS dengan aman dan efisien, walaupun banyak thread yang bekerja bersamaan. Walaupun pola Singleton bisa jamin cuma ada satu objek yang ada, itu belum tentu aman dari thread seperti yang bisa DashMap atasi. Dengan DashMap, kita bisa yakin daftar subscriber kita aman dan dikelola dengan efektif meski di thread-safe.

#### Reflection Publisher-2
1. Dalam desain MVC, kita pisahkan Service dan Repository agar setiap bagian hanya punya satu prinsip single responsibility. Service itu untuk atur logika bisnis dan olah data dari Repository, sedangkan Repository itu untuk bantu akses dan atur data di database. Karena dibagi begitu, kode kita jadi lebih rapi, gampang dimengerti, dan gampang di refactor.

2. Kalau kita cuma pakai Model tanpa Service atau Repository, nanti bagian-bagian kode kita terlalu bergantung satu sama lain. Ini bikin susah pas mau ubah-ubah kode, karena setiap perubahan di Model bakal pengaruhi semua kode lain. Ini malah bikin kode kita jadi lebih rumit dan kurang fleksibel buat dikembangin atau direfactor.

3. Postman itu alat yang keren buat cek dan pastiin kalau aplikasi yang kita buat itu bekerja dengan benar. Pake Postman, kita bisa kirim permintaan HTTP ke tempat-tempat yang berbeda di aplikasi kita dan lihat jawabannya untuk cek data itu bener dan sama. Bisa juga tes fitur dasar kayak bikin, baca, update, sama hapus data. Gampangnya atur permintaan dan langsung lihat jawabannya itu membantu banget buat cepet-cepet tes dan perbaiki aplikasi kita.

#### Reflection Publisher-3

1. Di tutorial ini, kita pakai cara push dari Observer Pattern. Ini artinya, setiap ada perubahan pada objek, seperti dibuat, dihapus, atau diperbarui, layanan notifikasi otomatis kasih tau semua subscriber tentang perubahan itu.

2. Kalau metode pull yang dipakai, setiap subscriber harus sendiri cek ada atau tidak perubahan data yang penting buat mereka. Ini bagus karena mereka yang subscribe yang atur kapan mau ambil data. Tapi, kelemahannya, mereka harus tahu banyak tentang data asalnya buat bisa melakukan ini.

3. Kalau kita pilih buat engga pakai multi-threading saat memberi notifikasi, bisa jadi ada delay waktu `NotificationService` perlu kasih tau setiap subscriber. Ini bisa bikin antrean jadi panjang kalau pelanggannya banyak, dan jadinya pengiriman notifikasi bisa lambat karena komputasi kita punya batasan.