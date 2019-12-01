---
comments: true
---

Hai, kali ini aku mau sharing tentang cara implementasi sederhana **Redis**. Sebelumnya mungkin aku jelaskan sedikit tentang apa itu **Redis** dan kegunaannya.

**Redis** adalah _in-memory data structure store_, dia digunakan sebagai _database_, _cache_ dan _message broker_. Lebih jelasnya silahkan baca di <a href="https://redis.io/" target="_blank">websitenya</a>.

Dan kali ini kita akan gunakan **Redis** ini untuk _caching_ respon API. _Caching_ ini berguna banget untuk meningkatkan performa aplikasi.

Skenario penggunaanya seperti ini, ketika client request endpoint jenis akomodasi `/accommodation-types` dan itu adalah awal pertama request maka sistem akan mengambil data tipe akomodasi dari database utama.

Setelah itu sistem akan menyimpan itu dalam redis cache terlebih dahulu dengan umur yang secukupnya sebelum memberikan respon ke client. Jadi ketika ada request lagi dari client, sistem akan mengecek pada cache terlebih dahulu. Jika ada, maka data dari redis itu yang diberikan ke client. Jika tidak, sistem melakukan proses seperti diawal tadi.

Btw, pilihlah data yang cenderung statis untuk disimpan di cache ya. Dan contoh implementasi ini dengan PHP ya hehe.

### Yang dibutuhkan:
- Redis sudah terinstall

```bash
ikrenet :: ~ » redis-cli --version
redis-cli 5.0.6
ikrenet :: ~ » redis-server --version
Redis server v=5.0.6 sha=00000000:0 malloc=jemalloc-5.2.1 bits=64 build=862f233732e771fd
```

- Pastikan servis redis jalan ya.

```bash
ikrenet :: ~ » sudo systemctl start redis.service
ikrenet :: ~ » systemctl status redis.service
● redis.service - Advanced key-value store
    Loaded: loaded (/usr/lib/systemd/system/redis.service; disabled; vendor preset: disabled)
    Active: active (running) since Sat 2019-11-30 13:59:53 WIB; 2s ago
  Main PID: 19726 (redis-server)
     Tasks: 4
    Memory: 31.0M
    CGroup: /system.slice/redis.service
            └─19726 /usr/bin/redis-server 127.0.0.1:6379

```

- Pastikan library redis nya sudah terinstall. Installnya pakai composer aja.

```bash
ikrenet :: ~ » composer require predis/predis
```

### Cara menggunakan:
- Berikut kode php nya dan btw ini aku pake framework Slim.

```php
<?php
namespace App\Controllers;

use Slim\Http\Request;
use Slim\Http\Response;
use App\DataMapper;
use App\Transformers\Accommodation\AccommodationTypeTransformer;

class AccommodationTypeController extends BaseController
{
    public function all(Request $request, Response $response)
    {

    // init redis
    $redis = new Predis\Client(array(
        'host' => $settings['host'], // 127.0.0.1
        'port' => $settings['port']  // 6379
    ));

    // mengambil data dari cache
    $cache = $redis->get('accommodation_type');

    // melakukan pengecekan apakah cache tersedia
    if (empty($cache)) {
        // mengambil data dari database utama

        // init data mapper
        $dataMapper = new DataMapper($this->container->db);
        $mapper = $dataMapper->getMapper('accommodation_type');;
        $accommodationTypes = $mapper->get();
        if (empty($accommodationTypes)) {
            return $this->emptyResponse('tipe akomodasi');
        }

        $array = $this->collectionTransform($accommodationTypes, new AccommodationTypeTransformer);

        // menyimpan dalam cache dengan umur satu minggu
        $redis->set('accommodation_type', json_encode($array));
        $redis->expireat('accommodation_type', strtotime('+1 week'));
    } else {
        // mengambil data dari cache
        $array = json_decode($cache);
    }

    return $response->write(json_encode($array, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT));
}

```

- Dari kode di atas cukup paham kan ya?. Implementasi redisnya hanya kode dibawah ini.

```php
// init redis
$redis = new Predis\Client(array(
    'host' => $settings['host'], // 127.0.0.1
    'port' => $settings['port']  // 6379
));

// mengambil data dari cache
$cache = $redis->get('accommodation_type');

// menyimpan dalam cache dengan umur satu minggu
$redis->set('accommodation_type', json_encode($array));
$redis->expireat('accommodation_type', strtotime('+1 week'));
```

Sebenernya masih banyak lagi, namun aku kira itu sudah cukup. Jika ingin tahu lebih detail jangan pernah lelah baca dokumentasi di webnya ya!.

Terima kasih.
