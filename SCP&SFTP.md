# **SCP & SFTP**
SCP (Secure Copy Protocol) adalah tools atau utilitas baris perintah (command-line) yang digunakan untuk mentransfer file dengan aman antara komputer lokal dan server, atau antar dua server jarak jauh melalui jaringan, dengan memanfaatkan protokol SSH. SFTP (Secure File Transfer Protocol) adalah protokol jaringan aman yang digunakan untuk mentransfer, mengakses, dan mengelola berkas melalui Secure Shell (SSH) dengan menggunakan enkripsi.

Dalam penginstallan SSH Kalian akan mendapatkan banyak depedency contohnya adalah SCP dan SFTP. Dua fitur ini yang kita dapatkan dalam dependency SSH berguna untuk mengirim (metransfer) file dan berkas antar device bisa untuk client ke server atau server ke client atau bahkan server ke server. Walau sama sama digunakan untuk metransfer file namun 2 protokol ini memiliki keunggulan dan kekurangannya masing masing. Berikut Tabel Perbedaan antara SCP dan SFTP

| Aspek | SCP (Secure Copy Protocol)| SFTP (SSH File Transfer Protocol) |
| ------| ------------------------- | --------------------------------- |
| Fungsi utama | SCP digunakan khusus untuk menyalin file dari satu komputer ke komputer lain melalui koneksi SSH. Protokol ini hanya berfokus pada proses pemindahan file tanpa menyediakan fitur untuk mengelola file di dalam server. | SFTP digunakan untuk mentransfer sekaligus mengelola file di server melalui koneksi SSH. Selain mengunduh dan mengunggah file, pengguna juga dapat melakukan operasi manajemen file seperti melihat isi folder, menghapus, atau mengganti nama file. |
| Cara penggunaan | SCP bekerja dengan satu perintah langsung di terminal untuk mengirim atau mengambil file, sehingga prosesnya bersifat sekali jalan dan tidak menyediakan mode interaktif. Setelah perintah dijalankan, proses transfer langsung berlangsung tanpa bisa menelusuri direktori di server. | SFTP bekerja dalam mode interaktif seperti FTP, di mana pengguna masuk ke dalam sesi SFTP terlebih dahulu, kemudian dapat menjalankan berbagai perintah untuk berpindah folder, melihat isi direktori, dan mengatur file secara langsung di server.  |
| Kemampuan melihat direktori  | SCP tidak menyediakan fasilitas untuk melihat isi direktori di server sebelum melakukan penyalinan file, sehingga pengguna harus sudah mengetahui lokasi file yang akan dikirim atau diambil. | SFTP memungkinkan pengguna untuk melihat isi direktori server menggunakan perintah seperti `ls`, sehingga pengguna dapat menelusuri struktur folder sebelum melakukan transfer file. |
| Pengelolaan file | SCP hanya berfungsi untuk menyalin file dan folder, tanpa mendukung operasi lain seperti menghapus atau mengganti nama file di server. Oleh karena itu, SCP tidak cocok digunakan untuk manajemen file jarak jauh. | SFTP mendukung berbagai operasi pengelolaan file seperti menghapus file, mengganti nama file, membuat folder baru, serta memindahkan file di dalam server, sehingga lebih fleksibel untuk administrasi file. |
| Kelanjutan transfer (resume) | SCP tidak mendukung fitur melanjutkan transfer file yang terputus di tengah jalan, sehingga jika koneksi terputus, proses transfer harus diulang dari awal. | SFTP mendukung fitur melanjutkan transfer file yang terputus, sehingga lebih efisien ketika mentransfer file berukuran besar atau pada jaringan yang tidak Stabil|
| Kecepatan transfer | SCP umumnya lebih cepat untuk proses penyalinan file sederhana karena mekanismenya lebih langsung dan tidak memerlukan sesi interaktif. | SFTP relatif lebih lambat dibanding SCP untuk transfer sederhana karena menyediakan banyak fitur tambahan seperti manajemen file dan komunikasi interaktif. |
| Keunggulan utama | Keunggulan SCP terletak pada kesederhanaannya, yaitu cukup dengan satu perintah untuk menyalin file secara aman melalui SSH tanpa konfigurasi tambahan. | Keunggulan SFTP terletak pada kemampuannya untuk mengelola file di server secara lengkap dan aman karena semua komunikasi dilakukan melalui SSH.                                                                                                     |
| Kekurangan utama | Kekurangan SCP adalah tidak adanya fitur manajemen file dan tidak mendukung kelanjutan transfer jika koneksi terputus, sehingga kurang fleksibel untuk penggunaan jangka panjang. | Kekurangan SFTP adalah prosesnya yang relatif lebih lambat dan penggunaan perintah yang lebih banyak dibanding SCP, sehingga kurang praktis untuk penyalinan file cepat. |
| Waktu yang cocok digunakan   | SCP paling cocok digunakan ketika pengguna hanya perlu menyalin file secara cepat dari atau ke server tanpa perlu mengelola isi direktori di dalam server. | SFTP paling cocok digunakan ketika pengguna perlu mengelola file di server, seperti mengunggah, mengunduh, menghapus, dan menata file secara terstruktur. |

