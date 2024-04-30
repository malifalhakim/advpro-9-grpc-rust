# Reflection

Nama : M.Alif Al Hakim \
Kelas : B \
NPM : 2206081250

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
> Unary adalah tipe dari *communication protocol* yang menyediakan interaksi antara client dan server dimana client akan mengirim satu *request* ke server dan mendapatkan satu *response* kembali. Sementara itu, *server streaming* adalah jenis *communication protocol* yang membolehkan server mengirimkan *stream of response* ke *client*. Pola ini cocok untuk skenario dimana server perlu melakukan *push* data yang banyak atau besar.  Sementara itu, *bi-directional streaming* adalah *communication pattern* dimana *client* dan *server* dapat mengirimkan banyak *message* satu sama lain dalam sebuah *continuous stream*. *Pattern* ini cocok digunakan ketika terdapat kebutuhan akan *ongoing* dan *interactive* *communication*.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
> Ketika mengimplementasikan layanan gRPC di Rust, sangat penting untuk mempertimbangkan aspek keamanan seperti *authentication*, *authorization*, dan *data encryption*. Mekanisme *authentication*, seperti berbasis token atau SSL/TLS, harus dipilih untuk memverifikasi identitas pihak-pihak yang berkomunikasi. *Authorization*, yang memastikan pengguna yang diautentikasi hanya mengakses sumber daya yang diizinkan, dapat dikelola melaluiaccess *control lists* atau *role-based access control*. *Data encryption* sangat penting untuk melindungi data dalam perjalanan (melalui SSL/TLS) dan saat tidak digunakan, yang membutuhkan data sensitif untuk dienkripsi sebelum disimpan.

3.What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
> Menurut saya, tantangan yang dapat dihadapi ketika mengimplementasikan bidirectional streaming adalah tantangan pada *concurrency*, seperti beberapa *client* dapat mengirim dan menerima *message* secara bersamaan. Dan dalam keadaan tersebut, harus menjamin *message ordering* yang sesuai. Selain itu, perlu diperhatikan *error handling* agar jika terjadi masalah, seperti *networks errors* atau *client disconnected*, kita perlu mencegah *system* mengalami error atau *crashing*.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

>Kelebihan :
>- `ReceiverStream` menyediakan cara yang sederhana untuk mengkonvert sebuah `Receiver` menjadi Stream` yang mana bisa digunakan langsung di gRPC methods yang mengembalikan stream of response
>- Terintegrasi dengan baik dengan Tokio ekosistem

>Kekurangan:
>- Bersifat *one way communication*, karena `ReceiverStream` hanya membungkus `Receiver`, bukan `Sender`.
>- `ReceiverStream tidak menyediakan adanya *built-in* error *handling*.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

> Untuk meningkatkan reuse, modularity, maintanability, dan extensibility kita dapat menggunakan *traits*. Traits membantu kita mendefinisikan berbagai perilaku yang mungkin juga digunakan banyak objek. Hal ini tentu saja membantu meningkatkan aspek-aspek yang telah disebutkan sebelumnya, dimana kode akan menjadi lebih singkat, mudah untuk di-*maintain*, dan juga mudah untuk ditambah. Selain itu, kita juga perlu membagi kode yang kita buat menjadi beberapa modul dan setiap modul hanya memiliki satu tanggung jawab. Kita juga perlu mempraktikan error handling yang sesuai dan test untuk menghindari adanya bugs.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
> *Steps* tambahan yang dapat dilakukan untuk meng-*handle* *payment* *processing logic* yang lebih kompleksi diantaranya melakukan validasi *request* untuk mengecek semua informasi yang dibutuhkan terdapat pada *request* dan dalam format yang benar. Selain itu, dapat ditambahkan juga *error handling*, jika terjadi kesalahan pada proses yang dilakukan. Kita juga dapat menyimpan *transaction* yang dilakukan untuk kebutuhan *auditing.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
>  Penggunaan gRPC sebagai communication protocol membantu kita dalam membuat men-*generate* *client* dan *server code* untuk banyak bahasa pemrograman. Hal ini membantu kita dalam memfasilitasi komunikasi antar services dengan bahasa pemrograman yang berbeda. Karena sifatnya ini yaitu language-agnostic, gRPC cocok untuk diterapkan di lingkungan yang terdapat banyak teknologi dan platform yang berbeda.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
> Kelebihan penggunaan HTTP/2 diantaranya membantu dalam mengirimkan multiple request dan responses dalam waktu yang sama atau *multiplexing*. Selain itu, juga terdapat fitur kunci lain, seperti *header compression*, dan *server push*. Semua hal tersebut membantu dalam membangun client dan server application yang lebih cepat dan scalable. Akan tetapi, HTTP/2 cenderung lebih kompleksi dibanding HTTP/1.1 sehingga membuatnya cenderung lebih sulit di implementasikan dan di debug. Selain itu, HTTP/2 *support* WebSocket, sebuah protokol untuk *full-duplex communication*.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
> REST API menggunakan *synchronous request-response model*. Dimana *client* perlu mengirimkan *request* dan menunggu *response* dari server. Model ini bersifat *stateless* dan sederhana, akan tetapi kurang cocok untuk *real-time communication* karena *client* tidak bisa menerima data dari *server*, kecuali *client* mengirimkan *request*. Sementara itu, gRPC memiliki kemampuan *bidirectional streaming*. Hal ini berarti *client* dan *server* dapat mengirim dan menerima data kapan saja, tanpa tergantung sama lain. Model ini lebih cocok untuk *real-time communication* karena data dapat dikirimkan dari server ke *client* saat *client* tersedia dan *client* juga dapat mengirimkan data ke server kapan saja.

10.What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
> Schema-based approach dengan gRPC bersifat strongly typed dan membutuhkan *predefined schema*. Hal ini akan menghasilkan code yang lebih *robust* dan *reliable* dimana kesalahan umum yang terjadi bisa dideteksi saat *compile time*. Selain itu, Protocol Buffers lebih *compact* dan cepat untuk di-serialize atau di-deserialize dari pada JSON sehingga dapat meningkatkan performa sistem. Akan tetapi JSON bersifat lebih fleksibel, dimana kita dapat menambahkan fields atau mengganti struktur dari data tanpa perlu mengupdate schema.