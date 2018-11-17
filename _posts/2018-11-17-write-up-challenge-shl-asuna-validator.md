---
comments: true
---
Hai, ini write up lagi dari challenge SHL. Challenge ini lanjutan dari yang sebelumnya yaitu Asuna Validator _"asuna telah kembali ingatannya namun dia diculik oleh professor jahat dan ingatannya di kunci oleh administrator, bisakah kamu membantu kirito untuk membuka kunci itu"_. 

Challenge ini seharusnya diselesaikan paling akhir tanggal 15/11/18, tapi aku baru bisa solve hari ini dengan menunjukkan flagnya tadi pagi dan aku disuruh buat WU nya. Baik langsung saja.

Di challenge ini aku diberi file **Asuna Validator.apk**, yaitu aplikasi android. Langsung coba jalankan dan seperti ini tampilannya.

<a href="/assets/images/Screenshot_2018-11-17-10-57-20-422_org.chall.asuna_validator.png" target="_blank">
    <img src="/assets/images/Screenshot_2018-11-17-10-57-20-422_org.chall.asuna_validator.png" style='width:100%;' border="0" alt="tampilan pertama aplikasi">
</a>

Disitu aku diminta memasukkan keygen. Nah, dari mana aku mendapatkan keygennya?, seperti pengalaman challenge sebelumnya aku lakukan reverse ke file tersebut.

Langkah :

Extract file **.apk** dengan perintah `unzip Asuna Validator.apk`.

Reverse file **.dex** nya dari hasil extract tadi dengan perintah `dex2jar classes.dex`.

Buka file **classes-dex2jar.jar** dengan aplikasi **JD-GUI**, untuk melihat sourcenya.

Berikut potongan source code dari **MainActivity.class** nya.

```java
protected void onCreate(final Bundle paramBundle)
{
    super.onCreate(paramBundle);
    setContentView(2131296283);
    paramBundle = (EditText)findViewById(2131165187);
    ((Button)findViewById(2131165309)).setOnClickListener(new View.OnClickListener()
    {
      public void onClick(View paramAnonymousView)
      {
        paramAnonymousView = new aincrad();
        if (new Kunci().validasiString(paramBundle.getText().toString()))
        {
          Object localObject = MainActivity.this.getApplicationContext();
          StringBuilder localStringBuilder = new StringBuilder();
          localStringBuilder.append("Your Flag is : ");
          localStringBuilder.append(paramAnonymousView.flagBuilder());
          localStringBuilder.append("\n But Check Your Logcat!!!");
          localObject = Toast.makeText((Context)localObject, localStringBuilder.toString(), 1);
          localStringBuilder = new StringBuilder();
          localStringBuilder.append("Flag is ");
          localStringBuilder.append(paramAnonymousView.flagBuilder());
          Log.d("Your Flag", localStringBuilder.toString());
          Log.d("Note!!!", "Ingatan asuna ternyata di kunci oleh kode kode rumit kata yui");
          Log.d("Note!!!", "Tolong untuk membuka kembali ingatan Asuna yang dikunci oleh Heathcliff yang jahat");
          Log.d("Xorry", "Find The Key! or Brute it! -Heathcliff-");
          ((Toast)localObject).show();
          return;
        }
        Toast.makeText(MainActivity.this.getApplicationContext(), "Panjang nya Kurang!! / Salah Kunci", 1).show();
      }
});
```

Dari potongan kode diatas, bisa aku baca kalau untuk bisa mendapatkan flag harus lolos dari pengecekan `if (new Kunci().validasiString(paramBundle.getText().toString()))`. Oke, aku coba cari kuncinya.

Kuncinya aku temukan di file **Kunci.class**. berikut potongan kodenya.

```java
public boolean validasiString(String paramString)
{
    char[] arrayOfChar = paramString.toCharArray();
    if (paramString.length() == 7)
    {
      if (arrayOfChar[0] == 'Y')
      {
        if (arrayOfChar[1] == 'U')
        {
          if (arrayOfChar[2] == 'I')
          {
            if (arrayOfChar[3] == 'C')
            {
              if (arrayOfChar[4] == 'H')
              {
                if (arrayOfChar[5] == 'A') {
                  return arrayOfChar[6] == 'N';
                }
                return false;
              }
              return false;
            }
            return false;
          }
          return false;
        }
        return false;
      }
      return false;
    }
    return false;
}
```

