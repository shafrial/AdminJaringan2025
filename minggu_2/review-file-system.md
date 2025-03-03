# FILESYSTEM DALAM UNIX/LINUX  

Filesystem dalam UNIX/Linux adalah sistem yang mengorganisir, menyimpan, dan mengelola file dalam penyimpanan komputer. Komponen utama dari filesystem meliputi:  

1. **Namespace** → Sistem penamaan dan hierarki direktori.  
2. **API** → Sistem call untuk manipulasi file dan direktori.  
3. **Model Keamanan** → Mekanisme perlindungan dan hak akses file.  
4. **Implementasi** → Software yang menghubungkan model logis ke hardware.  

Filesystem populer yang digunakan dalam UNIX/Linux antara lain **ext4, XFS, UFS, ZFS, Btrfs**, serta filesystem kompatibel dengan Windows seperti **NTFS dan FAT**.  

---

## **1. Pathnames**  
Pathname adalah lokasi suatu file dalam filesystem dan dapat berupa:  

- **Absolute pathname** → Dimulai dari root (`/`), contoh:  
  ```
  /home/user/file.txt
  ```  
- **Relative pathname** → Berdasarkan lokasi saat ini, contoh:  
  ```
  ./file.txt
  ```  

---

## **2. Mounting dan Unmounting Filesystem**  
Filesystem bisa dimount menggunakan perintah berikut:  
```bash
mount /dev/sda4 /users
```
Unmounting dilakukan dengan:  
```bash
umount /users
```
Jika filesystem masih digunakan, gunakan:  

- **Lazy unmount:**  
  ```bash
  umount -l /users
  ```
- **Force unmount:**  
  ```bash
  umount -f /users
  ```

Untuk mengetahui proses yang menggunakan filesystem:  
```bash
lsof /home/user
fuser -m /home/user
```

---

## **3. Organization of the File Tree**  
Struktur utama filesystem UNIX/Linux mencakup:  

- **`/` (Root Directory)** → Direktori utama.  
- **`/boot`** → Berisi kernel dan file booting.  
- **`/etc`** → File konfigurasi sistem.  
- **`/sbin` dan `/bin`** → Utilitas sistem esensial.  
- **`/tmp`** → Direktori untuk file sementara.  
- **`/dev`** → Mewakili perangkat hardware.  
- **`/lib` dan `/lib64`** → Library sistem.  
- **`/usr`** → Program tambahan dan library non-esensial.  
- **`/var`** → Menyimpan log dan file yang sering berubah.  

---

## **4. Jenis File dalam Filesystem**  
UNIX/Linux mendukung beberapa jenis file:  

1. **Regular files** → File teks, biner, program eksekusi (`/bin/bash`).  
2. **Directories** → Penyimpanan file dalam hierarki.  
3. **Character device files** → Berkomunikasi dengan hardware berbasis karakter (`/dev/tty0`).  
4. **Block device files** → Berkomunikasi dengan perangkat penyimpanan (`/dev/sda`).  
5. **Local domain sockets** → Untuk komunikasi antar proses (`/var/run/docker.sock`).  
6. **Named pipes (FIFO)** → Komunikasi antar proses dalam satu sistem.  
7. **Symbolic links** → Referensi ke file lain (`ln -s /bin /usr/bin`).  

---

## **5. File Attributes**  
Setiap file dalam UNIX/Linux memiliki atribut yang terdiri dari:  

1. **Permission Bits** → Hak akses untuk pemilik, grup, dan pengguna lain.  
2. **Special Mode Bits** → SetUID, SetGID, dan Sticky Bit.  
3. **Ownership (Kepemilikan File)** → Setiap file memiliki pemilik dan grup.  
4. **Timestamps** → UNIX/Linux melacak waktu akses, modifikasi, dan perubahan metadata file.  

### **a) Permission Bits (Hak Akses File)**  
Hak akses dibagi menjadi tiga kategori:  

- **Owner (u)** → Pemilik file.  
- **Group (g)** → Pengguna dalam grup pemilik file.  
- **Others (o)** → Pengguna lain di sistem.  

Jenis izin:  

- **r (read)** → Membaca isi file atau melihat isi direktori.  
- **w (write)** → Mengedit isi file atau menambah/menghapus file dalam direktori.  
- **x (execute)** → Menjalankan file atau memasuki direktori.  

Melihat hak akses dengan `ls -l`:  
```bash
ls -l file.sh
```

Mengubah hak akses dengan `chmod`:  
```bash
chmod u+w,g-x,o=r file.sh
chmod 755 file.sh
```

### **b) Special Mode Bits (SetUID, SetGID, Sticky Bit)**  

1. **SetUID (`chmod u+s`)** → Saat file dijalankan, proses akan berjalan dengan hak pemilik file.  
2. **SetGID (`chmod g+s`)** → File yang dibuat di dalam direktori akan mewarisi grup direktori.  
3. **Sticky Bit (`chmod +t`)** → Hanya pemilik file yang bisa menghapusnya dalam direktori bersama.  

Contoh Sticky Bit pada `/tmp`:  
```bash
chmod +t /tmp
ls -ld /tmp
```

### **c) Ownership (Kepemilikan File)**  

Mengubah pemilik file dengan `chown`:  
```bash
chown user:group file.sh
```
Mengubah grup file dengan `chgrp`:  
```bash
chgrp group file.sh
```

### **d) Timestamps (Waktu Akses & Modifikasi)**  
UNIX/Linux mencatat tiga jenis waktu pada setiap file:  

1. **Access Time (`atime`)** → Waktu terakhir file dibaca.  
2. **Modify Time (`mtime`)** → Waktu terakhir isi file diubah.  
3. **Change Time (`ctime`)** → Waktu terakhir metadata file berubah.  

Melihat timestamp dengan `stat`:  
```bash
stat file.sh
```
Mengubah waktu akses dan modifikasi dengan `touch`:  
```bash
touch -m -d "2025-03-01 12:00" file.sh
```

---

## **6. Manajemen Kepemilikan dan Akses File**  
### **Mengatur Default Permission (`umask`)**  
`umask` menentukan default permission untuk file dan direktori baru.  

```bash
umask 022
```

Menghasilkan:  
- File: `644` (rw-r--r--)  
- Direktori: `755` (rwxr-xr-x)  

---

## **7. Access Control Lists (ACLs)**  
ACLs memungkinkan kontrol hak akses yang lebih fleksibel dibandingkan permission standar.  

Menampilkan ACL:  
```bash
getfacl file.sh
```
Menambahkan akses pengguna tertentu:  
```bash
setfacl -m u:user:rw file.sh
```
Menghapus entri ACL:  
```bash
setfacl -x u:user file.sh
```

---

# **🎯 Kesimpulan**  
Pemahaman tentang **filesystem dan atribut file dalam UNIX/Linux** sangat penting untuk:  

✔ **Mengelola hak akses dan keamanan sistem**  
✔ **Mengatur kepemilikan file dalam server multi-user**  
✔ **Memahami bagaimana sistem operasi menangani file dan direktori**  

Sebagai mahasiswa Teknik Informatika, **pemahaman mendalam tentang permission, special bits, timestamps, dan ACLs akan sangat berguna dalam administrasi sistem dan keamanan jaringan**. Jika ada bagian yang perlu diperjelas, silakan beri tahu saya! 🚀  
