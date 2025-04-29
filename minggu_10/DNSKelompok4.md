# DNS Server 

Mata Kuliah : Workshop Administrasi Jaringan

Dosen Pengampu : Dr. Ferry Astika Saputra ST, M.Sc

# Kelompok 4

Anggota :

1. M. Rafi Rizaldi (3123600001)

2. M. Rizaldy Ramadhan (3123600011)

3. Shafrial Azis (3123600024)

# Topologi

![image](https://github.com/user-attachments/assets/2f36d468-5c75-4052-bb5a-5330d7ab00e2)

# Konfigurasi DNS
### 1. Mengunduh bind9 dan bind9utils 

```bash
apt -y install bind9 bind9utils;
```
### 2. Melakukan konfigurasi untuk named.conf 

```bash
nano /etc/bind/named.conf;
```
![image](https://github.com/user-attachments/assets/5b62288c-78c4-47cc-a049-5eed73e6d70e)

### 3. Melakukan konfigurasi untuk named.conf.options 

```bash
nano /etc/bind/named.conf.options;
```
![image](https://github.com/user-attachments/assets/5b62288c-78c4-47cc-a049-5eed73e6d70e)

### 3. Melakukan konfigurasi untuk named.conf.internal-zones 

```bash
nano /etc/bind/named.conf.internal-zones;
```
![image](https://github.com/user-attachments/assets/5b62288c-78c4-47cc-a049-5eed73e6d70e)
### 4. Melakukan konfigurasi untuk kelompok4.home 

```bash
nano /var/cache/bind/kelompok4.home;
```
![image](https://github.com/user-attachments/assets/5b62288c-78c4-47cc-a049-5eed73e6d70e)
### 5. Melakukan konfigurasi untuk 4.168.192.db 

```bash
nano /var/cache/bind/4.168.192.db;
```
![image](https://github.com/user-attachments/assets/5b62288c-78c4-47cc-a049-5eed73e6d70e)

### 6. Testing koneksi menggunakan named-checkzone 

```bash
named-checkzone kelompok4.home /var/cache/bind/kelompok4.home;
```
```bash
named-checkzone 4.168.192.in-addr-arpa /var/cache/bind/4.168.192;
```
![image](https://github.com/user-attachments/assets/5b62288c-78c4-47cc-a049-5eed73e6d70e)

### 7. Tes menggunakan dig 

```bash
dig kelompok4.home;
```
```bash
dig -x 192.168.4.10;
```
![image](https://github.com/user-attachments/assets/5b62288c-78c4-47cc-a049-5eed73e6d70e)

# Konfigurasi Web Server

### 1. Menginstal web server 

```bash
apt -y install apache2
```
### 2.Melakukan konfigurasi untuk tampilan web server 
```bash
nano /var/www/html/index.html
```
### 3.Restart web server

```bash
systemctl restart apache2
```
### 4.Akses web menggunakan browser
 - Lokal: 192.168.4.10

![image](https://github.com/user-attachments/assets/0fbeb090-397f-4c17-9b28-74ec9e16f004)

 - Klien lain: 192.168.2.10

![image](https://github.com/user-attachments/assets/615487c2-5cb6-40c3-808b-3616e847c557)
