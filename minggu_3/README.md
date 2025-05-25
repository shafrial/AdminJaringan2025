# Workshop Administrasi Jaringan

**Dosen Pengampu:** Dr. Ferry Astika Saputra ST, M.Sc  
**Nama:** Shafrial Azis  
**NRP:** 3123600024  
**Kelas:** 2 D4 IT A

---

## Minggu 3

### 1. Install NTP

1. **Melakukan penginstalan untuk package NTP**

   ```bash
   apt install ntp
   ```
  ![1](/minggu_3/img/1.jpg)
  Sebelum melakukan konfigurasi, unduh terlebih dahulu package NTP untuk melakukan konfigurasi. Gunakan perintah ‘apt install ntp’. Pastikan anda berada di dalam root saat melakukan instalasi.

2. **Pengecekan server yang digunakan untuk NTP**

   ```bash
   date -R
   ntpq -p
   ```
   ![1](/minggu_3/img/2.jpg)
  Untuk melakukan pengecekan server NTP, gunakan perintah ‘ntpq -p’. Dapat dilihat bahwa NTP masih menggunakan server dari Debian.

3. **Konfigurasi NTP menggunakan server Indonesia**

   Edit file:

   ```bash
   nano /etc/ntpsec/ntp.conf
   ```
   ![1](/minggu_3/img/3.jpg)
Untuk melakukan konfigurasi, buka file /etc/ntpsec/ntp.conf menggunakan editor yang ada. Kemudian, lakukan modifikasi di dalam file tersebut dengan mengganti server NTP debian dengan server NTP Indonesia seperti pada gambar. Untuk menemukan server pool sesuai region, dapat menggunakan website ‘https://www.ntppool.org’.

4. **Mulai ulang sistem NTP**

   ```bash
   systemctl restart ntp
   ntpq -p
   ```
   ![1](/minggu_3/img/4.jpg)
Mulai ulang sistem ntp menggunakan perintah ‘systemctl restart ntp’. Setelah itu, lakukan pengecekan apakah server NTP telah digunakan atau belum. Gunakan perintah ‘ntpq -p’. Dapat dilihat dari capture yang ada, server NTP Host sudah menggunakan Server NTP Indonesia.
5. **Modifikasi Timezone**

   ```bash
   timedatectl set-timezone Asia/Jakarta
   ```
   ![1](/minggu_3/img/5.jpg)
Lakukan pengaturan agar timezone menggunakan Asia/Jakarta untuk mendapatkan Waktu Indonesia Barat. Gunakan perintah ’timedatectl set-timezone Asia/Jakarta’.

6. **Pengecekan Waktu Server**

   ```bash
   date
   ```
   ![1](/minggu_3/img/6.jpg)
   Gunakan perintah ‘date’ untuk melihat waktu sesuai server. Dapat dilihat dari capture bahwa telah berhasil mengubah server menjadi server NTP Indonesia dari server Debian. Terdapat juga keterangan WIB di dalamnya, karena kita melakukan pengaturan Timezone.

---

### 2. Install dan Konfigurasi Samba

#### 2.1 Public Shared Folder

1. **Install samba**
   ```bash
   apt install -y samba
   ```
   ![5](/minggu_3/img/7.jpg)
   Sebelum melakukan konfigurasi, unduh terlebih dahulu package NTP untuk melakukan konfigurasi. Gunakan perintah ‘apt install - y samba’ untuk melakukan pengunduhan secara langsung tanpa konfirmasi. Pastikan anda berada di dalam root saat melakukan instalasi.

2. **Membuat direktori**
   ```bash
   mkdir /home/share
   ls
   ```
   ![5](/minggu_3/img/8.jpg)
   Gunakan perintah ‘mkdir share’ untuk membuat sebuah direktori bernama “share” di dalam direktori /home. Direktori ini digunakan untuk public shared folder.

3. **Pengaturan izin akses**
   ```bash
   chmod 777 /home/share
   ```
   ![5](/minggu_3/img/9.jpg)
   Untuk melakukan pengaturan izin akses terhadap direktori /share. gunakan perintah ‘chmod 777 share’. Di dalam linux, ijin akses direpresentasikan dalam tiga kategori menggunakan angka oktal. Kategori tersebut adalah Owner, Group, dan Other. Setiap kategori memiliki tiga jenis izin akses diantaranya : Read, Write, Execute. Read memiliki nilai 4, Write memiliki nilai 2, Execute memiliki nilai 1. chmod 777 berarti Owner, Group, Other memiliki ijin Read, Write, Others untuk direktori /share. 

