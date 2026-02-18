## SSH Port Forwarding

SSH Port Forwading atau SSH Tunneling adalah sebuah teknik atau cara untuk melakukan tunneling protocol. Tunneling adalah metode dalam jaringan komputer yang membungkus (enkapsulasi) paket data di dalam paket lain untuk dikirim melalui jaringan publik dengan aman. Sehingga SSH Port Forwarding memungkinkan data dapat di transfer dari satu jaringan ke jaringan lain menggunakan SSH.

Sebelum melakukan Forwarding ada beberapa hal yang Harus di konfigurasi di file sshd_config yaitu membuka komentar dengan menghapus pagar pada AllowAgentForwarding, AllowTcpForwarding, dan GatewayPorts. Setelah itu ubah value GatewayPorts dari no ke yes

```bash
nano /etc/ssh/sshd_config
```

```txt
 89 AllowAgentForwarding yes
 90 AllowTcpForwarding yes
 91 GatewayPorts yes
```
Ada 3 Tipe SSH Port Forwading yaitu:

1. Local Port Forwading `-L`
Meneruskan koneksi dari user ke server SSH dan kemudian ke port host tujuan. Jadi Seperti ini Contoh: Di-server memiliki port yang berjalan misal port 8000 namun tidak dapat diakses secara langsung alias lokal. Tetapi kita dapat mengunjunginya dengan menggunakan SSH Local Port Forwading. Dengan membuaut port lokal misal port 1010 untuk meneruskannya ke port host tujuan. Untuk menjalankan SSH Local Port Forwading kalian dapat menggunakan parameter -L, Beikut Command Lengkapnya:

```bash
#ssh -L <Port-Local>:localhost:<Port-Tujuan> user@<ip-Server>
ssh -L 1010:localhost:8000 root@11.10.10.11
```

Sehingga Kalian akan membuka port 1010 untuk meneruskannya ke port 8000 yang berjalan pada server dengan IP 11.10.10.11. Dan kalian dapat membuka web tersebut secara lokal `http://localhost:1010`

2. Remote Port Forwading `-R`
Meneruskan port dari server ke user dan kemudian ke port host tujuan. Remote Port Forwarding memungkinkan sebuah komputer atau PC dapat diakses melalui internet. Jadi seperti ini Contoh: pada laptop kalian atau PC Lokal kalian mempunyai web yang berjalan pada port 2121. Lalu kalian ingin semua orang ingin melihatnya secara publik nah kebetulan anda mempunyai server yang berjalan di IP Public yaitu 11.10.10.11. Karena hanya ingin diperlihatkan sebenertar kita bisa menggunakan SSH Remote Port Forwarding dengan membuka port di misal port 1234 di server. Dibanding memindahkan seluruh file skrip website lalu di up di server, lebih baik kita menggunakan SSH Remote Port Forwarding.

Untuk Menggunakan SSH Remote Port Forwarding kalian bisa menggunakan parameter `-R` atau kalian bisa meniru perintah dibawah ini:

```bash
#ssh -R 0.0.0.0:<Remote-Port>:localhost:<Port-Host> user@<ip-Server>
ssh -R 0.0.0.0:1234:localhost:2121 root@11.10.10.11
```

Sehingga kalian bisa membuka di browser kalian dan menuliskan IP Server beserta port nya. Port yang digunakan yaitu Remote Port yang kita buka saat melakukan SSH Remote Port Forwarding, jadi seperti ini http://11.10.10.11:1234 Maka saat kalian membuka di browser hasilnya adalah port web lokal tadi diteruskan ke port server karena hasil SSH Remote Port Forwarding

3. Dynamic Port Forwarding `-D`
SSH Dynamic Port Forwarding yaitu SSH client yang akan membuat SOCKS proxy yang nantinya akan dikonfigurasikan ke sistem operasi atau web browser. SOCKS (Socket Secure) Proxy adalah  server perantara yang mengarahkan lalu lintas internet antara perangkat Anda dan tujuan akhirnya. Sehingga Dynamic Port Forwarding memiliki konsep yang sama dengan proxy atau VPN.

Untuk menjalankan SSH Dynamic Port Forwarding kalian dapat menggunakan tambahan Parameter `-D`. Berikut Contoh Lengkap Penggunaannya

```bash
#ssh -D <Port-Local> user@<ip-Server>
ssh -D 1080 root@11.10.10.11
```

Setelah melakukan SSH Dynamic Port Forwarding, koneksi dapat digunakan mirip seperti menggunakan VPN.Contohnya untuk browsing di browser, pengguna dapat mengatur pengaturan Network pada browser yang digunakan. Pada menu Settings pilih Network lalu pilih Manual Proxy. Setelah itu, pada bagian SOCKS, masukkan Host dengan IP lokal atau 127.0.0.1 dan Port 1080 sesuai dengan port yang digunakan saat melakukan SSH Dynamic Port Forwarding. Selanjutnya pilih tipe SOCKS v5. Jika tersedia opsi “Proxy DNS when using SOCKS v5”, maka opsi tersebut perlu dicentang. Setelah pengaturan disimpan, aktivitas browsing akan menggunakan IP dari server SSH. Konsep ini mirip dengan VPN, yaitu menggunakan server perantara untuk menyembunyikan IP asli pengguna.

Catatan: Dynamic Port Forwarding tidak membuat seluruh sistem menggunakan proxy, melainkan hanya aplikasi yang dikonfigurasi untuk menggunakan proxy tersebut.

Selain sebagai alternatif penggunaan VPN yang ringan, SSH Dynamic Port Forwarding juga digunakan untuk melakukan pivoting, yaitu teknik memanfaatkan sebuah device atau server sebagai perantara (jembatan) untuk mengakses jaringan atau layanan yang tidak dapat diakses secara langsung. Contohnya, ketika pengguna tidak dapat mengakses website C secara langsung, tetapi memiliki akses SSH ke server B yang dapat mengakses website tersebut, maka pengguna dapat melakukan SSH Dynamic Port Forwarding ke server B. Dengan demikian, koneksi dari device pengguna akan diteruskan melalui server B sehingga website C dapat diakses melalui jaringan server B. SSH Dynamic Port Forwarding memiliki berbagai kegunaan lainnya, terutama dalam kegiatan administrasi jaringan, pengujian keamanan, dan eksplorasi jaringan internal.
