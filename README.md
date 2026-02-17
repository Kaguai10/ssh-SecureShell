<div align="center">
  <img src="https://readme-typing-svg.herokuapp.com?size=30&color=ff2400&center=true&vCenter=true&width=650&lines=Secure+Shell+(SSH)+Server+di+Debian">
</div>

## Apa Itu SSH?

SSH adalah singkatan dari Secure Shell, yaitu cara untuk mengakses komputer atau server dari jarak jauh lewat jaringan dengan aman. Dengan SSH, kita bisa menjalankan perintah dan mengelola sistem seolah-olah sedang berada langsung di depan komputer tersebut. Keamanannya dijaga dengan sistem enkripsi, sehingga data seperti username dan password tidak mudah disadap oleh orang lain. Karena aman dan praktis, SSH banyak digunakan untuk mengelola server, belajar Linux, dan kebutuhan jaringan maupun keamanan siber.

## Penginstalan dan Penggunaan SSH

Sebelum masuk ke proses instalasi, pastikan server yang akan digunakan sudah terhubung ke jaringan dan bisa mengakses internet. Pada bagian ini, kita akan mulai memasang layanan SSH agar perangkat dapat diakses dari jarak jauh dengan lebih mudah dan aman. Ikuti langkah-langkah berikut secara berurutan supaya proses instalasi berjalan lancar. Untuk penginstallanya sangatlah mudah kalian dapat melakukan Update terlebih dahulu pada server kalian lalu menginstall ssh server.

```bash
apt update
apt install openssh-server -y
```

Setelah melakukan Penginstalan kalian dapat memeriksa status ssh menggunakan perintah berikut ini:

```bash
systemctl status ssh
systemctl enable ssh #Jika statusnya disable
systemctl start ssh #Jika statusnya masih di stop
```

SSH sacara default berjalan di port 22. Jika status SSH sudah aktif dan berjalan maka kita bisa langsung mencoba meremote shell. Kita dapat melakukan Test remote SSH menggunakan perintah ini:

```bash
#ssh user@ip
ssh kaguai@192.168.10.10
```
Setelah penginstalan SSH secara default kita hanya bisa meremote user demi keamanan agar pihak luar tidak dapat meremote root. Namun jika kite memerlukan akses root secara langsung disaat melakukan ssh kita dapat mengubah konfigurasi file sshd_config pada bagian Root login permission . *Note: lebih aman kita melakukan ssh ke User lalu dari terminal kita masuk ke root dengan perintah `sudo su` atau `su`.
```bash
nano /etc/ssh/sshd_config
```
Ubah Value PermitRootLogin menjadi yes Untuk mengizinkan ssh dengan root

```txt
 32 #LoginGraceTime 2m
 33 PermitRootLogin yes
 34 #StrictModes yes
 35 #MaxAuthTries 6
 36 #MaxSessions 10
```

Selain itu kita dapat juga merubah default port pada SSH dari 22 yaitu ke port yang kita inginkan, Untuk mengubahnya pun kita dapat melakukannya dengan mengkonfigurasi file sshd_config

Ubah Value Port dari 22 menjadi nomor port yang kalian inginkan

```txt
 12 Include /etc/ssh/sshd_config.d/*.conf
 13
 14 Port 1010
```

Jika Nomor port ssh diubah maka dalam melakukan koneksi atau remote shell ada tambahan parameter yaitu `-p` sehingga seperti ini
```bash
ssh root@192.168.10.10 -p 1010
```

## SSH Keygen
ssh-keygen adalah alat baris perintah (command-line tool) standar pada sistem Unix, Linux, macOS, dan Windows yang digunakan untuk membuat, mengelola, dan mengonversi pasangan kunci kriptografi (public dan private key) untuk autentikasi SSH. Untuk Membuat ssh-keygen kalian dapat mengikuti tutorial ini:

Untuk membuat SSH Keygen kalian melakukannya di pihak client. Jadi SSH-Keygen ini digunakan agar Server bisa langsung mengenali client-nya menggunakan key. Untuk membuatnya cukup jalankan perintah `ssh-keygen` Untuk membuat Kunci SSH yang baru.