4. **Konfigurasi file `smb.conf`**
   ```bash
   nano /etc/samba/smb.conf
   ```
   ![5](/minggu_3/img/10.jpg)
   Tambahkan konfigurasi:
   - Line 25: `unix charset = UTF-8`
   - Line 37: `interfaces = 127.0.0.0/8 192.168.1.0/24 eth0`
   - Line 98: konfirmasi
   - Tambahkan bagian akhir konfigurasi folder publik  
   ![5](/minggu_3/img/11.jpg)  
   ![5](/minggu_3/img/12.jpg)
   ![5](/minggu_3/img/13.jpg)
Setelah memberikan ijin akses pada direktori /share. Lakukan konfigurasi di dalam file etc/samba/smb.conf. Gunakan perintah ‘nano etc/samba/smb.conf’ untuk menggunakan editor nano dalam melakukan konfigurasi file smb.conf. Lakukan konfigurasi untuk line 25, tambahkan ‘unix charset = UTF-8’, line 37 ‘interfaces = 127.0.0.0/8 192.168.1.0/24 eth0’, konfirmasi untuk line 98. Terakhir, tambahkan capture terakhir ke dalam line terakhir di file /etc/samba/smb.conf.

5. **Restart Samba**
   ```bash
   systemctl restart smbd
   ```
   ![5](/minggu_3/img/21.jpg)

   Mulai ulang smbd untuk menjalankan konfigurasi terbaru.

6. **Akses melalui File Manager**
   ![5](/minggu_3/img/14.png)
   Pilih section network di dalam file manager, kemudian klik kanan dan pilih map network drive. Masukkan ‘\\192.168.1.66\share’ untuk mengakses folder share secara publik. 
8. **Koneksi Terhubung**
   
   ![5](/minggu_3/img/15.png)
   ![5](/minggu_3/img/16.png)
   Sharing folder berhasil dilakukan. Lakukan tes dengan membuat file test.txt

10. **Pengecekan file test.txt**
   ![5](/minggu_3/img/17.jpg)
Dapat dilihat dari capture, file test.txt berada di dalam direktori /share yang berarti direktori /share dapat dilakukan sharing secara publik.

#### 2.2 Limited Shared Folder

1. **Membuat direktori**
   ```bash
   mkdir /home/share01
   ```
   ![5](/minggu_3/img/18.jpg)
Gunakan perintah ‘mkdir share01’ untuk membuat sebuah direktori bernama “share01” di dalam direktori /home. Direktori ini digunakan untuk private shared folder.

2. **Pengaturan izin akses**
   ```bash
   chmod 770 /home/share01
   ```
   ![5](/minggu_3/img/19.jpg)
Untuk melakukan pengaturan izin akses terhadap direktori /share. gunakan perintah ‘chmod 770 share01’. Perintah ini memberikan ijin akses penuh terhadap Owner dan Group tanpa memberikan izin pada Other. Ijin akses dapat dilihat menggunakan perintah ‘ls -l’.

3. **Konfigurasi file `smb.conf`**
   ```bash
   nano /etc/samba/smb.conf
   ```
   ![5](/minggu_3/img/20.jpg)
Setelah memberikan ijin akses pada direktori /share01. Lakukan konfigurasi di dalam file etc/samba/smb.conf. Gunakan perintah ‘nano etc/samba/smb.conf’ untuk menggunakan editor nano dalam melakukan konfigurasi file smb.conf. Lakukan konfigurasi untuk line 25, tambahkan ‘unix charset = UTF-8’, line 37 ‘interfaces = 127.0.0.0/8 192.168.53.0/24 eth0’, konfirmasi untuk line 98. Terakhir, tambahkan capture terakhir ke dalam line terakhir di file /etc/samba/smb.conf. 

4. **Restart Samba**
   ```bash
   systemctl restart smbd
   ```
   ![5](/minggu_3/img/21.jpg)
Mulai ulang smbd untuk menjalankan konfigurasi terbaru.

5. **Membuat grup**
   ```bash
   groupadd smbgroup01
   ```
   ![11](/minggu_3/img/22.jpg)
   Untuk membuat sebuah group maka kita gunakan perintah ‘groupadd smbgroup01’. Grup ini digunakan untuk mengelola izin akses bersama pada file atau direktori. Kemudian, jalankan perintah ‘chgrp smbgroup /home/share01’ untuk memberikan akses kepada anggota yang berada di dalam group smbgroup01 bergantung pada izin yang telah ditetapkan(contoh: chmod).