## **SCP (Secure Copy Protocol)**
Kita dapat menggunakan SCP menggunakan perintah `scp` berikut tutorial menggunakan SCP:

> **UPLOAD FILE**
1. client ke server
```bash
#scp <name_file>  <user>@<ip-server>:<path_tujuan>
scp file.txt kaguai@11.10.10.11:/home/kaguai/
```
Jadi kita akan menggunakan perintah diatas untuk mengupload file yang ada di komputer lokal kita ke Server. Pada contoh diatas kita mengupload file bernama 'file.txt' ke server 11.10.10.11 menggunakan user kaguai. Lalu file txt itu disimpan di server pada direktori `/home/kaguai/`.

2. Server ke Server (Remote)
```bash
#scp <user1>@<ip-server1>:<file_denganPathLengkap> <user2>@<ip-server2>:<path_tujuan>
scp kaguai@11.10.10.11:/home/kaguai/file.txt admin@11.10.10.13:/home/admin/ 
```
Jadi perintah diatas akan kita gunakan ketika kita ingin menyalin file yang ada pada suatu server ke server lain-nya dengan cara kita remote secara langsung. Pada contoh diatas saya mengakses server '11.10.10.11' dengan user kaguai untuk menyalin file yang ada didalamnya yaitu file.txt yang terletak di `/home/kaguai` ke server '11.10.10.13' menggunakan user admin dan menaruh filenya di direktori `/home/admin`.

> **Upload Folder**
1. client ke server
```bash
#scp -r <nama_folder> <user>@<ip-server>:<path_tujuan>
scp -r project kaguai@11.10.10.11:/home/kaguai/
```

Sama seperti mengupload file namun pada Upload folder kita menambahkan parameter `-r`, dengan menggunakan parameter '-r' menandakan bahwa kita akan menyalin berupa folder. Pada contoh diatas saya mengupload folder dengan nama foldernya yaitu project ke server '11.10.10.11' dengan user kaguai dan disimpan pada direktori '/home/kaguai/'.

2. Server ke server (Remote)
```bash
#scp -r <user1>@<ip-server1>:<NameFolder_denganPathLengkap> <user2>@<ip-server2>:<path_tujuan>
scp -r kaguai@11.10.10.11:/home/kaguai/project/ root@11.10.10.13:/var/www/
```

Kurang lebih sama juga seperti Upload file server ke server kita menggunakan remote. dan yaps untuk mengupload folder kita juga harus menambahkan parameter `-r` untuk menandakan bahwa kita akan menyalin folder. Pada contoh diatas saya menyalin folder project yang terletak pada '/home/kaguai' pada server 11.10.10.11 dengan user kaguai lalu saya upload ke server '11.10.10.13' dengan user root dan disimpan di direktori '/var/www'.

> **Mendownload file dan folder dari Server**
1. Mendownload File
```bash
#scp <user>@<ip-server>:<NameFile_denganPathLengkap> <path_tujuan>
scp kaguai@11.10.10.11:/home/kaguai/file.txt Document/sharing/
```

Untuk mendownload file kita hanya merubah urutan kita mengakses server terlebih dahulu lalu pilih local direktori tujuan untuk menyimpan file. Pada contoh diatas saya akan mendownload file dari server '11.10.10.11' dengan user kaguai untuk mengambil file yang ada pada direktori server yaitu /home/kaguai dan nama filenya adalah 'file.txt' lalu saya akan simpan di komputer saya pada direktori 'Document/sharing'.

2. Mendownload Folder
```bash
#scp -r <user>@<ip-server>:<NameFolder_denganPathLengkap> <path_tujuan>
scp -r kaguai@11.10.10.11:/home/kaguai/project/ /var/www/
```

Mendownload Folder dengan SCP sama seperti menyalin folder namun hanya urutannya kita tukar yaitu remote server dulu baru path local directori. Pada contoh diatas saya akan mendownload folder dari server '11.10.10.11' dengan user kaguai untuk mengambil folder yang ada pada direktori server yaitu /home/kaguai dan nama foldernya adalah 'project' lalu saya akan simpan di komputer saya pada direktori '/var/www/'.


