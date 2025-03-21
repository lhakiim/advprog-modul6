# advprog-modul6

### Commit 1 Reflection notes

Pada main.rs ia akan menerima koneksi, membaca request, dan memprosesnya. Lalu ia  akan menampilkan is dari request koneksi tersebut.

#### Fungsi `main`:
TcpListener digunakan untuk menerima koneksi masuk dari browser. `listener.incoming()`: Mengembalikan iterator yang menghasilkan aliran koneksi TCP yang masuk. Setiap koneksi direpresentasikan oleh objek TcpStream. Kemudian ia mengambil koneksi yang berhasil. Jika terjadi kesalahan atau gagal, program akan panic dan berhenti karena menggunakan `unwrap()`. Jika koneksi tersebut berhasil maka ia akan memanggil fungsi `handle_connection` untuk memproses koneksi yang masuk.

#### Fungsi `handle_connection`:
`BufReader` digunakan untuk membaca data dari koneksi dengan lebih efisien. Kemudian data dibaca perbaris menggunakan `buf_reader .lines()` dan menghasilkan nilai String dari result. Lalu ia akan mengumpulkan header HTTP dengan `take_while(|line| !line.is_empty())`, sehingga hanya bagian header yang diambil. Kemudian baris yang dibaca tersebut akan dikumpulkan kedalam vektor. Setelah itu, permintaan HTTP yang diterima akan dicetak ke terminal menggunakan `println!("Request: {:#?}", http_request);`

