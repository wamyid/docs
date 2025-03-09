# WhatsAuth : Free 2FA, OTP, Notif, WhatsApp Gateway API Gratis

WhatsAuth menghadirkan solusi untuk :
1. Single Sign On
2. 2FA (2 Factor Auth)
3. OTP (One Time Password)
4. Notifikasi WA
5. WhatsApp API Gateway untuk integrasi dengan aplikasi anda

Struct WhatsAuth
```go
package model
//Header yang dikirim ke webhook
type Header struct {
	Secret string `reqHeader:"secret"`
}
//Body Message yang dikirim ke webhook
type WAMessage struct {
	Phone_number       string  `json:"phone_number,omitempty" bson:"phone_number,omitempty"`
	Reply_phone_number string  `json:"reply_phone_number,omitempty" bson:"reply_phone_number,omitempty"`
	Chat_number        string  `json:"chat_number,omitempty" bson:"chat_number,omitempty"`
	Chat_server        string  `json:"chat_server,omitempty" bson:"chat_server,omitempty"`
	Group_name         string  `json:"group_name,omitempty" bson:"group_name,omitempty"`
	Group_id           string  `json:"group_id,omitempty" bson:"group_id,omitempty"`
	Group              string  `json:"group,omitempty" bson:"group,omitempty"`
	Alias_name         string  `json:"alias_name,omitempty" bson:"alias_name,omitempty"`
	Message            string  `json:"messages,omitempty" bson:"messages,omitempty"`
	EntryPoint         string  `json:"entrypoint,omitempty" bson:"entrypoint,omitempty"`
	From_link          bool    `json:"from_link,omitempty" bson:"from_link,omitempty"`
	From_link_delay    uint32  `json:"from_link_delay,omitempty" bson:"from_link_delay,omitempty"`
	Is_group           bool    `json:"is_group,omitempty" bson:"is_group,omitempty"`
	Filename           string  `json:"filename,omitempty" bson:"filename,omitempty"`
	Filedata           string  `json:"filedata,omitempty" bson:"filedata,omitempty"`
	Latitude           float64 `json:"latitude,omitempty" bson:"latitude,omitempty"`
	Longitude          float64 `json:"longitude,omitempty" bson:"longitude,omitempty"`
	LiveLoc            bool    `json:"liveloc,omitempty" bson:"liveloc,omitempty"`
}
//Response kembalian dari WebHook
type Response struct {
	Response string `json:"response"`
}
```

## Persiapan WhatsApp Gateway

