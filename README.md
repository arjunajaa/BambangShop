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

1. Dalam konteks desain pola Observer, penggunaan Subscriber (atau Observer) sebagai sebuah trait dalam bahasa Rust seringkali dianggap sebagai pendekatan yang sangat berguna. Dengan menerapkan Subscriber sebagai trait, kita memberikan kemampuan untuk memiliki banyak implementasi konkret yang berbeda. Ini memungkinkan fleksibilitas yang lebih besar dalam pengembangan kode karena memungkinkan penambahan jenis-jenis subscriber baru tanpa perlu mengubah struktur kode yang sudah ada.
<br> <br> Ketika kita berbicara tentang implementasi ini dalam konteks BambangShop, menggunakan sebuah struct Model tunggal mungkin sudah cukup. Namun, mempertimbangkan kemungkinan adanya berbagai jenis subscriber di BambangShop, menerapkan Subscriber sebagai sebuah trait akan menjadi pilihan yang lebih bijak. Dengan cara ini, setiap jenis subscriber dapat memiliki perilaku notifikasi yang berbeda, tanpa mengganggu fungsi utama dari struct Model. Hal ini memungkinkan BambangShop untuk menjadi lebih adaptif terhadap perubahan dan kebutuhan bisnis yang mungkin muncul di masa depan.

2. Dalam konteks ini, DashMap menjadi pilihan yang lebih unggul daripada Vec ketika kita perlu menangani id unik di Program dan URL di Subscriber. DashMap adalah implementasi HashMap yang bersifat konkuren, yang berarti itu dapat digunakan dengan aman dari beberapa thread secara bersamaan. Kelebihan utama DashMap adalah efisiensinya dalam menyimpan pasangan key-value dengan kompleksitas waktu konstan O(1) untuk operasi penambahan, penghapusan, dan pencarian berdasarkan key.
<br><br> Di sisi lain, jika kita menggunakan Vec untuk memastikan keunikan, kita harus melakukan iterasi pada seluruh daftar untuk memeriksa duplikat setiap kali menambahkan item baru. Ini menghasilkan kompleksitas waktu O(N), yang tidak efisien terutama saat ukuran data menjadi besar.
<br> <br> Oleh karena itu, penggunaan DashMap menjadi lebih disukai karena efisiensinya dalam memastikan keunikan id dan URL, serta kemampuannya untuk menangani akses konkuren dengan aman.

3. Dalam situasi tersebut, DashMap memang menjadi solusi yang lebih baik untuk menjamin keamanan thread saat beberapa thread beroperasi pada variabel statis SUBSCRIBERS secara bersamaan. Hal ini terutama karena Singleton Pattern tidak selalu dapat menjamin keamanan thread, terutama ketika ada konkurensi di dalam sistem.
<br> <br> Dengan menggunakan DashMap, kita memastikan bahwa operasi-operasi pada SUBSCRIBERS dapat dilakukan secara aman oleh banyak thread sekaligus. DashMap memungkinkan akses konkuren ke Map, sehingga menghindari masalah seperti race condition yang mungkin terjadi dalam implementasi Singleton.
<br> <br> Oleh karena itu, penggunaan DashMap dalam kasus tersebut merupakan pilihan yang lebih aman dan dapat diandalkan untuk memastikan keamanan thread dan integritas data, ketimbang mengandalkan Singleton Pattern yang mungkin kurang dapat diandalkan dalam konteks konkurensi.

#### Reflection Publisher-2

1. Dengan memisahkan "Service" dan "Repository", kita dapat meningkatkan kejelasan dan keterbacaan kode karena menerapkan prinsip Single Responsibility Principle. "Repository" bertugas untuk mengelola akses data, sedangkan "Service" fokus pada logika bisnis, menggunakan Repository untuk memperoleh dan menyimpan data yang dibutuhkan.
<br> <br> Dengan memisahkan ini, setiap bagian dari aplikasi memiliki fokus yang jelas dan tanggung jawab yang terdefinisi dengan baik. Ini membuat kode lebih mudah dipahami dan dirawat karena setiap komponen dapat diuji dan diperbarui secara independen tanpa mempengaruhi yang lain.
<br> <br> Pemisahan ini juga meningkatkan maintainability, karena memungkinkan pengembang untuk fokus pada satu aspek dari aplikasi pada satu waktu tanpa harus khawatir tentang bagian lainnya. Jadi, pemisahan antara "Service" dan "Repository" bukan hanya tentang struktur kode yang lebih bersih, tetapi juga tentang meningkatkan fleksibilitas dan maintainability dari aplikasi secara keseluruhan.

2. Jika kita hanya bergantung pada Model dalam pengembangan aplikasi, maka Model akan menjadi rumah bagi semua method yang terkait dengan operasi logika bisnis dan akses data. Akibatnya, interaksi langsung antara model-model ini akan terjadi, mungkin memaksa kita untuk menambahkan method tambahan dalam model untuk mendukung interaksi langsung tersebut. Seiring waktu, dengan bertambahnya fitur dan kompleksitas program, Model bisa menjadi terlalu "berisi".
<br> <br> Kondisi seperti ini menghasilkan kompleksitas yang tidak diinginkan dan mengurangi modularitas serta kemudahan pemeliharaan kode. Model yang menjadi terlalu "berisi" akan sulit dipahami dan dipelihara, serta membuat pengembangan lebih lambat karena sulit untuk memisahkan tanggung jawab antara komponen-komponen dalam program.
<br> <br> Oleh karena itu, memisahkan tanggung jawab antara Model, Service, dan Repository menjadi penting untuk mempertahankan modularitas dan pemeliharaan kode yang baik. Dengan demikian, setiap komponen dapat fokus pada tugasnya sendiri dan interaksi antara komponen-komponen dapat dikelola dengan lebih terstruktur dan efisien.

3. Awalnya, saya menggunakan Postman hanya untuk keperluan mata kuliah PBP, tetapi saya tidak terlalu memanfaatkan fitur-fitur yang tersedia saat itu. Namun, setelah saya mulai menjelajahi Postman dalam proyek ini, saya menyadari betapa kuatnya alat ini untuk melakukan pengujian API.
<br> <br> Postman memungkinkan saya mengirimkan permintaan HTTP ke server dan memeriksa responsnya dengan mudah. Fitur ini sangat berguna untuk melakukan debugging dan memverifikasi bahwa setiap endpoint berfungsi sesuai yang saya harapkan.
<br> <br> Penggunaan Postman telah membuka mata saya terhadap potensi alat ini. Saya menemukan bahwa dengan Postman, saya dapat dengan cepat melakukan pengujian API dan memvalidasi fungsionalitasnya tanpa perlu menulis skrip pengujian yang rumit atau menggunakan alat pengujian yang lebih kompleks. Ini benar-benar mempermudah proses pengembangan dan memastikan kualitas API yang saya bangun.

#### Reflection Publisher-3
