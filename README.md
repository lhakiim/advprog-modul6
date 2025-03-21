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