[![WebHook](https://img.youtube.com/vi/-I-Mf0pQMxk/0.jpg)](https://www.youtube.com/watch?v=-I-Mf0pQMxk)

Tahapan ini dilakukan terlebih dahulu sebelum melakukan pendaftaran, hal-hal yang harus dipersiapkan antara lain:
1. Siapkan Nomor WhatsApp yang akan dijadikan Gateway API
2. Siapkan URL WebHook sebagai penerima pesan masuk. Jika menggunakan [GoCroot](https://gocroot.if.co.id/) URL webhook akan tampak seperti ini `https://asia-southeast2-awangga.cloudfunctions.net/logiccoffee/webhook/nomor/62881022526506`. ganti `62881022526506` dengan nomor whatsapp anda dengan format 628xxx. dan sesuaikan URL `asia-southeast2-awangga` dan `logiccoffee` sesuai dengan project dan nama GCF.  
   ![image](https://github.com/user-attachments/assets/c348b3f3-b5bf-4cc4-b6d1-2ce82ead218a)
   Contoh URL webhook yang didaftarkan ke apidocs:  
   ```txt
   https://asia-southeast2-awangga.cloudfunctions.net/bukupedia/webhook/nomor/6287752000300
   ```
4. Buat dahulu database dengan nama yang sesuai dengan keinginan kemudian edit pada file db.go folder config nama database yang sudah dibuat.
   ```go
   var mongoinfo = atdb.DBInfo{
	   DBString: MongoString,
	   DBName:   "naskah",
   }
   ```
   Contoh diatas adalah nama database nya naskah
5. Pengguna GoCroot cukup untuk membuat collection baru bernama profile di dalam database kemudian isi dengan json:
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
   Silahkan sesuaikan pada bagian token, phonenumber dan secret. Untuk nama bot sesuaikan di bagian botname dan triggerword. url disamakan dengan url webhook.
6. Pengguna GoCroot untuk login menggunakan google sign in, maka buat juga collection credentials yang berisi:
   ```json
   {
	  "token": "##TOKEN##",
	  "refresh_token": "##RTOKEN##",
	  "token_uri": "https://oauth2.googleapis.com/token",
	  "client_id": "##Client_ID##",
	  "client_secret": "##CLIENTSECRET##",
	  "scopes": [
	    "https://www.googleapis.com/auth/spreadsheets",
	    "https://www.googleapis.com/auth/documents",
	    "https://www.googleapis.com/auth/drive",
	    "https://www.googleapis.com/auth/blogger",
	    "https://www.googleapis.com/auth/gmail.send",
	    "https://www.googleapis.com/auth/gmail.readonly",
	    "https://www.googleapis.com/auth/calendar",
	    "https://www.googleapis.com/auth/drive.file"
	  ],
	  "expiry": "2024-06-13T17:27:35.666Z",
	  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
	  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
	  "javascript_origins": [
	    "https://whatsauth.my.id",
	    "https://wa.my.id",
	    "https://www.do.my.id"
	  ],
	  "project_id": "awangga",
	  "redirect_uris": [
	    "https://wa.my.id/gsign/",
	    "https://www.do.my.id/login/"
	  ],
	  "tokentype": "Bearer",
	  "accesstoken": "##ACCESSTOKEN##"
   }
   ```
   Jangan lupa untuk setting PRKEY yang berisi private key paseto pada setting env atau secrets settings

## Pendaftaran WhatsApp Gateway Melalui Interface Web
Proses nya pertama **login dulu di [wa.my.id](https://wa.my.id/login)** dengan urutan :
1. Buka laman **[login wa.my.id](https://wa.my.id/login)**. Scan QR Code dengan scanner QR atau tombol foto dari aplikasi whatsapp, kakak akan diarahkan masuk ke dalam situs.  
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/a4da833b-e267-4f1e-be93-c9f2244b55e2)  
2. Input URL dan Secret Webhook kakak terus klik submit.
3. Masukkan Pair Code ke WhatsApp yang ada di Handphone tunggu beberapa saat sampai proses loading di handphone selesai.  
   ![WhatsApp Image 2023-11-07 at 01 07 50_3f9cbb85](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/a3e3bca7-d78e-4f74-a2fb-34ef850e91c3)  
   ![WhatsApp Image 2023-11-07 at 01 07 45_d9155096](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/9e44609e-321d-43f6-b760-6a8f038a7411)  
   ![WhatsApp Image 2023-11-07 at 01 07 39_e0a1d259](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/249fab3a-7bba-4b50-b160-41cd6fa825db)  
   Setelah itu beri nama whatsauth dan akan ada linked device yang baru pada whatsapp anda
   ![WhatsApp Image 2024-11-20 at 18 10 19_87591c0f](https://github.com/user-attachments/assets/a2391f42-addb-451e-a2c4-e4727cc9f52a)  
5. Simpan token sementara yang muncul untuk digunakan di laman [apidocs](https://wa.my.id/apidocs/#/signup/signUpNewUser) untuk ditukar menjadi token yang berlaku selama 30 hari.  
   ![image](https://github.com/whatsauth/docs/assets/11188109/7e9548f6-0f3f-4892-95ce-12e7d645c698)  
6. Kita akan melakukan uji pengiriman pesan ke nomor orang lain untuk memastikan WhatsApp kita sudah terdaftar dengan baik. Masuk menu Kirim Pesan untuk mengirimkan pesan.  
   ![image](https://github.com/whatsauth/docs/assets/11188109/83c31870-4f31-411e-871a-5b41b020717d)  
   ![image](https://github.com/whatsauth/docs/assets/11188109/81aa28df-10f8-4ebb-af6a-4ba9b98e8582)  
   Tunggu beberapa menit maka pesan akan sampai ke tujuan, pastikan nomor tujuan tidak memblokir nomor pengirim.
7. Buka [Dokumen api](https://wa.my.id/apidocs/#/signup/signUpNewUser) untuk menukar token langkah sebelumnya menjadi token yang berlaku selama 30 hari.
8. Klik bagian Authorize dan masukkan token ke dalam kolom Value: dan klik Authorize  
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/78d313a7-345f-40fe-9cf6-7cbf58fbba2e)  
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/54826caf-597a-4151-938c-bbb077b23741)
9. Klik API signup, klik Try it out. Kemudian masukkan URL dan Secret dari WebHook yang sudah dibuat sebelumnya. Lihat respon, simpan baik baik token yang diterima, token tersebut berlaku selama 30 hari.
   ![image](https://github.com/whatsauth/whatsauth.github.io/assets/11188109/fd89a320-3228-4cad-85d8-ecefd9a324e5)
   ![image](https://github.com/whatsauth/docs/assets/11188109/c1573feb-d39b-4c33-8b29-a5ba9708e299)  
   Untuk pengguna GoCroot, pakai token balasan tersebut untuk di update ke dalam colection profile database mongo yang digunakan WeebHook
   
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

[![Tutorial Web](https://img.youtube.com/vi/2erAXAWQB6Q/0.jpg)](https://www.youtube.com/watch?v=2erAXAWQB6Q)

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
   [![Setting Parameter](https://img.youtube.com/vi/2667pmLihLo/0.jpg)](https://www.youtube.com/watch?v=2667pmLihLo)  
   ```js
   import {qrController,deleteCookie} from "https://cdn.jsdelivr.net/gh/whatsauth/js@0.2.1/whatsauth.js";
   import { wauthparam } from "https://cdn.jsdelivr.net/gh/whatsauth/js@0.2.1/config.js";
   
   wauthparam.auth_ws="d3NzOi8vYXBpLndhLm15LmlkL3dzL3doYXRzYXV0aC9wdWJsaWM=";
   wauthparam.keyword="aHR0cHM6Ly93YS5tZS82MjgzMTMxODk1MDAwP3RleHQ9d2g0dDVhdXRoMA==";
   wauthparam.tokencookiehourslifetime=18;
   wauthparam.redirect ="/auth"
   deleteCookie(wauthparam.tokencookiename);
   qrController(wauthparam);
   ```
   Ubah variabel `wauthparam.keyword` disesuaikan dengan nomor yang di daftarkan di WhatsAuth. Gunakan Base64 Decode dan Encode dengan melakukan update dari string keyword diatas untuk di update.
   ![image](https://github.com/user-attachments/assets/8e24a1ac-7249-45d3-8823-ee7f07c82058)  

4. gsi.js : pengaturan login menggunakan google sign in
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

## Repo Auth

Untuk melakukan otorisasi antara frontend dan backend maka diperlukan satu repo tambahan bernama auth(ada di settingan qr.js yaitu /auth).
Pada file index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="index.js" type="module"></script>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Selamat Datang di Naskah Bukupedia</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="scene">
        <div class="shadow"></div>
        <div class="jumper">
          <div class="spinner">
            <div class="scaler">
              <div class="loader">
                <div class="cuboid">
                  <div class="cuboid__side"></div>
                  <div class="cuboid__side"></div>
                  <div class="cuboid__side"></div>
                  <div class="cuboid__side"></div>
                  <div class="cuboid__side"></div>
                  <div class="cuboid__side"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      
      </div>
</body>
</html>
```

Pada file index.js
```js
import {getCookie} from "https://cdn.jsdelivr.net/gh/jscroot/lib@0.1.8/cookie.js";
import {getJSON} from "https://cdn.jsdelivr.net/gh/jscroot/lib@0.1.8/api.js";
import {redirect} from "https://cdn.jsdelivr.net/gh/jscroot/lib@0.1.8/url.js";

if (getCookie("login")){
    getJSON("https://asia-southeast2-awangga.cloudfunctions.net/florka/data/user","login",getCookie("login"),responseFunction);
}else{
    redirect("/login");
}


function responseFunction(result){
    console.log(result);
    if (result.status === 200){
        redirect("/dashboard");
    }else{
        redirect("/daftar");
    }
}
```
dan file [style.css](./auth/style.css)

## Integrasi WhatsAuth dengan JSCroot dan GOCroot

[![Tutorial Web](https://img.youtube.com/vi/LDQ8Ty8B9eM/0.jpg)](https://www.youtube.com/watch?v=LDQ8Ty8B9eM)  

Untuk pengembangan sendiri. Silahkan buka [Panduan Deployment WebHook](/webhook)

