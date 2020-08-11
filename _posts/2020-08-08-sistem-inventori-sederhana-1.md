---
comments: true
---

Aku mencoba membuat sebuah web sistem inventori sederhana mulai dari rancangan sampai menjadi aplikasi web. Sistem ini sangat sederhana, dia hanya akan memberikan output berupa laporan stok dan untuk proses inputnya hanya mencatatat barang yang masuk dan keluar.

Langsung saja, mulai dari oret-oretan awal.

### DFD Level 0:
Sistem ini akan dijalankan oleh 2 tipe user yaitu Admin dan Manager. Nantinya user manager hanya akan menerima output dari program yaitu Laporan Stok. Sedangkan untuk user admin dia akan lebih banyak melakukan input ke program. 

Gambarannya seperti berikut:

<a href="/assets/images/dfd-0-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/dfd-0-sistem-inventori.jpg" style='width:100%;' border="0" alt="DFD Level 0">
</a>

### DFD Level 1:
Berikut untuk lebih detail dari apa saja aktivitas user admin.

<a href="/assets/images/dfd-1-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/dfd-1-sistem-inventori.jpg" style='width:100%;' border="0" alt="DFD Level 1">
</a>

### ERD:
Dan ini adalah ERD, yang nantinya akan menjadi rancangan dari bentuk database nya.

<a href="/assets/images/erd-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/erd-sistem-inventori.jpg" style='width:100%;' border="0" alt="ERD">
</a>

Pada beberapa gambar di atas, hanya aku tampilkan proses pokok dari aplikasi saja. Untuk proses autentikasi (login/logout) tidak aku tampilkan karena aku anggap kita sudah banyak mengetahui prosesnya.

Untuk gambar selanjutnya adalah oret-oretan untuk tampilan webnya.

### Login
<a href="/assets/images/login-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/login-sistem-inventori.jpg" style='width:100%;' border="0" alt="Login Page">
</a>

### Dashboard
<a href="/assets/images/dashboard-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/dashboard-sistem-inventori.jpg" style='width:100%;' border="0" alt="Dashboard Page">
</a>

### Master Barang
<a href="/assets/images/barang-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/barang-sistem-inventori.jpg" style='width:100%;' border="0" alt="Master Barang">
</a>

### Barang Masuk
<a href="/assets/images/barang-masuk-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/barang-masuk-sistem-inventori.jpg" style='width:100%;' border="0" alt="Barang Masuk">
</a>

...

Cukup sampai ini saja, Lanjut ke tahap selanjutnya yaitu membuat database.
<a href="/2020/08/09/sistem-inventori-sederhana-2.html">Selanjutnya</a>
