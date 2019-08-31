---
comments: true
---

Buit-in web server, fitur ini sangat berguna untuk developer web terutama dalam proses development.

### Berikut beberapa keuntungannya :
1. Tidak perlu lagi install program apache sebagai web server. Hanya perlu install PHP saja
2. Lokasi project bisa dimana saja tidak harus di folder htdocs atau www.


### Yang dibutuhkan:
- Progam PHP


### Cara menggunakan:

berikut keterangan dari `man php`
```bash
--server addr:port
-S addr:port   Start built-in web server on the given local address and port

--docroot docroot
-t docroot     Specify the document root to be used by the built-in web server
```
Misalkan saya punya source code dari cms wordpress dan berada di path cms/wordpress. Maka, kita buat built-in web server untuk web wordpress ini seperti ini.

```bash
$ php -S localhost:7676 --docroot=cms/wordpress
PHP 7.3.8 Development Server started at Fri Aug 30 22:02:38 2019
Listening on http://localhost:7676
Document root is /home/ikr/cms/wordpress
Press Ctrl-C to quit.

```

Setelah sukses seperti diatas, untuk melihat webnya cukup dengan akses http://localhost:7676 saja. Dan seperti ini hasil desain tampilannya.

<a href="/assets/images/2019-08-30-220707_1365x715_scrot.png" target="_blank">
    <img src="/assets/images/2019-08-30-220707_1365x715_scrot.png" style='width:100%;' border="0" alt="tampilan wordpress">
</a>

Semoga bermanfaat

Terima Kasih
