# WhatsAuth : Free 2FA, OTP, Notif, WhatsApp Gateway API Gratis

WhatsAuth menghadirkan solusi untuk :
1. Single Sign On
2. 2FA (2 Factor Auth)
3. OTP (One Time Password)
4. Notifikasi WA
5. WhatsApp API Gateway untuk integrasi dengan aplikasi anda


## Persiapan WhatsApp Gateway
Tahapan ini dilakukan terlebih dahulu sebelum melakukan pendaftaran, hal-hal yang harus dipersiapkan antara lain:
1. Siapkan Nomor WhatsApp yang akan dijadikan Gateway API
2. Siapkan URL WebHook sebagai penerima pesan masuk, kita akan melakukan test dahulu untuk mengetahui apa saja yang dikirim ke webhook bisa menggunakan layanan pipedream. Silahkan buka [Panduan Membuat Dummy WebHook](https://universitas.bukupedia.co.id/ws/Chapter02/)

## Pendaftaran WhatsApp Gateway Melalui Interface Web
Proses nya tinggal buka [wa.my.id](https://wa.my.id/login) dengan urutan :
1. Scan QR Code dengan scanner QR atau tombol foto dari aplikasi whatsapp, kakak akan diarahkan masuk ke dalam situs.  
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/a4da833b-e267-4f1e-be93-c9f2244b55e2)  
2. Input URL dan Secret Webhook kakak terus klik submit.
3. Masukkan Pair Code ke WhatsApp yang ada di Handphone tunggu beberapa saat sampai proses loading di handphone selesai.  
   ![WhatsApp Image 2023-11-07 at 01 07 50_3f9cbb85](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/a3e3bca7-d78e-4f74-a2fb-34ef850e91c3)  
   ![WhatsApp Image 2023-11-07 at 01 07 45_d9155096](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/9e44609e-321d-43f6-b760-6a8f038a7411)  
   ![WhatsApp Image 2023-11-07 at 01 07 39_e0a1d259](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/249fab3a-7bba-4b50-b160-41cd6fa825db)  
   Tunggu beberapa menit hingga proses sinkronisasi WhatsApp selesai berjalan.
4. Simpan token sementara yang muncul untuk digunakan di laman [apidocs](https://wa.my.id/apidocs/#/signup/signUpNewUser) untuk ditukar menjadi token yang berlaku selama 30 hari.  
   ![image](https://github.com/whatsauth/docs/assets/11188109/7e9548f6-0f3f-4892-95ce-12e7d645c698)  
5. Kita akan melakukan uji pengiriman pesan untuk memastikan WhatsApp kita sudah terdaftar dengan baik. Masuk menu Kirim Pesan untuk mengirimkan pesan.  
   ![image](https://github.com/whatsauth/docs/assets/11188109/83c31870-4f31-411e-871a-5b41b020717d)  
   ![image](https://github.com/whatsauth/docs/assets/11188109/81aa28df-10f8-4ebb-af6a-4ba9b98e8582)  
   Tunggu beberapa menit maka pesan akan sampai ke tujuan, pastikan nomor tujuan tidak memblokir nomor pengirim.
6. Buka [Dokumen api](https://wa.my.id/apidocs/#/signup/signUpNewUser) untuk menukar token langkah sebelumnya menjadi token yang berlaku selama 30 hari.
7. Klik bagian Authorize dan masukkan token ke dalam kolom Value: dan klik Authorize  
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/78d313a7-345f-40fe-9cf6-7cbf58fbba2e)  
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/54826caf-597a-4151-938c-bbb077b23741)
8. Klik API signup, klik Try it out. Kemudian masukkan URL dan Secret dari WebHook yang sudah dibuat sebelumnya. Lihat respon, simpan baik baik token yang diterima, token tersebut berlaku selama 30 hari.
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/fd89a320-3228-4cad-85d8-ecefd9a324e5)
   ![image](https://github.com/whatsauth/docs/assets/11188109/c1573feb-d39b-4c33-8b29-a5ba9708e299)  

## Panduan Development WebHook Disertai Contoh Program

Silahkan buka [Panduan Deployment WebHook](/webhook)
   
## List fungsi API Lainnya
Beberapa list fungsi API lainnya :
1. Untuk pendaftaran ulang device(whatsapp baru install ulang). Pilih pada bagian API device. Klik Try it out, kemudian masukkan token pada langkah sebelumnya. Ketika execute, maka akan ada notifikasi Pair Device pada handphone. Masukkan kode unik dari respon server field code ke WhatsApp pair device di handphone.  
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/c55f0c20-1586-4c54-a676-b0ffa9b73f17)  
   ![WhatsApp Image 2023-11-07 at 01 07 50_3f9cbb85](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/a3e3bca7-d78e-4f74-a2fb-34ef850e91c3)  
   ![WhatsApp Image 2023-11-07 at 01 07 45_d9155096](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/9e44609e-321d-43f6-b760-6a8f038a7411)  
   ![WhatsApp Image 2023-11-07 at 01 07 39_e0a1d259](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/249fab3a-7bba-4b50-b160-41cd6fa825db)  
   Tunggu beberapa menit hingga proses sinkronisasi WhatsApp selesai berjalan.
2. Mencoba mengirimkan notif pesan kepada nomor telepon tujuan. Buka API message klik Try it out, isi to,isgroup dan message. Ketika klik execute maka akan ada notif pesan ke nomor tujuan dari nomor Gateway yang didaftarkan.
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/74d73883-2c91-4c22-a35c-1a4e2ef88977)  

## QRCode Login
API whatsauth dapat digunakan untuk pengembangan implementasi SSO, login menggunakan QR. Caranya deploy dahulu [JS ini](https://github.com/whatsauth/js).

## Tidak merespon pesan
Jika pesan yang dikirim tidak mendapatkan balasan dari webhook lebih dari 1 menit sejak pesan dikirim, maka coba langkah ini:
1. Mencoba mengirimkan notif pesan kepada nomor telepon tujuan. Buka API message klik Try it out, isi to,isgroup dan message. Ketika klik execute maka akan ada notif "device belum di start".
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/74d73883-2c91-4c22-a35c-1a4e2ef88977)  
2. Start device pada bagian API device. Klik Try it out, kemudian masukkan token pada langkah sebelumnya. Ketika execute, maka akan ada balasan message dari server.
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/2eaf0098-6e91-4733-b7ca-96e2483e5b58)  
