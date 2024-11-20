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
2. Siapkan URL WebHook sebagai penerima pesan masuk. Jika menggunakan [GoCroot](https://gocroot.if.co.id/) URL webhook akan tampak seperti ini `https://asia-southeast2-awangga.cloudfunctions.net/logiccoffee/webhook/nomor/62881022526506`. ganti `62881022526506` dengan nomor whatsapp anda dengan format 628xxx. dan sesuaikan URL `asia-southeast2-awangga` dan `logiccoffee` sesuai dengan project dan nama GCF.
3. Pengguna GoCroot cukup untuk membuat collection baru bernama profile di dalam database kemudian isi dengan json:
   ```json
   {
     "token": "v4.public.asdasfafdfsdfsdf",
     "phonenumber": "62881022526506",
     "secret": "secretkamuyangpanjangdanrumit089u08j32",
     "url": "https://asia-southeast2-awangga.cloudfunctions.net/logiccoffee/webhook/nomor/62881022526506",
     "urlapitext": "https://api.wa.my.id/api/v2/send/message/text",
     "urlapiimage": "https://api.wa.my.id/api/send/message/image",
     "urlapidoc": "https://api.wa.my.id/api/send/message/document",
     "urlqrlogin": "https://api.wa.my.id/api/whatsauth/request",
     "qrkeyword": "wh4t5auth0",
     "publickey": "0d6171e848ee9efe0eca37a10813d12ecc9930d6f9b11d7ea594cac48648f022",
     "botname": "lofe",
     "triggerword": "lofe",
     "telegramtoken": "",
     "telegramname": ""
   }
   ```
   Silahkan sesuaikan pada bagian token, phonenumber dan secret. Untuk nama bot sesuaikan di bagian botname dan triggerword.

## Pendaftaran WhatsApp Gateway Melalui Interface Web
Proses nya pertama **login dulu di [wa.my.id](https://wa.my.id/login)** dengan urutan :
1. Buka laman **[login wa.my.id](https://wa.my.id/login)**. Scan QR Code dengan scanner QR atau tombol foto dari aplikasi whatsapp, kakak akan diarahkan masuk ke dalam situs.  
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

## Login Menggunakan WhatsAuth
API whatsauth dapat digunakan untuk pengembangan implementasi SSO, login menggunakan QR dan Google SignIn. Buat repo baru yang berisi 3 file utama, yaitu:
1. index.html : File html utama yang memanggil js qr dan gsi
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta http-equiv="X-UA-Compatible" content="IE=edge">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>WhatsAuth | Free WhatsApp API OTP Notif Broadcast Gratis</title>
       <link href="style.css" rel="stylesheet">
   	<script src="qr.js" type="module"></script>
   	<script src="gsi.js" type="module"></script>
   </head>
   <body>
    <div id="hasphonenumber" class="w-full h-screen bg-blue-100 flex items-center justify-center">
        <div class="w-96 bg-white rounded-xl">
            <p class="font-bold text-center mb-4" id="useracclog">Tap/Scan dengan <a href="./camwab.jpg" target="_blank">Camera WA</a></p>
            <div class="flex justify-center mt-2 mb-4" id="whatsauthqr">
                <img src="loading.svg">
            </div>
            <p class="font-bold text-center mb-4" id="whatsauthcounter">counter</p>
            <p class="font-bold text-center mb-4" id="logs"><a href="https://wa.my.id">WhatsAuth Free WhatsApp Notif, OTP, API Gateway Gratis</a></p>
        </div>
    </div>
   </body>
   </html>   
   ```
2. qr.js : pengaturan login menggunakan qr
   ```js
   import {qrController,deleteCookie} from "https://cdn.jsdelivr.net/gh/whatsauth/js@0.2.1/whatsauth.js";
   import { wauthparam } from "https://cdn.jsdelivr.net/gh/whatsauth/js@0.2.1/config.js";
   
   wauthparam.auth_ws="d3NzOi8vYXBpLndhLm15LmlkL3dzL3doYXRzYXV0aC9wdWJsaWM=";
   wauthparam.keyword="aHR0cHM6Ly93YS5tZS82Mjg3NzUyMDAwMzAwP3RleHQ9d2g0dDVhdXRoMA==";
   wauthparam.tokencookiehourslifetime=18;
   wauthparam.redirect ="/auth"
   deleteCookie(wauthparam.tokencookiename);
   qrController(wauthparam);
   ```
3. gsi.js : pengaturan login menggunakan google sign in
   ```js
   import {setCookieWithExpireHour,getCookie} from "https://cdn.jsdelivr.net/gh/jscroot/lib@0.0.4/cookie.js";
   import {postJSON} from "https://cdn.jsdelivr.net/gh/jscroot/lib@0.0.4/api.js";
   import {redirect} from "https://cdn.jsdelivr.net/gh/jscroot/lib@0.0.4/url.js";
   import {addCSSInHead,addJSInHead} from "https://cdn.jsdelivr.net/gh/jscroot/lib@0.1.6/element.js";
   import Swal from 'https://cdn.jsdelivr.net/npm/sweetalert2@11/src/sweetalert2.js';
   
   
   await addCSSInHead("https://cdn.jsdelivr.net/npm/sweetalert2@11/dist/sweetalert2.css");
   
   const url="https://asia-southeast2-awangga.cloudfunctions.net/bukupedia/auth/users";
   
   const client_id="239713755402-4hr2cva377m43rsqs2dk0c7f7cktfeph.apps.googleusercontent.com";
   
   // Panggil fungsi untuk menambahkan elemen
   appendGoogleSignin(client_id,url);
   
   
   // Buat fungsi untuk memanggil gsi js dan menambahkan elemen div ke dalam DOM
   async function appendGoogleSignin(client_id, target_url) {
       try {
           // Memuat script Google Sign-In
           await addJSInHead("https://accounts.google.com/gsi/client");
           // Menginisialisasi Google Sign-In dan menetapkan gSignIn sebagai callback
           google.accounts.id.initialize({
               client_id: client_id,
               callback:  (response) => gSignIn(response, target_url), // Menggunakan gSignIn sebagai callback untuk Google Sign-In
           });
           // Memunculkan pop-up Google Sign-In
           google.accounts.id.prompt();
           console.log('Google Sign-In open successfully!');
       } catch (error) {
           console.error('Failed to load Google Sign-In script:', error);
       }
   }
   
   async function gSignIn(response, target_url) {
       try {
           const gtoken = { token: response.credential };
           await postJSON(target_url, "login", getCookie("login"), gtoken, responsePostFunction);
       } catch (error) {
           console.error("Network or JSON parsing error:", error);
           Swal.fire({
               icon: "error",
               title: "Network Error",
               text: "An error occurred while trying to log in. Please try again.",
           });
       }
   }
   
   function responsePostFunction(response) {
       if (response.status === 200 && response.data) {
           console.log(response.data);
           setCookieWithExpireHour('login',response.data.token,18);
           redirect("/dashboard");
       } else {
           console.error("Login failed:", response.data?.message || "Unknown error");
           Swal.fire({
               icon: "error",
               title: "Login Failed",
               text: response.data?.message || "Anda belum terdaftar dengan login google, silahkan tap atau scan qr dahulu untuk pendaftaran.",
           }).then(() => {
               redirect("/login");
           });
       }
   }
   ```

Untuk file-file tambahan:
1. [style.css](style.css)
2. [loading.svg](loading.svg)

Jangan lupa untuk menambahkan domainnya di Console APIs&Services - Client ID for Web application  
![image](https://github.com/user-attachments/assets/12191720-6f6c-4155-bf75-d966b0f60c4c)

Contoh URL webhook yang didaftarkan ke apidocs:  
```txt
https://asia-southeast2-awangga.cloudfunctions.net/bukupedia/webhook/nomor/6287752000300
```

## Tidak merespon pesan
Jika pesan yang dikirim tidak mendapatkan balasan dari webhook lebih dari 1 menit sejak pesan dikirim, maka coba langkah ini:
1. Mencoba mengirimkan notif pesan kepada nomor telepon tujuan. Buka API message klik Try it out, isi to,isgroup dan message. Ketika klik execute maka akan ada notif "device belum di start".
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/74d73883-2c91-4c22-a35c-1a4e2ef88977)  
2. Start device pada bagian API device. Klik Try it out, kemudian masukkan token pada langkah sebelumnya. Ketika execute, maka akan ada balasan message dari server.
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/2eaf0098-6e91-4733-b7ca-96e2483e5b58)  
