# tutorial6

# Commit 1 Reflection Notes

Pada tahap awal modul ini, saya telah melakukan beberapa langkah penting dalam proses pembelajaran penggunaan Git dan pembuatan server web sederhana menggunakan Rust. Berikut adalah poin-poin utama yang saya pelajari dan refleksi saya:

1. **Pengaturan Repository Git**  
   - Dengan perintah `git remote add tutorial6 <repository-url>`, saya berhasil menambahkan remote dengan nama *tutorial6* dan melakukan commit awal dengan pesan "initialized".  
   - Proses push ke remote repository berjalan lancar, yang membuktikan bahwa pengaturan remote dan proses commit sudah benar.  
   - **Refleksi:** Pengalaman ini memperkuat pemahaman saya mengenai pentingnya mengelola remote repository serta langkah-langkah dasar penggunaan Git untuk version control.

2. **Pembuatan Web Server Sederhana**  
   - Saya mengimplementasikan server web single-threaded dengan menggunakan `TcpListener` untuk menangani koneksi masuk.  
   - Dalam kode, saya menggunakan loop untuk membaca setiap koneksi, dan dengan fungsi `handle_connection`, saya memproses request dari browser dengan membaca header HTTP menggunakan `BufReader` dan metode `.lines()`.  
   - Meskipun server belum mengirimkan response lengkap ke browser, pesan "Connection established!" muncul di konsol, menandakan bahwa server berhasil menerima koneksi.  
   - **Refleksi:** Saya belajar bahwa meskipun server sederhana ini belum mengirimkan response ke browser, proses membaca request HTTP dapat dilakukan dengan cara yang efisien. Saya juga menyadari bahwa browser dapat melakukan beberapa percobaan koneksi jika response tidak segera diterima, yang merupakan perilaku normal.

3. **Pembelajaran Keseluruhan**  
   - Melalui langkah-langkah praktis ini, saya memahami cara integrasi antara version control dengan Git dan penerapan konsep dasar network programming di Rust.  
   - Proses ini membuka wawasan saya untuk mengembangkan server web yang lebih kompleks di masa depan dengan menambahkan fitur-fitur seperti pengiriman response HTTP yang valid dan penanganan error yang lebih baik.

Secara keseluruhan, pengalaman ini memberikan landasan yang kuat dalam menggunakan Git dan memulai pengembangan aplikasi jaringan dengan Rust. Langkah berikutnya adalah mengembangkan server dengan kemampuan multi-threaded serta menambahkan fitur-fitur response yang lebih lengkap.

# Commit 2 Reflection Notes

![Commit 2 screen capture](/assets/images/commit2.png)


Pada milestone ini, saya memodifikasi fungsi `handle_connection` agar server dapat mengirimkan HTML ke browser. Dengan menggunakan `fs::read_to_string("hello.html")`, saya membaca isi file HTML dan menyusunnya menjadi respons HTTP lengkap yang terdiri dari `status_line`, header seperti `Content-Length`, serta isi HTML itu sendiri. Hal ini penting untuk memberi tahu browser berapa banyak data yang harus dibaca, dan untuk memastikan bahwa browser dapat menampilkan konten yang benar. Saya juga menambahkan header `HTTP/1.1 200 OK` untuk memberi tahu bahwa permintaan berhasil diproses. Proses ini memperlihatkan cara kerja protokol HTTP pada level dasar: server membaca permintaan (meskipun saat ini belum memproses detail path atau metode HTTP), lalu membentuk respons yang valid sehingga browser dapat merendernya. Sebelum menjalankan server lagi, saya memastikan untuk mematikan proses server lama agar port tidak terkunci. Dengan memuat file `hello.html` di browser, saya melihat halaman HTML sederhana yang dikirim oleh server, menunjukkan bahwa mekanisme pengiriman file statis sudah berhasil. Hal ini memberi saya pemahaman lebih dalam tentang cara Rust menangani koneksi TCP dan bagaimana data HTTP perlu diformat agar dapat dikenali dengan benar oleh browser.

# Commit 3 Reflection Notes

![Commit 2 screen capture](/assets/images/commit3.png)

Pada tahap ini, saya mempelajari cara memvalidasi request yang masuk dan memberikan respons yang berbeda tergantung pada path yang diminta. Jika browser meminta path `/bad`, maka server mengirimkan file `404.html` beserta status `404 NOT FOUND`. Sebaliknya, untuk path lain (misalnya `/`), server mengirimkan file `hello.html` dengan status `200 OK`.

**Hal yang saya pelajari:**

1. **Parsing Request Line**  
   - Dengan melihat baris pertama request (misalnya `"GET /bad HTTP/1.1"`), saya dapat menentukan apakah pengguna meminta resource yang valid atau tidak.  
   - Saya menggunakan `starts_with("GET /bad")` untuk memeriksa apakah request mengarah ke halaman “bad”.

2. **Menyiapkan File yang Tepat**  
   - Ketika path tidak sesuai, server membaca dan mengirimkan `404.html`, yang menampilkan pesan error.  
   - Bila path sesuai, server membaca dan mengirimkan `hello.html`, yang menampilkan konten normal.

3. **Refactoring & Perawatan Kode**  
   - Memisahkan respons ke dalam dua file terpisah (`hello.html` dan `404.html`) memudahkan pemeliharaan.  
   - Menggunakan `format!` untuk membangun string respons HTTP membantu menjaga kode tetap ringkas dan mudah dipahami.

**Mengapa refactoring diperlukan?**  
Refactoring dibutuhkan agar kode lebih terstruktur dan mudah dikembangkan di masa depan. Dengan memisahkan logika pemilihan file, status HTTP, serta konten HTML, kita bisa menambahkan halaman lain atau menyesuaikan status HTTP tanpa mengubah banyak bagian kode.

Secara keseluruhan, proses ini memberi saya pemahaman yang lebih dalam tentang bagaimana web server dapat mengenali permintaan tertentu dan memberikan tanggapan berbeda sesuai dengan resource yang diminta.
