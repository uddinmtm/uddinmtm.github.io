---
comments: true
---

Web sistem inventori sederhana ini aku bangun pake PHP. Aku coba pisahkan antara tampilan (frontend) dan logika aplikasinya atau pengolahan datanya (backend), sehingga akan banyak menggunakan konsep <a href="https://www.petanikode.com/javascript-ajax/">ajax</a>.

Struktur foldernya seperti ini.
- **inventori**
    - **public** _(root folder aplikasi)_
        - **actions** 
            - items.php
            - login.php
            - logout.php
        - **css** _(dari template)_
        - **fragments**
        - **js** _(dari template)_
        - **template-files** _(dari template)_
        - **vendor** _(dari template)_
        - dashboard.php
        - index.php
        - m_items.php
    - middleware.php 

Semisal untuk halaman login, untuk sisi tampilan aku buat file _`public/index.php`_ sedang untuk pengolahan aku buat file _`public/actions/login.php`_.

Pada halaman login ini, setelah si user meng-klik tombol Login, dengan bantuan javascript jQuery kulakukan ajax _request_ dengan metode POST ke file _backend_. Yang selanjutnya _backend_ akan merespon dengan status _success_ atau _error_. Jika _success_, maka halaman akan dipindahkan ke halaman _dashboard_.

Seperti ini:

```javascript
$('#frmLogin').submit(function(e) {
    $.ajax({
        url: 'actions/login.php', 
        method: 'POST', 
        data: $(this).serialize(),
        success: function(res) {
            window.location = 'dashboard.php';
        },
        error: function(res) {
            console.log(res);
        }
    });

    e.preventDefault();
});
```
Link <a href="https://raw.githubusercontent.com/uddinmtm/inventori/master/public/index.php" target="_blank">file</a>.
