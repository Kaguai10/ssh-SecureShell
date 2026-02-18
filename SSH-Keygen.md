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

[Baca Kembali Tentang SSH-Server](https://github.com/Kaguai10/ssh-server)



