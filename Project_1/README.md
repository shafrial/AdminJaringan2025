# Workshop Administrasi Jaringan

**Tanggal:** 25 Mei 2025  
**Dosen Pengampu:** Dr. Ferry Astika Saputra ST, M.Sc  
**Nama:** Shafrial Azis  
**NRP:** 3123600024  
**Kelas:** 2 D4 IT A

---

## Minggu 13

### Instalasi Docker Desktop

![Instalasi Docker Desktop](/Project_1/img/1.png)

---

### Membuat Sebuah Container di dalam Docker

```bash
docker run --name mysql-axon \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=classicmodels \
  -p 3306:3306 \
  -d mysql:latest
```

![Run Container](/Project_1/img/2.png)

Penjelasan:
- `docker run` : membuat dan menjalankan container baru
- `--name mysql-axon` : memberi nama container
- `-e MYSQL_ROOT_PASSWORD=root` : set password root
- `-e MYSQL_DATABASE=classicmodels` : buat database saat container dijalankan
- `-p 3306:3306` : buka port 3306 dari container ke host
- `-d` : jalankan di background
- `mysql:latest` : gunakan image MySQL terbaru

---

### Mengecek Container yang Sedang Berjalan

```bash
docker ps
```

![Cek Container](/Project_1/img/3.png)
Perintah ini merupakan singkatan dari "process status", seperti di Linux, untuk menampilkan proses yang aktif, tapi dalam konteks ini: container yang aktif (running).

---

### Copy File SQL ke Dalam Container

```bash
docker cp "Axon sales - Mysql Database.sql" mysql-axon:/Axon.sql
```

![Copy SQL](/Project_1/img/4.png)
Penjelasan:
- `cp` : Singkatan dari copy. Fungsinya untuk menyalin file atau folder antara host (komputer) dan container Docker.
- `Axon sales - Mysql Database.sql` : Nama file di komputer lokal yang ingin disalin. Karena nama file mengandung spasi, maka dibungkus dengan tanda kutip "...". 
- `mysql-axon` : Nama container tujuan yang sudah dibuat dan jalankan (harus aktif atau minimal ada).
- `:/Axon.sql ` : Tujuan file di dalam container. Artinya file akan disalin ke root directory (/) di dalam container, dan dinamai ulang menjadi Axon.sql.
---

### Masuk ke Dalam Container

```bash
docker exec -it mysql-axon bash
```

![Masuk Container](/Project_1/img/5.png)
Penjelasan:
Digunakan untuk masuk ke dalam container Docker bernama mysql-axon dan menjalankan shell bash di dalamnya
- `exec` : Singkatan dari "execute", digunakan untuk menjalankan perintah di dalam container yang sedang berjalan.
- `-it` : Kombinasi dua opsi penting:
  i.	-i (interactive): tetap membuka input agar bisa mengetik langsung.
  ii.	-t (tty): membuat terminal agar tampilannya seperti terminal Linux biasa.
 
- `mysql-axon` : Nama container yang ingin dimasuki.
- `bash` : Shell Linux yang akan dijalankan dalam container.

---

### Jalankan MySQL di Dalam Container

```bash
mysql -u root -p
```

![Login MySQL](/Project_1/img/6.png)
Perintah ini digunakan untuk masuk (login) ke MySQL client dari terminal atau shell — baik di komputer lokal maupun di dalam Docker container.

- `mysql` : Menjalankan MySQL Command Line Client (CLI), yaitu program untuk berinteraksi dengan MySQL lewat terminal.
- `-u root` : Opsi -u berarti "user". Masuk sebagai user root, yaitu administrator utama di MySQL.
- `-p` : Opsi -p memberitahu MySQL untuk meminta password (tidak langsung diketik di command line untuk keamanan). Setelah enter, akan diminta memasukkan password (contohnya root, sesuai yang di set di dalam Docker). 

---

### Jalankan File SQL di Dalam Container

```sql
source /Axon.sql;
```

![Eksekusi SQL](/Project_1/img/7.png)
Digunakan di dalam MySQL client (mysql>) untuk menjalankan file SQL eksternal.
- `source` : Perintah bawaan MySQL untuk menjalankan (mengeksekusi) seluruh isi file SQL baris demi baris.
- `/Axon.sql` : Path atau lokasi file SQL yang ingin dijalankan, dalam hal ini berada di root directory (/) dari sistem (misalnya Docker container).


---

### Menyambungkan Power BI dengan Docker Desktop

![Power BI Connect](/Project_1/img/8.png)
- Gunakan menu **Get Data** di Power BI.
- Pilih **MySQL Database**.
- Pastikan **MySQL Connector/ODBC** sudah terinstal.

---

### Load Data dari SQL Server di Dalam Container

![Data Loaded](/Project_1/img/9.png)
- Data dari database berhasil dimuat ke Power BI.

### Load Data dari SQL Server di Dalam Container

![Axon Visualization](/Project_1/img/10.png)
- Power BI berhasil terhubung dengan Docker

