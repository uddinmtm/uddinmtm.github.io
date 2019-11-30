---
comments: true
---

<a href="/assets/images/2019-11-16-113324_1366x768_scrot.png" target="_blank">
    <img src="/assets/images/2019-11-16-113324_1366x768_scrot.png" style='width:100%;' border="0" alt="landing page chat-api.com">
</a>


Hai, aku mau sharing pengalaman mencoba layanan dari <a href="https://chat-api.com" target="_blank">**chat-api.com**</a> (versi trial) untuk mengirim pesan WA lewat API.

### Yang dibutuhkan:
- Handphone yang sudah terinstall WA.
- Buat akun di **chat-api.com**.

### Cara menggunakan:
- Login dulu ke **chat-api.com**.
- Tambahkan atau daftarkan _instance_ / akun whatsapp kita ke **chat-api.com**. Cukup mudah, hanya tinggal scan qr-code yang tampil di webnya, mirip saat melakukan scan qr-code pada **web.whatsapp.com**.
- Jika akun whatsapp kita sudah terdaftar, kita akan mendapatkan **API URL** dan **Token** untuk menggunakan layanan API nya. Namun pastikan WA handphone tetap konek ya, mirip perlakuan ke **web.whatsapp.com**.
- Selanjutkan kita akan mencoba mengirim pesan teks dan file melalui layanan API yang diberikan. Sebelumnya kita sudah mendapatkan API URL **"https://api.chat-api.com/instance99999"**  dan Token **"d7ex1xxxxxxxxxxx"**.

Berikut source percobaanya menggunakan PHP.

### Send Messsage
Di sini aku coba kirim pesan ke 6289678528565 (62 kode negara harus diberikan) dengan pesan "Hello!".
```php
<?php
$data = [
    'phone' => '6289678528565', // Receivers phone
    'body' => 'Hello!', // Message
];
$json = json_encode($data); // Encode data to JSON
// URL for request POST /message
$url = 'https://api.chat-api.com/instance99999/sendMessage?token=d7ex1xxxxxxxxxxx';
// Make a POST request
$options = stream_context_create(['http' => [
        'method'  => 'POST',
        'header'  => 'Content-type: application/json',
        'content' => $json
    ]
]);
// Send a request
$result = file_get_contents($url, false, $options);
```
Kita dapat response berupa json
```
{
    "sent": true,
    "message": "Sent to 6289678528565@c.us",
    "id": "true_6289678528565@c.us_3EB0B34410B0AC336410",
    "queueNumber": 5
}
```
Pesan akan masuk antrian pengiriman terlebih dahulu.

### Send File
Oke, lanjut coba kirim pesan ke nomor yang sama dengan file pdf. Kita cukup menyiapkan link file nya aja dan itu harus online dan menggunakan ssl (https) seperti ini https://www.soundczech.cz/temp/lorem-ipsum.pdf.
```php
<?php
$data = [
    'phone' => '6289678528565', // Receivers phone
    'body' => 'https://www.soundczech.cz/temp/lorem-ipsum.pdf', // HTTP link
    'filename' => 'lorem-ipsum.pdf', // File name
    'caption' => 'Test kirim file pdf' // Text under the file
];
$json = json_encode($data); // Encode data to JSON
// URL for request POST /message
$url = 'https://api.chat-api.com/instance99999/sendFile?token=d7ex1xxxxxxxxxxx';
// Make a POST request
$options = stream_context_create(['http' => [
        'method'  => 'POST',
        'header'  => 'Content-type: application/json',
        'content' => $json
    ]
]);
// Send a request
$result = file_get_contents($url, false, $options);
```
Kita dapat response berupa json
```
{
    "sent": true,
    "message": "Sent to 6289678528565@c.us",
    "id": "true_6289678528565@c.us_3EB05F97986EE8B82F01",
    "queueNumber": 6
}
```

Btw, dokumentasi yang diberikan di webnya sudah lengkap banget. Ini hanya sedikit cuplikannya saja. Oh iya, proses pengiriman pesannya kadang tidak sampai satu menit. Silahkan dicoba!.

Semoga bermanfaat.
