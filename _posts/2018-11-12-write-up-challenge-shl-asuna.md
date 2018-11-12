---
comments: true
---
Hai, kali ini aku mau nulis write up challenge dari SHL. Ini adalah WU pertamaku jadi mohon maklum.
Challenge ini kita akan diminta untuk mencari ingatan asuna yang hilang di aplikasi android yang diberikan.

Hal pertama yang aku lakukan adalah menginstall aplikasinya. Setelah itu aku buka, dan berikut adalah tampilan pertamanya.

![tampilan pertama aplikasi](/assets/images/photo_2018-11-12_23-24-01.jpg)

Aku lihat ada pesan *Reverse me dan Dapatkan Flag*, dari pesan itu aku langsung coba extract file .apk di laptop dengan perintah `unzip`. Isi filenya seperti ini.

![gambar hasil extract](/assets/images/photo_2018-11-12_23-26-22.jpg)

Setelah itu, aku coba buka isi filenya dengan menggunakan `vim`, termasuk file classes.dex . Ternyata isinya tidak bisa kebaca. Aku coba lagi dengan perintah _%!xxd_ sedikit bisa terbaca, tapi tidak menemukan sesuatu apapun yang menarik di sana. Aku coba untuk break dahulu setelah itu. Kemudian aku coba browsing di google tentang reversing apk dan mendapat petunjuk dari sini <https://stackoverflow.com/questions/12732882/reverse-engineering-from-an-apk-file-to-a-project>.

Setelah membaca itu, ternyata aku perlu menginstall aplikasi **dex2jar** untuk membaca file _classes.dex_ di dalam folder hasil extract apk nya. Langsung saja aku coba install itu dan kemudian kujalankan dan akhirnya aku mendapatkan file _classes-dex2jar.jar_ .

![gambar proses dex2jar](/assets/images/photo_2018-11-13_05-12-31.jpg)

Kemudian sesuai petunjuk tadi, file *.jar* nya aku buka dengan aplikasi **JD-GUI** . Dan ternyata waw, aku bisa melihat file *.java* nya. 

![gambar saat dibuka di aplikasi jd-gui](/assets/images/2018-11-12-234748_1366x768_scrot.png)

Setelah itu, aku langsung tertuju dengan file _MainActivity.class_ karena biasanya file ini yang biasa dipanggil pertama. Berikut isi file nya.

```java
package org.chall_reverse_android;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;

public class MainActivity
  extends AppCompatActivity
{
  protected void onCreate(Bundle paramBundle)
  {
    super.onCreate(paramBundle);
    setContentView(2131296283);
    paramBundle = new challenges();
    StringBuilder localStringBuilder = new StringBuilder();
    localStringBuilder.append("Your Flag is ");
    localStringBuilder.append(paramBundle.anu());
    localStringBuilder.append("But the middle Flag is Missing :((");
    Log.d("Flag", localStringBuilder.toString());
    Log.d("Flag", "Help me To Find it :((((");
    Log.d("Secret", "Get it from Reverse to get Lost string");
  }
}
```

Dari file itu aku lihat dia melakukan log untuk flag nya. Dari pengalaman develop android, log ini bisa dibaca dengan dengan perintah `adb logcat`. Langsung aku coba colokkan hpku dengan laptop dan menjalankan perintah itu. Tapi sebelumnya aku tambahkan perintah `grep Flag` untuk memudahkan membaca logcatnya. Berikut hasilnya.

![gambar dari logcat](/assets/images/2018-11-12-235658_1366x768_scrot.png)

Aku merasa aku sudah berhasil disini, karena aku sudah dapat flagnya. Tapi kenapa flag nya aneh yaitu _U0hMe3NpbXBsZV9DVFI0OTYwWVZBUl8qUkVEQUNURUQqX1JFVkVSU0V9_. 

Langsung saja aku coba decode di terminal dengan perintah `echo 'U0hMe3NpbXBsZV9DVFI0OTYwWVZBUl8qUkVEQUNURUQqX1JFVkVSU0V9' | base64 -d` dan hasilnya `SHL{simple_CTR4960YVAR_*REDACTED*_REVERSE}` . Kemudian aku baca lagi baik-baik lognya, ternyata aku masih disuruh untuk menemukan bagian tengah yang hilang.

Tetap semangat, aku coba baca lagi alur kerja aplikasinya dari file *.java* . Sampai akhirnya aku menemukan ada file yang aneh yaitu _secret_flag.class_, karena dia tidak dipanggil di _challenges.class_.

Kemudian aku coba copy isi file _secret_flag.class_, kemudian menambahkan script untuk menampilkan value nya. Dan coba ku jalankan, hasilnya `_asuna_<3_me_` . Waw, sepertinya aku udah berhasil ini. Agar tampilan flag nya lengkap, aku coba koding ulang khusus untuk menampilkan flag builder nya saja dengan mereplace `*REDACTED*` dengan _secret_flag_, dan aku jalankan di compiler online java. Berikut kodenya.

```java
import java.util.Random;

public class HelloWorld {

	// anu kedua
	public static String StringAnu() {
		char[] chars1 = "ABCDEF012GHIJKL345MNOPQR678STUVWXYZ9".toCharArray();
		Random r = new Random();
		StringBuilder sb1 = new StringBuilder();
		char[] str_2 = { 115, 105, 109, 112, 108, 101, 95 };
				
		int i = 0;
		while (i <= 10)
		{
			char c = chars1[r.nextInt(chars1.length)];
			sb1.append(c);
			i += 1;
		}
		StringBuilder localStringBuilder = new StringBuilder();
		localStringBuilder.append(String.valueOf(str_2));
		localStringBuilder.append(sb1.toString());
		localStringBuilder.append("_");
		return localStringBuilder.toString();
	}
				
	public static void main(String []args){
		char[] anu_pertama = { 83, 72, 76, 123 };
		char[] anu_ketiga = { 95, 82, 69, 86, 69, 82, 83, 69, 125 };
		char[] asuna = { 95, 97, 115, 117, 110, 97, 95, 60, 51, 95, 109, 101, 95 };
						
		StringBuilder localStringBuilder = new StringBuilder();
		localStringBuilder.append(String.valueOf(anu_pertama));
		localStringBuilder.append(StringAnu());
		localStringBuilder.append(String.valueOf(asuna));
		localStringBuilder.append(String.valueOf(anu_ketiga));
		System.out.println(localStringBuilder.toString());
	}
}
```
Dan ini hasilnya **SHL{simple_G3480E0UXFU__asuna_<3_me__REVERSE}**. Kemudian aku screenshoot dan kirim ke Dark Assassin.

![gambar dari logcat](/assets/images/2018-11-13-044009_1366x768_scrot.png)

Dan ternyata benar, Terima kasih.

