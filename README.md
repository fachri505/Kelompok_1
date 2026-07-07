# sistem-operasi-si25-kelompok1

# Tugas Instalasi Debian 13 Headless - Kelompok 1


# Laporan Tugas Kelompok
## Instalasi Debian 13 Headless Web Server

**Mata Kuliah:** Sistem Operasi (SI-25)  
**Program Studi:** Sistem Informasi  
**Universitas Galuh**

---

# 👥 Anggota Kelompok 1 (SI-2025B)

1. Naufal Firmansyah — 7020250027
2. Mohammad Ihsan Bari — 7020250032
3. Hasya Fazza Ferdian — 7020250030
4. Muhammad Fachri Khairurrijal — 7020250033

---

# 🎯 Spesifikasi Lingkungan Server

| Komponen | Keterangan |
|----------|------------|
| Hypervisor | VMware Workstation Pro |
| Sistem Operasi | Debian 13 Headless (CLI) |
| Hostname | kelompok2sia |
| IP Address VM | 192.168.146.130 |
| Web Server | Nginx |
| Port Forwarding | Host 8080 → Guest 80 |

IP Address diperoleh menggunakan perintah:

```bash
ip a
```

Kemudian melihat interface **ens33**.

---

# 🛠️ Langkah-Langkah Praktikum

# 1. Instalasi Debian 13 Headless

Pada tahap pertama dilakukan instalasi sistem operasi Debian 13 menggunakan mode teks (CLI) tanpa desktop environment sehingga server menjadi lebih ringan dan efisien.

## Langkah 1. Boot Installer Debian

- Jalankan virtual machine menggunakan file ISO Debian 13.
- Pada menu awal installer pilih **Install** (bukan Graphical Install).

**Penjelasan**

Mode Install digunakan agar proses instalasi berlangsung menggunakan tampilan terminal (headless).

---

## Langkah 2. Memilih Bahasa

Pilih bahasa installer.

Contoh:

- English

Klik **Continue**.

**Penjelasan**

Bahasa installer hanya digunakan selama proses instalasi.

---

## Langkah 3. Memilih Negara

Pilih lokasi sesuai wilayah.

Contoh:

- Indonesia

Klik **Continue**.

**Penjelasan**

Lokasi digunakan untuk menentukan timezone dan mirror repository Debian.

---

## Langkah 4. Layout Keyboard

Pilih keyboard:

- American English

Klik Continue.

---

## Langkah 5. Konfigurasi Jaringan

Installer akan mendeteksi kartu jaringan secara otomatis.

Apabila menggunakan DHCP maka alamat IP akan diperoleh otomatis.

Jika diminta Domain Name dapat dikosongkan.

---

## Langkah 6. Konfigurasi Hostname

Masukkan hostname:

```text
kelompok2sia
```

Klik Continue.

**Penjelasan**

Hostname merupakan nama komputer yang akan digunakan di dalam jaringan.

---

## Langkah 7. Membuat User

Masukkan:

- Password root
- Nama user
- Username
- Password user

**Penjelasan**

Root digunakan sebagai administrator sedangkan user biasa digunakan untuk aktivitas harian.

---

## Langkah 8. Partisi Harddisk

Pilih menu:

```
Guided - use entire disk
```

Pilih harddisk:

```
/dev/sda
```

Kemudian pilih:

```
All files in one partition (recommended)
```

Lalu pilih:

```
Finish partitioning and write changes to disk
```

Pilih **Yes**.

**Penjelasan**

Metode Guided membuat partisi secara otomatis menggunakan seluruh kapasitas harddisk.

---

## Langkah 9. Instalasi Sistem

Installer mulai menyalin seluruh file sistem Debian ke harddisk.

Tunggu hingga proses selesai.

---

## Langkah 10. Software Selection

Pada menu Software Selection hanya centang:

- SSH Server
- Standard System Utilities

Hilangkan centang lainnya.

**Penjelasan**

SSH Server digunakan untuk mengakses server dari komputer lain.

Standard System Utilities merupakan utilitas dasar yang dibutuhkan sistem.

Tidak menginstal Desktop Environment agar server tetap ringan.

### Screenshot Software Selection

![Software Selection](Images/01-Software-Selection.png)

---

## Langkah 11. Instalasi GRUB

Saat muncul pertanyaan:

Install the GRUB boot loader?

Pilih:

```
Yes
```

Kemudian pilih lokasi:

```
/dev/sda
```

**Penjelasan**

GRUB merupakan bootloader yang digunakan untuk menjalankan sistem operasi ketika komputer dinyalakan.

---

## Langkah 12. Login Pertama

Setelah restart, login menggunakan user yang telah dibuat.

