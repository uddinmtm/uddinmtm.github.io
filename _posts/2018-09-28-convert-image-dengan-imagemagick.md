---
comments: true
---
Sebelumnya, aku kalo mau convert image, sering pake Gimp. Atau terkadang pake converter image online yang bertebaran di internet, padahal hanya mau convert sederhana seperti jpg to png atau resize ukurannya saja.

Dan karena aku juga tidak begitu passion di design, jadi aplikasi seperti Gimp jarang sekali terpakai. Trus aku coba browsing, kira-kira ada gak ya tools untuk convert image via terminal?, eh ternyata ada pake imagemagick dengan tool convert nya.

Untuk menggunakannya, pastikan aplikasi ImageMagick nya terinstall dulu ya.

_install_
```bash
# pacman -S imagemagick
```
_check version_
```bash
$ magick -version
Version: ImageMagick 7.0.8-10 Q16 x86_64 2018-08-17 https://www.imagemagick.org
Copyright: © 1999-2018 ImageMagick Studio LLC
License: https://www.imagemagick.org/script/license.php
Features: Cipher DPC HDRI Modules OpenCL OpenMP
Delegates (built-in): bzlib cairo fontconfig freetype gslib heic jng jp2 jpeg lcms lqr ltdl lzma openexr pangocairo png ps raw rsvg tiff webp wmf x xml zlib
```


Begini penggunaan sederhananya :

1. convert image dari png ke jpg.
```bash
$ convert original.png result.jpg
```

2. convert image dari ukuran asli ke ukuran 50% nya.
```bash
$ convert -resize 50% original.png result.png
```

Dokumentasi lengkapnya, silahkan kunjungi <https://www.imagemagick.org/script/convert.php>.


Semoga bermanfaat.