#REVIEW MATERI "PROCESS CONTROL"

## **1. Components of a Process**  
Dalam sistem operasi Linux/UNIX, sebuah proses terdiri dari dua bagian utama:  
1. **Address space** → Memori yang diberikan kernel kepada proses.
2. **Kernel data structure** → Menyimpan informasi tentang status, prioritas, sumber daya, file dan port network yang digunakan oleh proses, signal mask proses, serta owner dari sebuah proses

Selain itu, **thread** adalah unit eksekusi di dalam proses yang berbagi address space tetapi memiliki stack sendiri.

---

## **2. The PID: Process ID Number**  
Setiap proses memiliki **Process ID (PID)** yang bersifat unik. **PID** adalah sebuah integer yang diberikan oleh kernel dari setiap proses Ketika proses itu dibuat. 

Konsep **namespace** memungkinkan proses yang berbeda memiliki PID yang sama. Namespace digunakan untuk membuat container, yang mengisolasi enviroments yang memiliki system mereka sendiri. 

---

## **3. The PPID: Parent Process ID Number**  
Setiap proses juga berhubungan dengan parent process, yaitu proses yang membuatnya. **Parent Process ID (PPID)** digunakan untuk merujuk parent proses di berbagai system calls 

---

## **4. The UID and EUID: User ID and Effective User ID**  
- **User ID (UID)** → Menentukan siapa pemilik proses.
- **Effective User ID (EUID)** → Digunakan untuk menentukan hak akses proses terhadap sumber daya.

Misalnya, saat menggunakan `sudo`, EUID berubah menjadi **root (UID 0)** sementara UID asli tetap milik pengguna biasa.

---

## **5. Lifecycle of a Process**  
Tahapan siklus hidup proses dalam Linux:
1. **Creation** → Proses dibuat menggunakan `fork()`.
2. **Execution** → Proses dapat mengganti dirinya dengan program lain menggunakan `exec()`.
3. **Waiting** → Proses dapat menunggu event tertentu atau I/O.
4. **Termination** → Proses berhenti secara normal (`exit()`) atau dipaksa (`kill`).

Saat booting, kernel memulai proses pertama (`init` atau `systemd`), yang kemudian meluncurkan proses-proses lain.

---

## **6. Signals**  
Sinyal adalah cara untuk mengirim notifikasi kepada proses. Sinyal digunakan untuk memberi tahu sebuah proses bahwa suatu event telah terjadi. Terdapat sekitar **30 jenis sinyal** yang digunakan untuk berbagai tujuan, seperti:
- **Interaksi antar proses.**
- **Mengontrol proses dari terminal.**
- **Manajemen proses oleh administrator.**
- **Respon terhadap error atau event dari kernel.**

Beberapa sinyal umum dan fungsinya:

- **KILL (`kill -9 PID`)** → Memaksa proses berhenti tanpa bisa dicegah atau ditangani oleh proses.
- **INT (`Ctrl + C`)** → Meminta proses berhenti dengan aman. Biasanya digunakan oleh terminal untuk menghentikan program yang berjalan.
- **TERM** → Meminta proses untuk keluar dengan baik. Ini adalah sinyal default dari `kill PID`.
- **HUP** → Dikirim ketika terminal ditutup. Sering digunakan untuk memberitahu daemon agar membaca ulang konfigurasi.
- **QUIT** → Mirip dengan TERM tetapi akan menghasilkan core dump jika tidak ditangani.

Sinyal dapat dikirim menggunakan perintah `kill`, `pkill`, atau `killall`.

---

## **7. Process Monitoring (`ps`, `top`, `htop`)**  
Alat untuk memantau proses dalam Linux:
- **`ps aux`** → Menampilkan daftar proses secara statis.
- **`top`** → Menampilkan proses secara dinamis dengan pembaruan berkala.
- **`htop`** → Versi lebih interaktif dari `top` yang memungkinkan navigasi dengan keyboard.

---

## **8. Process Priority (Nice & Renice)**  
Linux menggunakan **nice value** untuk menentukan prioritas proses:
- Nilai **-20** → Prioritas tinggi.
- Nilai **19** → Prioritas rendah.

Perintah yang digunakan:
- `nice -n 10 command` → Menjalankan proses dengan prioritas rendah.
- `renice -n -5 -p PID` → Mengubah prioritas proses yang sedang berjalan.

Kernel menggunakan **Completely Fair Scheduler (CFS)** untuk menyeimbangkan alokasi CPU berdasarkan nice value.

---

## **9. Process Debugging (`strace`, `/proc` filesystem)**  
Untuk menganalisis aktivitas proses:
- **`strace -p PID`** → Melihat sistem call yang dilakukan proses.
- **`/proc/PID/`** → Direktori pseudo yang berisi informasi tentang proses.

File `/proc/PID/status` menampilkan informasi status dan penggunaan memori dari suatu proses.

---

## **10. Scheduled Processes (`cron`)**  
### **Cron Job**  
Cron digunakan untuk menjalankan tugas secara otomatis pada waktu tertentu. Format umum:
```
*     *     *     *     *  perintah
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +----- Hari dalam seminggu (0 - 6, Minggu=0)
|     |     |     +------- Bulan (1 - 12)
|     |     +--------- Tanggal (1 - 31)
|     +----------- Jam (0 - 23)
+------------- Menit (0 - 59)
```
Contoh:
```
30 2 * * * /usr/bin/python3 /home/user/script.py
```
Perintah di atas menjalankan script Python setiap hari pukul 02:30.

---


