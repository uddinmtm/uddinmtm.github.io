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

- Sisi _frontend_

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

- Sisi _backend_

```php
...

$username = mysqli_real_escape_string($conn, $_POST['username']);
$password = md5($_POST['password']);

$sql = "SELECT * FROM m_user WHERE username='$username'";
$results = mysqli_query($conn, $sql);
if (mysqli_num_rows($results) == 0) {
    echo json_encode(['message' => 'username not found']);
    http_response_code(400);
    die();
}

$user =  mysqli_fetch_assoc($results);
if ($user['password'] !== $password) {
    echo json_encode(['message' => 'wrong password']);
    http_response_code(400);
    die();
}

// set session
$_SESSION["name"] = $user['name'];
$_SESSION["username"] = $user['username'];

echo json_encode("success");
```
Link <a href="https://raw.githubusercontent.com/uddinmtm/inventori/master/public/actions/login.php" target="_blank">file</a>.

Untuk contoh CRUD _(create, read, update, delete)_ bisa dilihat di file Master User. Berikut link <a href="https://raw.githubusercontent.com/uddinmtm/inventori/master/public/m_items.php" target="_blank">frontend</a>, <a href="https://raw.githubusercontent.com/uddinmtm/inventori/master/public/actions/items.php" target="_blank">backend</a>.

Untuk project secara keseluruhannya bisa diambil di sini <a href="https://github.com/uddinmtm/inventori" target="_blank">github</a>. Disini aku mengerjakannya dengan temen ku, <a href="https://github.com/msapuan" target="_blank">Sapuan</a> namanya.
