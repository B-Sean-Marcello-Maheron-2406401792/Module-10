### “Experiment 1.1: Original timer from the book”

![img.png](img.png)

### “Experiment 1.2: Understanding how it works.”
![img_1.png](img_1.png)

Pada Experiment 1.2, urutan output menunjukkan bahwa pesan hey hey tercetak di terminal mendahului pesan howdy dan done yang berada di dalam blok asinkronus. Fenomena ini terjadi karena sifat dasar Future di dalam bahasa pemrograman Rust yang bersifat lazy atau malas, di mana sebuah instruksi asinkronus tidak akan langsung dieksekusi saat dideklarasikan. Ketika fungsi spawner.spawn dipanggil, ia hanya bertugas membungkus objek future tersebut ke dalam sebuah Task lalu mengirimkannya ke dalam antrean saluran komunikasi ready_queue milik Executor. Proses pendaftaran tugas ini berlangsung sangat cepat dan bersifat non-blocking, sehingga alur utama program (main thread) langsung melanjutkan eksekusi ke baris baris kode berikutnya tanpa menunggu blok asinkronus selesai diproses. Akibatnya, perintah mencetak pesan hey hey langsung dieksekusi terlebih dahulu oleh main thread. Blok kode asinkronus di dalam spawner baru akan benar-benar dijalankan dan dievaluasi ketika kendali program diserahkan sepenuhnya kepada fungsi executor.run di bagian akhir fungsi utama. Di dalam mesin executor inilah metode poll pertama kali dipicu untuk menjalankan tugas yang mengantre, yang kemudian mencetak kata howdy, menangguhkan diri selama dua detik saat menemui perintah await pada TimerFuture, dan akhirnya mencetak kata done setelah dibangunkan kembali oleh komponen waker.