```bash
ssh-keygen
```
Secara default SSH Keygen akan membuat key menggunakan algoritma RSA, Namun kalian dapat menggunakan algoritma lain dengan menambahkan parameter `-t` ssh-keygen memiliki Algoritma seperti ed25519, rsa, ecdsa, ataupun dsa. Berikut perbandingan Algoritma SSH Keygen
| Algoritma | Dasar Kriptografi (Cara Kerja) | Ukuran Key | Keunggulan | Kekurangan | Dampak Praktis (di dunia nyata) |
| --------- | ------------------------------ | ------------ | ------------ | ----------- | --------------- |
| **RSA** | Berdasarkan **faktorisasi bilangan prima besar**. Keamanan = susah memfaktorkan n = p × q | 2048–4096 bit | - Sangat kompatibel (didukung hampir semua OS & server lama) <br> - Konsep mudah dipahami (matematika dasar) | - Perlu key besar agar aman <br> - Proses tanda tangan & verifikasi lebih lambat <br> - File key besar | - Login SSH sedikit lebih lambat <br> - Cocok untuk sistem lama <br> - Beban CPU lebih tinggi dibanding ECC |
| **DSA** | Berdasarkan **Discrete Logarithm Problem** di bilangan modulo | Tetap 1024 bit | - Dulu standar pemerintah (lama) <br> - Struktur mirip RSA (mudah dipelajari sejarahnya) | - Ukuran key kecil (lemah) <br> - Rentan terhadap serangan modern <br> - Banyak server menolak DSA | - Risiko key bisa dipalsukan <br> - Tidak cocok untuk sistem modern |
| **ECDSA** | Berdasarkan **Elliptic Curve Discrete Logarithm Problem (ECDLP)**                         | 256 / 384 / 521 bit | - Lebih kecil dari RSA tapi setara kuatnya <br> - Lebih cepat dari RSA <br> - Hemat bandwidth | - Implementasi lebih kompleks <br> - Keamanan sangat tergantung kualitas random number <br> - Kurang stabil di sistem lama | - Login lebih cepat dari RSA <br> - Lebih efisien di server & IoT |
| **ed25519** | Varian khusus ECC (Curve25519) dengan desain modern & aman secara default | Tetap (≈256 bit) | - Aman tanpa perlu pilih ukuran bit <br> - Sangat cepat <br> - Tahan kesalahan implementasi <br> - Key kecil | - Tidak didukung sistem SSH sangat tua | - Login SSH paling cepat <br> - File key kecil <br> - Minim risiko salah konfigurasi |

Contoh penggunaan alogaritma menggunakan ed25519
```bash
ssh-keygen -t ed25519
```
Dalam pembuatan ssh-keygen kalian akan diminta tempat menyimpan key ssh `Enter file in which to save the key (/home/kaguai/.ssh/id_ed25519):` Jika kalian ingin biarkan default maka file key akan disimpan di direktori `.ssh` Jika ingin mengubahnya maka kalian dapat merubahnya dengan menuliskan path terbaru.

Selain meminta tempat penyimpanan file key ssh-keygen juga akan meminta password `Enter passphrase for "/home/kaguai/.ssh/id_ed25519" (empty for no passphrase):` Jika kalian tidak ingin menggunakan password maka kalian bisa langsung tekan enter 2 kali. Jika kalian ingin menggunakan password bisa langsung ketikan password-nya lalu nanti akan ada konfirmasi password `Enter same passphrase again: ` maka tuliskan kembali password dengan sama. Password ini akan digunakan untuk memverivikasi saat melakukan SSH.

Setelah selesai membuat SSH Keygen kalian dapat mengirimnya ke server menggunakaan `ssh-copy-id`.

```bash
ssh-copy-id kaguai@192.168.10.10 #sesuaikan dengan nama user server kalian
```

Setelah mengcopy kunci ssh selanjutnya melakukan SSH ke server
```bash 
ssh kaguai@192.168.10.10
```
Jika kalian dalam pembuatan SSH Keygen tidak membuat password untuk file key-nya maka kalian akan langsung login ke terminalnya. *Tetapi* Jika kalian menggunakan password maka saat login kalian akan diminta untuk memverivikasi File Key kalian dengan meminta password `Enter passphrase for key '/home/kaguai/.ssh/id_ed25519': ` Disini kalian cukup masukan password file Key yang kalian buat tadi diawal.

Untuk mengetahui lebih tentang SSH kalian dapat menggunakan manual-nya ssh menggunakan perintah ini
```bash
man ssh
```