### Screenshot Login Debian

![Login Terminal](Images/02-Debian-Login.png)

---

# 2. Konfigurasi User Sudo dan Update Repository

Setelah instalasi selesai dilakukan konfigurasi hak akses administrator pada user biasa.

Masuk sebagai root kemudian jalankan:

```bash
apt update && apt upgrade -y
```

**Penjelasan**

Perintah ini memperbarui daftar repository dan menginstal seluruh paket terbaru sehingga sistem menjadi lebih aman dan stabil.

Selanjutnya install sudo:

```bash
apt install sudo -y
```

Tambahkan user ke grup sudo.

```bash
usermod -aG sudo kelompok2sia
```

Restart sistem.

```bash
reboot
```

Setelah login kembali, uji menggunakan:

```bash
sudo apt update
```

Apabila meminta password dan berhasil dijalankan berarti konfigurasi sudo berhasil.

### Screenshot

![Update](Images/07-apt-update.png)

![Install Sudo](Images/08-install-sudo.png)

![Usermod](Images/09-usermod.png)

---

# 3. Instalasi Web Server Nginx

Install seluruh paket yang dibutuhkan.

```bash
sudo apt install net-tools curl git nginx -y
```

Keterangan:

- net-tools digunakan untuk melihat konfigurasi jaringan.
- curl digunakan untuk melakukan request HTTP.
- git digunakan untuk clone repository.
- nginx digunakan sebagai web server.

Jalankan service.

```bash
sudo systemctl start nginx
```

Agar otomatis aktif saat boot.

```bash
sudo systemctl enable nginx
```

Periksa status service.

```bash
sudo systemctl status nginx
```

Status harus menunjukkan:

```
active (running)
```

### Screenshot

![Status Nginx](Images/03-nginx-status.png)

---

# 4. Membuat Halaman Profil Kelompok

Masuk ke direktori web.

```bash
cd /var/www/html
```

Edit file bawaan.

```bash
sudo nano index.html
```

Masukkan halaman HTML berisi identitas kelompok.

Simpan perubahan.

Restart nginx.

```bash
sudo systemctl restart nginx
```

**Penjelasan**

Restart dilakukan agar konfigurasi dan halaman web terbaru dimuat kembali oleh Nginx.

### Screenshot

![Edit HTML](Images/04-Edit-HTML.png)

---

# 5. Konfigurasi Port Forwarding VMware

Buka:

```
Edit
→ Virtual Network Editor
→ NAT Settings
```

Tambahkan aturan:

| Host Port | Guest Port |
|------------|------------|
| 8080 | 80 |

Simpan konfigurasi.

Buka browser Windows Host.

Akses:

```
http://localhost:8080
```

Apabila halaman profil kelompok muncul berarti konfigurasi berhasil.

### Screenshot NAT

![NAT](Images/05-nat-settings.png)

### Screenshot Browser

![Browser](Images/06-browser-host.png)

---

# 🎥 Link Video Demo

Tambahkan tautan video YouTube atau Google Drive di bawah ini.

```
https://youtube.com/...
```

---

# 📝 Kesimpulan

Berdasarkan praktikum yang telah dilakukan, proses instalasi Debian 13 Headless berhasil diselesaikan mulai dari pemasangan sistem operasi, konfigurasi pengguna, instalasi web server Nginx, hingga pengujian akses web melalui port forwarding VMware.

Melalui praktikum ini diperoleh pemahaman bahwa server berbasis Command Line Interface (CLI) lebih ringan dibandingkan server yang menggunakan Graphical User Interface (GUI), sehingga lebih efisien dalam penggunaan sumber daya komputer. Selain itu, penggunaan SSH mempermudah proses administrasi server dari komputer lain melalui jaringan.

## Poin-poin yang Dipelajari

- Memahami proses instalasi Debian 13 dalam mode headless.
- Memahami konfigurasi hostname, user, password, dan partisi harddisk.
- Memahami konfigurasi jaringan dasar pada Debian.
- Memahami penggunaan perintah apt untuk melakukan update sistem.
- Memahami cara memberikan hak akses administrator menggunakan sudo.
- Memahami instalasi dan konfigurasi web server Nginx.
- Memahami pengelolaan layanan menggunakan systemctl.
- Memahami cara melakukan port forwarding pada VMware.
- Memahami cara menguji web server melalui browser host.
- Menambah pengalaman dalam melakukan administrasi server Linux berbasis terminal.

Secara keseluruhan, praktikum ini memberikan pengalaman langsung mengenai instalasi, konfigurasi, dan pengelolaan server Debian 13 secara headless sebagai dasar administrasi sistem operasi Linux.# Kelompok_1
Tugas_Instal_debian
