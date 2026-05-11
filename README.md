# Modul 9 Software Architecture

## Reflection 1

a. How much data your publisher program will send to the message broker in one
run?  
Dalam sekali eksekusi, program publisher dirancang untuk mengirimkan tepat 5 buah data event secara berurutan ke message broker. Masing-masing data tersebut dibungkus dalam struktur UserCreatedEventMessage yang berisi kombinasi informasi unik berupa user_id dan user_name (secara spesifik merepresentasikan data untuk Amir, Budi, Cica, Dira, dan Emir). Pengiriman kelima pesan ini dilakukan secara instan dalam satu siklus program utama, di mana publisher mempublikasikan setiap objek pesan tersebut ke dalam antrean (queue) bernama "user_created" di RabbitMQ sebelum akhirnya eksekusi program publisher selesai dan berhenti.  
b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber
program, what does it mean?  
Kesamaan URL koneksi tersebut menandakan bahwa program publisher dan subscriber terhubung ke satu message broker terpusat yang sama, yaitu instance RabbitMQ yang berjalan di lokal (localhost) pada port standar AMQP (5672) menggunakan kredensial akses default (guest:guest). Secara arsitektural, penggunaan alamat broker yang sama ini sangat penting karena broker bertindak sebagai jembatan perantara komunikasi async di antara keduanya. Dengan menunjuk ke alamat yang sama, publisher tahu ke mana ia harus melempar pesan agar masuk ke dalam antrean, sementara subscriber juga tahu ke titik mana ia harus "mendengarkan" (listen) untuk menarik pesan-pesan tersebut, sehingga kedua program dapat saling berinteraksi tanpa harus terhubung secara langsung satu sama lain (decoupled).

## Reflection 2

### Running RabbitMQ as message broker

![Running RabbitMQ](public/running-rabbitmq.png)

### Sending and processing event

![Sending and processing event](public/connection-message-broker.png)
![Processing event in subscriber](public/console-subscriber-publisher.png)

Berdasarkan screenshot di atas, ketika perintah cargo run dieksekusi pada publisher, program mengirimkan 5 data event secara bersamaan ke dalam antrean RabbitMQ. Karena subscriber sedang berjalan dan dalam status aktif mendengarkan (listening), message broker langsung mendistribusikan kelima pesan tersebut untuk ditangkap dan diproses secara instan oleh subscriber. Hal ini membuktikan bahwa komunikasi async antara publisher dan subscriber melalui RabbitMQ telah berjalan dengan sukses.

### Monitoring chart based on publisher

![Monitoring chart based on publisher](public/monitoring-chart-publisher.png)

Lonjakan (spike) pada grafik berkaitan langsung dengan eksekusi program publisher. Setiap kali menjalankan perintah cargo run, program secara instan mengirimkan 5 event pesan sekaligus ke dalam message broker. Grafik tersebut merespons dengan menampilkan lonjakan tajam pada metrik message rates yang menunjukkan lonjakan jumlah pesan yang masuk (publish) ke antrean RabbitMQ dalam waktu yang sangat singkat pada waktu tersebut.
