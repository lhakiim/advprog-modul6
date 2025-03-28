# advprog-modul6

### Commit 1 Reflection notes

Pada main.rs ia akan menerima koneksi, membaca request, dan memprosesnya. Lalu ia  akan menampilkan is dari request koneksi tersebut.

#### Fungsi `main`:
TcpListener digunakan untuk menerima koneksi masuk dari browser. `listener.incoming()`: Mengembalikan iterator yang menghasilkan aliran koneksi TCP yang masuk. Setiap koneksi direpresentasikan oleh objek TcpStream. Kemudian ia mengambil koneksi yang berhasil. Jika terjadi kesalahan atau gagal, program akan panic dan berhenti karena menggunakan `unwrap()`. Jika koneksi tersebut berhasil maka ia akan memanggil fungsi `handle_connection` untuk memproses koneksi yang masuk.

#### Fungsi `handle_connection`:
`BufReader` digunakan untuk membaca data dari koneksi dengan lebih efisien. Kemudian data dibaca perbaris menggunakan `buf_reader .lines()` dan menghasilkan nilai String dari result. Lalu ia akan mengumpulkan header HTTP dengan `take_while(|line| !line.is_empty())`, sehingga hanya bagian header yang diambil. Kemudian baris yang dibaca tersebut akan dikumpulkan kedalam vektor. Setelah itu, permintaan HTTP yang diterima akan dicetak ke terminal menggunakan `println!("Request: {:#?}", http_request);`

### Commit 2 Reflection notes
![Commit 2(1) screen capture](/assets/images/Commit%202(1).png)
![Commit 2(2) screen capture](/assets/images/Commit%202(2).png)

Pada method `handle_connection` terjadi penambahan code yang membuatnya dapat mengirimkan respon ke client. `status_line` berisi status 200 OK yang menandakan bahwa permintaan berhasil diproses. Kemudian content akan membaca isi file `hello.html`, jika file tersebut tidak ditemukan maka program akan panic karena menggunakan `unwrap()`. Content tersebut akan dihitung panjangnya agar dapat digunakan dalam header HTTP. Selanjutnya,server menyusun respons dalam format `{status_line}\r\nContent-Length:{length}\r\n\r\n{contents}`. Respon yang dibuat akan dikirimkan kembali ke client sehingga ketika user mengaksesnya melalui browser server akan mengembalikan isi dari `hello.html`.

### Commit 3 Reflection notes
![Commit 3(1) screen capture](/assets/images/Commit%203(1).png)
![Commit 3(2) screen capture](/assets/images/Commit%203(2).png)

Pada `handle_connection` sebelumnya, server akan selalu mengembalikan file HTML yang sama (hello.html). Sekarang, server memeriksa baris pertama request (request_line) untuk menentukan apakah user meminta path root (/). Jika request valid, server akan merespons dengan isi file `hello.html`,jika tidak, server akan mengembalikan respons halaman error `404.html`. Saya juga melakukan refactoring kode agar lebih bersih dan mudah dipahami. Sebelumnya, terdapat duplikasi kode pada blok if-else, yaitu respons 200 maupun 404 memiliki bagian kode yang sama dalam membaca file dan mengirimkan respons. Refactor pada bagian 
``` rust
if request_line == "GET / HTTP/1.1" {
        let status_line = "HTTP/1.1 200 OK";
```
menjadi 
``` rust
let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
        ("HTTP/1.1 200 OK", "hello.html")
```
Alasan utama refactoring ini adalah untuk mengurangi redundansi kode, meningkatkan readability, dan mempermudah pemeliharaan.

### Commit 4 Reflection notes

Method `handle_connection` dilakukan refactor dan ditambahkan endpoint baru yaitu `/sleep`. Request dari endpoint ini akan mendelay respon server selama 5 detik kemudian akan mengembalikan respon berupa `hello.html`. Karena server dijalankan dalam mode single-threaded, server hanya dapat mengirimkan satu request dalam satu waktu. Jadi, jika request `/sleep` dijalankan, maka request berikutnya harus menunggu hingga request `/sleep` selesai.

### Commit 5 Reflection notes

Pada commit sebelumnya, server mengalami hambatan karena harus menangani permintaan `/sleep`. Karena server berjalan dengan single-threaded maka permintaan lain harus menunggu permintaan tersebut selesai. Implementasi Threadpool memungkinkan server menangani beberapa permintaan secara paralel dengan beberapa thread. Dengan adanya ini pengguna tidak perlu menunggu lama untuk request yang seharusnya cepat, bahkan jika ada request lain yang memakan waktu lama. Setiap worker adalah thread yang siap menerima tugas. Ketika execute dipanggil, tugas dikirim ke channel. Worker yang tersedia akan mengambil tugas dari channel dan mengeksekusinya.