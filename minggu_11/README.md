# Rangkuman: Protokol dan Infrastruktur Email

## 1. Protokol Mail

### SMTP (Simple Mail Transfer Protocol)
- Digunakan untuk **mengirim email** dari klien ke server atau antar server email.
- Bekerja di **port 25**, **587** (untuk TLS), atau **465** (untuk SMTPS).
- Hanya digunakan untuk pengiriman, **bukan untuk menerima atau membaca email**.

### POP3 (Post Office Protocol v3)
- Digunakan untuk **mengambil email** dari server ke perangkat klien.
- Secara default, **menghapus email dari server** setelah diunduh.
- Bekerja di **port 110** (default) dan **port 995** untuk **POP3S (POP3 Secure)**.
- Cocok untuk pengguna yang hanya mengakses email dari satu perangkat.

### IMAP (Internet Message Access Protocol)
- Digunakan untuk **mengakses dan mengelola email langsung di server**.
- Tidak menghapus email secara otomatis, memungkinkan sinkronisasi antar banyak perangkat.
- Bekerja di **port 143** (default) dan **port 993** untuk **IMAPS (IMAP Secure)**.
- Cocok untuk pengguna multi-perangkat.

### POP3S (POP3 Secure)
- Versi aman dari POP3 yang menggunakan **enkripsi SSL/TLS**.
- Beroperasi pada **port 995**.
- Menjaga kerahasiaan data saat pengambilan email dari server.

---

## 2. Informasi Mail Server dalam Sebuah Domain

Untuk mengetahui informasi mail server dari suatu domain, digunakan **DNS record bernama MX (Mail Exchange)**.

```bash
nslookup -type=MX example.com
```

**Peran Mail Server**:  
  - **Inbound Server**: Menerima email dari luar (via SMTP) dan menyimpannya di mailbox penerima.  
  - **Outbound Server**: Mengirim email ke server lain (via SMTP).  

**Autentikasi dan Keamanan**:  
  - Mekanisme seperti **SPF, DKIM, DMARC** digunakan untuk memverifikasi keaslian email dan mencegah spoofing/spam.  
  - Contoh software: Postfix, Exim, Microsoft Exchange.  

**Konfigurasi Pengguna**:  
  - Saat setup email, pengguna perlu memasukkan:  
    - **Incoming Server** (POP3/IMAP) untuk menerima email.  
    - **Outgoing Server** (SMTP) untuk mengirim email.

## 3. Introduction to Electronic Mail
![](https://media.geeksforgeeks.org/wp-content/uploads/20200731122504/Email1.png)

### Komponen Utama Email
1. Alamat email: Ini adalah pengidentifikasi unik untuk setiap pengguna, biasanya dalam format name@domain.com.
2. Klien email: Ini adalah program perangkat lunak yang digunakan untuk mengirim, menerima, dan mengelola email, seperti Gmail, Outlook, atau Apple Mail.
3. Server email: Ini adalah sistem komputer yang bertanggung jawab untuk menyimpan dan meneruskan email ke penerima yang dituju.

### Komponen Sistem Email
1. User Agent(UA) : Program yang digunakan untuk mengirim dan menerima email. Kadang-kadang, program ini disebut sebagai pembaca surat.
2. Message Transfer Agent(MTA) : Bertanggung jawab untuk transfer email dari satu sistem ke sistem lainnya. Untuk mengirim email, sebuah sistem harus memiliki MTA klien dan MTA sistem.
3. Mailbox : Merupakan file pada hard drive lokal untuk mengumpulkan email. Email yang terkirim ada dalam file ini.
4. Spool File : File yang berisi email yang akan dikirim; UA menambahkan email keluar ke file ini, dan MTA mengekstrak email untuk dikirim.

#### Kelebihan & Kekurangan Email:  
- **Kelebihan**:  
  - Komunikasi cepat dan praktis secara global.
  - Pesan mudah disimpan dan dicari kembali.
  - Bisa mengirim lampiran (dokumen, gambar, video).
  - Lebih hemat biaya dibanding surat atau faks.
  - Tersedia 24 jam nonstop.  
- **Kekurangan**:  
  - Rentan terhadap spam dan phishing.
  - Terlalu banyak email bisa membingungkan.
  - Mengurangi interaksi langsung.
  - Risiko salah paham karena tidak ada ekspresi wajah atau nada suara.
  - Masalah teknis bisa mengganggu layanan.




