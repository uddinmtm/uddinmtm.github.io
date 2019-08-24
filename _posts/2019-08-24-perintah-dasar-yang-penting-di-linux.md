---
comments: true
---

Berikut adalah beberapa perintah dasar linux yang penting untuk diketahui karena akan sering digunakan.

1. `ls` - list directory content.
2. `cd` - change working directory.
3. `mkdir` - make diretories.
4. `touch` - change file timestamp. Namun biasanya digunakan untuk create file.
5. `rm` - remove riles or directories.
6. `cat` - concatenate files and print on the standard output.
7. `tail` - output the last part of files.
8. `pwd` - print name of current/working directory.
9. `echo` - display a line of text.
10. `man` - an interface to the on-line reference manuals.
11. `du` - estimate file space usage.
11. `df` - report file system disk space usage.
12. `less` - opposite of more.
13. `more` - file perusal filter for crt viewing.
13. `grep` - print lines that match patterns.

Contoh penggunaan :
1. Menampilkan isi folder secara detail, urut sesuai tanggal, dan ukuran filenya mudah dibaca.

    ```bash
    $ ls -lht
    total 48K
    -rwxr-xr-x 1 ikr ikr  115 Aug  8 09:19 queue.sh
    drwxr-xr-x 3 ikr ikr 4.0K Jul 29 10:15 php-app
    drwxr-xr-x 4 ikr ikr 4.0K Jul 18 21:45 slstatus
    drwxr-xr-x 3 ikr ikr 4.0K Jul 18 21:43 dwm
    drwxr-xr-x 3 ikr ikr 4.0K Jul 17 09:53 surf
    drwxr-xr-x 3 ikr ikr 4.0K Jul 13 17:16 tabbed
    drwxr-xr-x 3 ikr ikr 4.0K Jul 13 00:39 dmenu
    drwxr-xr-x 3 ikr ikr 4.0K Jul 13 00:38 st
    drwxr-xr-x 3 ikr ikr 4.0K Jul 11 06:59 xssstate
    drwx------ 6 ikr ikr 4.0K Jun 14 18:55 anydesk-5.1.1
    drwxr-xr-x 3 ikr ikr 4.0K Aug 12  2018 slock

    ```
2. Pindah ke direktori home user dengan cepat.
    ```bash
    $ cd ~
    ```
2. Pindah ke direktori aktif sebelumnya dengan cepat.
    ```bash
    $ cd -
    ```
3. Keluar satu direktory dari direktori yang sedang aktif.
    ```bash
    $ cd ..
    ```
4. Membuat direktori baru di dalam sebuah direktori.
    ```bash
    $ mkdir app/new-directory
    ```
5. Membuat sebuah file baru dengan perintah touch.
    ```bash
    $ touch new.txt
    ```
6. Menghapus sebuah direktori beserta isinya.
    ```bash
    $ rm -r old-files/
    ```
7. Melihat isi sebuah file yang panjang dan menampilkannya sebagian.
    ```bash
    $ cat README.txt | less
    ```
8. Melihat bagian akhir dari isi sebuah file sebanyak 10 baris.
    ```bash
    $ tail -n 10 log.txt
    ```
9. Mengetahui sekarang aktif di direktori mana.
    ```bash
    $ pwd
    ```
10. Mencetak sebuah text di terminal.
    ```bash
    $ echo 'Ini teks gak penting'
    ```
11. Mencetak sebuah text ke dalam sebuah file.
    ```bash
    $ echo 'Ini teks gak penting' > text.txt
    $ echo 'Ini teks gak penting banget' >> text.txt
    ```
12. Melihat dokumentasi perintah `man`.
    ```bash
    $ man man
    ```
13. Mengetahui ukuran dari sebuah folder
    ```bash
    $ du -sh Documents/
    17G     Documents
    ```
14. Mengetahui info partisi.
    ```bash
    $ df -h
    ```
15. Menampilkan sebuah konten yang panjang per halaman
    ```bash
    $ ps aux | more
    $ ps aux | less
    ```
    `more` akan lanjut terus sampai baris akhir.

    `less` bisa kembali ke awal.
16. Menampilkan isi sebuah direktori yang mengandung kata '.php'
    ```bash
    $ ls -l my-web/ | grep '.php'
    ```

Semoga bermanfaat dan sekian terima kasih
