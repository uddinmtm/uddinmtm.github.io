---
comments: true
---
Hai, kali ini aku coba melakukan eksperimen dengan menggunakan bash script, sekalian sambil belajar. Dengan bash script, aku mau generate thumbnail image dari sebuah folder.

Sebelumnya aku udah mencoba melakukan konfersi gambar dengan menggunakan perintah `convert` dari imagemagick. Selanjutnya dengan perintah itu juga, aku akan generate thumbnail image, namun dengan beberapa tambahan bash script.

Langkah :
1. Perintah `convert` pastikan jalan.
2. Persiapkan gambar - gambar yang mau digenerate.
3. Buat folder baru untuk lokasi file thumbnail.
4. Buat file bash scriptnya dan taruh di lokasi yang sama dengan gambar - gambar yang akan diproses.
5. Berikan permission untuk execute file base scriptnya dengan perintah `chmod +x nama_file` .
6. Jalankan.

Berikut isi file bash nya:

```bash
#!/bin/sh

# loop filenames from ls command
for file in $(ls)
do
    # convert file with option resize 50% to folder test
    convert -resize 50% $file test/$file
done
```

Terima kasih.








