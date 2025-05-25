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
   ![5](/minggu_3/img/14.jpg)
7. **Koneksi Terhubung**
   
   ![8](8.jpg)

8. **Pengecekan file test.txt**
   ![9](9.jpg)

#### 2.2 Limited Shared Folder

1. **Membuat direktori**
   ```bash
   mkdir /home/share01
   ```

2. **Pengaturan izin akses**
   ```bash
   chmod 770 /home/share01
   ```

3. **Konfigurasi file `smb.conf`**
   ```bash
   nano /etc/samba/smb.conf
   ```
   Tambahkan konfigurasi:
   - Line 25: `unix charset = UTF-8`
   - Line 37: `interfaces = 127.0.0.0/8 192.168.53.0/24 eth0`
   - Tambahkan bagian akhir konfigurasi folder private  
   ![10](10.jpg)

4. **Restart Samba**
   ```bash
   systemctl restart smbd
   ```

5. **Membuat grup**
   ```bash
   groupadd smbgroup01
   chgrp smbgroup01 /home/share01
   ```
   ![11](11.jpg)

6. **Menambahkan user ke grup**
   ```bash
   adduser debian
   smbpasswd -a debian
   usermod -aG smbgroup01 debian
   ```

7. **Akses via Windows**
   ```
   \\192.168.92.130\share01
   ```
   ![12](12.jpg)

8. **Masukkan kredensial user**
   ![13](13.jpg)

9. **Koneksi berhasil**
   ![14](14.jpg)

10. **Akses dari Linux/WSL**
    ```bash
    apt -y install smbclient cifs-utils
    smbclient '\\192.168.92.130\share01' -U debian
    ```

11. **Download file**
    ```bash
    mget "test.txt"
    ```
    ![15](15.jpg)

---

## 3. Rangkuman tentang Package Management

### 1. Instalasi dan Konfigurasi Sistem

```bash
fdisk -l
lsblk
mkfs.ext4 /dev/sdX
mount /dev/sdX /mnt
timedatectl set-timezone Asia/Jakarta
hostnamectl set-hostname nama_host
adduser nama_pengguna
usermod -aG sudo nama_pengguna
```

### 2. Manajemen Paket dan Pembaruan Sistem

```bash
apt update
apt upgrade -y
apt install nama_paket
apt remove nama_paket
apt autoremove -y
apt-cache search nama_paket
```

### 3. Manajemen Pengguna dan Hak Akses

```bash
usermod -aG nama_grup nama_pengguna
groups nama_pengguna
chown pengguna:grup nama_file
chmod 755 nama_file
chmod 644 nama_file
passwd nama_pengguna
```

### 4. Manajemen Proses dan Layanan

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

```bash
ip a
ip route
ip addr add 192.168.1.100/24 dev eth0
ping 8.8.8.8
```

### 6. Firewall dan Keamanan

```bash
ufw enable
ufw allow 22/tcp
ufw deny 80/tcp
ufw status verbose
```

### 7. Monitoring Sistem

```bash
free -h
tail -f /var/log/syslog
systemctl --failed
```

### 8. Automasi dengan Cron dan Scripting

```bash
crontab -e
# Contoh: 0 2 * * * /path/to/script.sh
chmod +x script.sh
./script.sh
```

### 9. Troubleshooting dan Recovery

```bash
fsck -y /dev/sdX
dpkg --configure -a
apt --fix-broken install
mv file.conf.bak file.conf
```