6. **Menambahkan user ke grup**
   ```bash
   adduser debian
   smbpasswd -a debian
   usermod -aG smbgroup01 debian
   ```
   ![23](/minggu_3/img/23.jpg)
   Gunakan perintah ‘adduser debian’ untuk membuat sebuah user bernama debian ke dalam sistem linux. Setelah itu, jalankan perintah perintah ‘smbpasswd -a debian’ untuk menambahkan akun “debian” ke dalam sistem autentikasi samba dan menetapkan password khusus untuk akses samba. Terakhir, jalankan perintah ‘usermod -aG smbgroup01 debian’ untuk menambahkan user “debian” ke dalam group smbgroup01.

7. **Akses via Windows**
   ![12](/minggu_3/img/24.jpg)
   Pilih section network di dalam file manager, kemudian klik kanan dan pilih map network drive. Masukkan ‘\\192.168.92.130\share01’ untuk mengakses folder share secara private.

8. **Masukkan kredensial user**
   ![12](/minggu_3/img/25.jpg)
   Masukkan username dan password untuk mengakses folder.

9. **Koneksi berhasil**
   ![12](/minggu_3/img/26.jpg)
   Berhasil mengakses direktori /share01 menggunakan windows

10. **Akses dari Linux/WSL**
    ```bash
    apt -y install smbclient cifs-utils
    smbclient '\\192.168.92.130\share01' -U debian
    ```
    ![12](/minggu_3/img/27.jpg)
    Sebelum melakukan sharing, lakukan instalasi untuk smbclient jika package belum ada. Jalankan perintah ‘apt -y install smbclient cifs-utils’ untuk melakukan instalasi package. Setelah itu gunakan perintah ‘smbclient  ’\\192.168.92.130\share01’ -U debian’ untuk mengakses shared folder samba menggunakan user “debian”. Kemudian, masukkan password dan berhasil melakukan akses terhadap direktori share01.

12. **Download file**
    ```bash
    mget "test.txt"
    ```
    ![12](/minggu_3/img/28.jpg)
    Untuk mengunduh sebuah file di dalam direktori /share01, gunakan perintah ‘mget “[namafile]“ ’, seperti contoh ‘mget “test.txt” ’. Setelah itu, file akan terunduh dan berada di dalam direktori user. Dapat dilihat dari capture bahwa file “test.txt” telah berada di dalam user. 

---

## 3. Rangkuman tentang Package Management

### 1. Instalasi dan Konfigurasi Sistem
Debian 12 adalah sistem operasi berbasis Linux yang dikenal dengan stabilitas dan keamanan tinggi. Instalasi sistem melibatkan pemartisian disk, pemilihan sistem file, dan konfigurasi awal seperti pengaturan pengguna serta jaringan. Setelah instalasi selesai, sistem perlu dikonfigurasi agar siap digunakan, termasuk pengaturan zona waktu, hostname, dan pembuatan akun pengguna dengan hak akses yang sesuai.

   a. Menampilkan daftar partisi yang tersedia

```bash
fdisk -l
lsblk
```
   b. Memformat partisi dengan sistem file ext4

```bash
mkfs.ext4 /dev/sdX
```
   c. Memasang (mount) partisi yang telah diformat

```bash
mount /dev/sdX /mnt
```
   d. Mengatur zona waktu agar sesuai dengan lokasi pengguna

```bash
timedatectl set-timezone Asia/Jakarta
```
   e. Mengubah hostname sistem
```bash
hostnamectl set-hostname nama_host
```
   f. Membuat akun pengguna baru

```bash
adduser nama_pengguna
```
   g. Menambah pengguna ke grup sudo  agar memiliki akses administratif

```bash
usermod -aG sudo nama_pengguna
```

### 2. Manajemen Paket dan Pembaruan Sistem
Debian menggunakan APT (Advanced Package Tool) untuk menginstal, menghapus, dan memperbarui perangkat lunak. Semua perangkat lunak yang didistribusikan secara resmi tersimpan dalam repositori yang dapat diakses secara online. Memastikan sistem selalu diperbarui sangat penting untuk mendapatkan fitur terbaru dan menjaga keamanan sistem. 

a. Memperbarui daftar paket yang tersedia dalam repositori
```bash
apt update
```

b. Mengupgrade sistem dengan versi terbaru dari paket yang diinstal
```bash
apt upgrade -y
```

c. Menginstal paket baru
```bash
apt install nama_paket
```

d. Menghapus paket yang tidak digunakan lagi
```bash
apt remove nama_paket
```