## **SFTP (Secure File Transfer Protocol)**
Kita dapat menggunakan SFTP menggunakan perintah `sftp`. Berbeda dengan SCP, SFTP lebih interaktif karena kita akan metransfer file seperti menggunakan terminal. Untuk memulai transfer file menggunakan SFTP kalian dapat menggunakan perintah ini:

```bash
#sftp <user>@<ip-server>
sftp kaguai@11.10.10.11
```

Pada Contoh diatas saya memulai melakukan transfer file menggunakan SFTP ke server '11.10.10.11' dengan user 'kaguai'. Saat kalian menggunakan sftp maka prompt akan berubah menjadi `sftp>`. Nah untuk menggunakannya kalian bisa mengetik `ls` untuk melihat isi direktori atau ingin melihat kita sedang berada di direktori mana kalian dapat menggunakan perintah `pwd` dan `cd` untuk pindah direktori. Setelah ini saya akan memberikan tutorial menggunakan sftp, berikut Tutorial menggunakan sftp:

>**Download** <br>

Untuk Mendownload dengan sftp kalian bisa gunakan perintah `get`
1. Download File
```bash
#cd <nama_dir>
cd Documents/
#get <name_file> <LocalPath_tujuan>
get data.txt /home/local/Document/
```

Pada contoh diatas saya pertama melakukan *change directory* ke direktori Documents karena file tujuan yang ingin di salin berada di direktori Documents. lalu saya gunakan perintah get untuk mendownload file data.txt dan saya akan menaruh file nya di komputer lokal saya pada direktori '/home/local/Document' kalian juga dapat menyalin file secara langsung tanpa pindah direktori tapi kalian wajib mengarahkan sesuai path direktori file berada. Seperti: `get Documents/data.txt /home/local/Documents/`

2. Download Folder
```bash
#get -r <nama_direktori> <LocalPath_directori>
get -r project/ /home/local/
```

sama seperti mendownload file namun untuk mendownload folder kita tambahkan parameter `-r`. pada contoh diatas saya mendownload folder project lalu saya simpan di komputer lokal saya pada direktori '/home/local/'.  Jika misal folder yang ingin didownload tidak terletak di direktori yang bekerja sekarang kalian dapat menggunakan perintah `cd` untuk pindah direktori atau mengarahkan secara langsung dengan menulis path direktori secara lengkap. Contoh: `get -r /home/kaguai/project/ /home/local/`.

>**Upload**<br>

Untuk mengupload dengan sftp kalian bisa menggunakan perintah `put`.

1. Mengupload File
```bash
#put <NamaFileLocal_denganPathLengkap> <path_tujuan>
put /home/local/Documents/data.txt /home/kaguai/Documents
```

Pada contoh diatas saya mengupload file yang ada pada komputer lokal saya yaitu file.txt yang terletak di direktori '/home/local/Documents', lalu saya mengirimnya untuk disimpan di server pada direktori '/home/kaguai/Documents/'.

2. Mengupload Folder
```bash
#put -r <NamaFolderLocal_denganPathLengkap> <path_tujuan>
put -r /home/local/Documents/project /home/kaguai/
```

Untuk mengupload folder kita cukup tambahkan parameter `-r`. Pada contoh diatas saya mengupload folder yang ada pada komputer lokal saya yaitu 'project' yang terletak di direktori '/home/local/Documents', lalu saya mengirimnya untuk disimpan di server pada direktori '/home/kaguai/.

>**Mengelola File dan Folder**<br>

Dalam pengunaan SFTP kita dapat melakukan pengelolaan. berikut cara perintah yang digunakan untuk mengelola File dan Folder dengan SFTP:
1. Hapus file => Menghapus file kalian dapat menggunakan perintah `rm <NamaFile>`
2. Membuat Folder => Membuat Folder di SFTP kalian dapat menggunakan perintah ini `mkdir <NamaFolderBaru>`
3. Menghapus Folder => Untuk menghapus folder kalian dapat menggunakan perintah `rmdir <NamaFolder>`
4. Merubah Nama File => Untuk merubah Nama file Kalian dapat menggunakan perintah ini `rename <NamaFile> <NamaFileBaru>`

Dan saya juga punya tips untuk SFTP yaitu kalian dapat menggunakan perintah `lls` untuk melihat isi direktori lokal, `lcd` untuk berpindah direktori pada komputer lokal kalian, dan `lpwd` untuk melihat tempat direktori kalian bekerja saat ini di lokal. *Note: dengan menambahkan 'l' dalam setiap perintah berarti kalian menjalankannya untuk lokal.

[Baca Kembali Tentang SSH-Server](https://github.com/Kaguai10/ssh-server)
