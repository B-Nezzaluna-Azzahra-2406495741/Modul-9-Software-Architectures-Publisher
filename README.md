# Modul 9 Software Architecture

## Reflection 1

a. How much data your publisher program will send to the message broker in one
run?  
Dalam sekali eksekusi, program publisher dirancang untuk mengirimkan tepat 5 buah data event secara berurutan ke message broker. Masing-masing data tersebut dibungkus dalam struktur UserCreatedEventMessage yang berisi kombinasi informasi unik berupa user_id dan user_name (secara spesifik merepresentasikan data untuk Amir, Budi, Cica, Dira, dan Emir). Pengiriman kelima pesan ini dilakukan secara instan dalam satu siklus program utama, di mana publisher mempublikasikan setiap objek pesan tersebut ke dalam antrean (queue) bernama "user_created" di RabbitMQ sebelum akhirnya eksekusi program publisher selesai dan berhenti.  
b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber
program, what does it mean?  
Kesamaan URL koneksi tersebut menandakan bahwa program publisher dan subscriber terhubung ke satu message broker terpusat yang sama, yaitu instance RabbitMQ yang berjalan di lokal (localhost) pada port standar AMQP (5672) menggunakan kredensial akses default (guest:guest). Secara arsitektural, penggunaan alamat broker yang sama ini sangat penting karena broker bertindak sebagai jembatan perantara komunikasi async di antara keduanya. Dengan menunjuk ke alamat yang sama, publisher tahu ke mana ia harus melempar pesan agar masuk ke dalam antrean, sementara subscriber juga tahu ke titik mana ia harus "mendengarkan" (listen) untuk menarik pesan-pesan tersebut, sehingga kedua program dapat saling berinteraksi tanpa harus terhubung secara langsung satu sama lain (decoupled).