e. Menghapus dependensi yang tidak diperlukan
```bash
apt autoremove -y
```

f. Mencari informasi tentang paket tertentu dalam repositori

```bash
apt-cache search nama_paket
```

### 3. Manajemen Pengguna dan Hak Akses
Setiap pengguna di sistem Linux memiliki UID (User ID) yang unik. Pengguna dapat dikelompokkan dalam grup untuk memudahkan pengelolaan hak akses. Izin akses file dikendalikan oleh sistem berbasis pemilik, grup, dan lainnya (owner, group, others).

a. Menambahkan pengguna ke grup tertentu
```bash
usermod -aG nama_grup nama_pengguna
```
b. Melihat grup yang diikuti oleh pengguna tertentu
```bash
groups nama_pengguna
```
c. Mengubah kepemilikan file atau direktori
```bash
chown pengguna:grup nama_file
```
d. Mengubah izin akses atau direktori
```bash
chmod 755 nama_file
chmod 644 nama_file
```
e. Mengubah passwd pengguna
```bash
passwd nama_pengguna
```

### 4. Manajemen Proses dan Layanan
Proses dalam sistem Linux adalah aplikasi atau layanan yang sedang berjalan. Debian menggunakan systemd untuk mengelola layanan yang berjalan secara otomatis saat sistem di-boot.

```bash
ps aux
top
kill PID
systemctl start nama_layanan
systemctl stop nama_layanan
systemctl status nama_layanan
systemctl enable nama_layanan
systemctl disable nama_layanan
```

### 5. Konfigurasi Jaringan
Debian mendukung berbagai jenis konfigurasi jaringan, baik dengan DHCP maupun IP statis. Menjaga konektivitas yang stabil dan aman sangat penting, terutama dalam lingkungan server.

a. Melihat konfigurasi jaringan saat ini 
```bash
ip a
```

b. Menampilkan rute jaringan yang digunakan oleh sistem
```bash
ip route
```

c. Menetapkan alamat IP statis untuk interface jaringan tertentu
```bash
ip addr add 192.168.1.100/24 dev eth0
```
d. Menguji koneksi jaringan dengan melakukan ping ke server tertentu
```bash
ping 8.8.8.8
```

### 6. Firewall dan Keamanan
Firewall digunakan untuk mengontrol lalu lintas jaringan, membatasi akses yang tidak sah, dan mencegah serangan dari luar. Debian menyediakan UFW (Uncomplicated Firewall) dan iptables untuk mengelola aturan keamanan

a. Mengaktifkan firewall UFW
```bash
ufw enable
```

b. Mengizinkan akses ke port SSH
```bash
ufw allow 22/tcp
```

c. Menolak akses ke port HTTP
```bash
ufw deny 80/tcp
```

d. Melihat aturan firewall yang aktif

```bash
ufw status verbose
```

### 7. Monitoring Sistem
Memantau sistem adalah tugas penting untuk mengidentifikasi penggunaan sumber daya dan mendeteksi potensi masalah lebih awal.

a. Melihat penggunaan RAM secara real-time
```bash
free -h
```

b. Melihat log terbaru dari sistem
```bash
tail -f /var/log/syslog
```

c. Menampilkan semua layanan yang gagal berjalan

```bash
free -h
```

### 8. Automasi dengan Cron dan Scripting
Automasi tugas administratif membantu mengurangi beban kerja manual dan memastikan sistem berjalan sesuai jadwal yang telah ditentukan.

a. Mengedit daftar cronjob pengguna
```bash
crontab -e
```
b. Menjalankan skrip setiap hari pukul 02.00
```bash
crontab -e
# Contoh: 0 2 * * * /path/to/script.sh
```
c. Memberikan izin eksekusi secara manual
```bash
chmod +x script.sh
```
d. Menjalankan skrip secara manual 

```bash
./script.sh
```

### 9. Troubleshooting dan Recovery
Masalah umum dalam Debian dapat meliputi kegagalan boot, kesalahan layanan, atau kehilangan koneksi jaringan. Memahami cara mengatasi masalah ini sangat penting bagi administrator sistem. 

   a. Memeriksa dan memperbaiki sistem file yang rusak
   ```bash
fsck -y /dev/sdX
```
   b. Memeriksa paket yang rusak dan memperbaikinya
   ```bash
dpkg --configure -a
apt --fix-broken install
```

   c. Mengembalikan file konfigurasi ke versi sebelumnya
   ```bash
mv file.conf.bak file.conf
```