Dari kode diatas aku tahu kunci / keygen nya adalah **YUICHAN**. Langsung coba aku masukkan di aplikasi yang sudah aku install tadi dan berhasil, muncul pesan seperti ini.

<a href="/assets/images/Screenshot_2018-11-17-10-57-48-821_org.chall.asuna_validator.png" target="_blank">
    <img src="/assets/images/Screenshot_2018-11-17-10-57-48-821_org.chall.asuna_validator.png" style='width:100%;' border="0" alt="keygen success">
</a>

Aku dapat pesan baru yang menampilkan suatu text yang unik dan menyuruh aku untuk melihat logcat. Oke aku coba lihat dengan perintah `adb logcat`, tapi sebelumnya aku hubungkan terlebih dahulu hp aku dengan laptop. Dan aku dapat log seperti ini.

```
11-17 11:51:38.965  4222  4222 D Your Flag: Flag is _D@wje~x`<z?zeoiSocaambhi~GE@@I^q
11-17 11:51:38.966  4222  4222 D Note!!! : Ingatan asuna ternyata di kunci oleh kode kode rumit kata yui
11-17 11:51:38.966  4222  4222 D Note!!! : Tolong untuk membuka kembali ingatan Asuna yang dikunci oleh Heathcliff yang jahat
11-17 11:51:38.966  4222  4222 D Xorry   : Find The Key! or Brute it! -Heathcliff-
```

Dari log itu ada pesan jika aku diminta untuk membuka text rahasia dengan menemukan kuncinya atau melakukan brute untuk bisa membaca text rahasianya.

Temen-temen ditahap ini aku kesulitan sampai berhari-hari, karena berbagai cara aku coba lakukan seperti finding character _"flag" "key"_, di fitur search **JD-GUI**, melakukan decrypt online dengan berbagai method. Sebenarnya aku agak curiga dengan kata "Xorry" di pesan yang diberikan, karena ada "XOR" disana. Aku coba browsing tentang XOR encryption dan ternyata memang ada. XOR encryption menggunakan key untuk proses encrypt & decrypt nya. Aku coba terus mencari key dan coba menggunakan XOR decryption online masih saja belum berhasil. Aku kira "YUICHAN" itu adalah key nya, ternyata bukan.

Setelah lama di kondisi tersebut, aku coba browsing lagi namun kali ini aku coba cari write up dari CTF yang ada soal tentang XOR dengan keyword _"ctf write up xor"_. Dan aku menemukan ini <https://hgarrereyn.gitbooks.io/th3g3ntl3man-ctf-writeups/2017/PACTF_2017/problems/boole/XOR_1/xor_1.html>.

Dari write up di atas, aku kira mirip dengan masalah yang aku hadapi. Dan aku dapat juga solver code nya. Aku copy dan modifikasi seperti ini.

```python
def sxor(s1,s2):    
    return ''.join(chr(ord(a) ^ ord(b)) for a,b in zip(s1,s2))

s = "_D@wje~ x`<z?zeoiSocaambhi~GE@@I^q"
for x in range(128):
    print sxor(chr(x)*len(s),s)
```

Kemudian aku jalankan, aku mendapat output seperti ini. Ini potongannya.

<a href="/assets/images/2018-11-17-072213_1366x768_scrot.png" target="_blank">
    <img src="/assets/images/2018-11-17-072213_1366x768_scrot.png" style='width:100%;' border="0" alt="ouput solver">
</a>

Dari output diatas, aku lihat ada yang menarik yaitu **SHL{fir,tl0v3vice_commanderKILLER}**. Aku coba konfirmasikan dan ternyata benar itu adalah flag nya.

Pengetahuan baru yang bisa aku dapat dari challenge ini adalah :
1. Program JD-GUI memiliki fitur search.
2. Ilmu tentang XOR Encryption.
3. Untuk menyelesaikan suatu challenge CTF, aku bisa baca - baca WU dari orang lain di internet.

Terima kasih kepada temen - temen SHL, terutama **Dark Assasin** yang telah memberikan challenge.