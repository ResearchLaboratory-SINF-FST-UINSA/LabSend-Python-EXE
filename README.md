\# LabSend-Python



\*\*LabSend-Python\*\* adalah aplikasi desktop Windows berbasis Python yang digunakan untuk transfer file cepat dari mahasiswa ke komputer lab melalui QR Code. Aplikasi ini dibuat untuk menggantikan proses transfer file yang kurang efisien seperti login WhatsApp, email, penggunaan flashdisk, atau media eksternal lain ketika mahasiswa ingin mencetak dokumen di lab.



Aplikasi berjalan sebagai `.exe` dengan system tray dan local web server menggunakan FastAPI. Operator atau aslab dapat menampilkan QR Code, menerima file masuk, melihat daftar upload, membuka file di tab browser baru, mengunduh file, membuka file dengan aplikasi default Windows, serta menandai file sebagai sudah dicetak.



\## Alur Utama



```text

Operator menjalankan LabSend

→ QR Code ditampilkan

→ Mahasiswa scan QR Code

→ Mahasiswa mengisi nama lengkap dan NIM

→ Mahasiswa upload satu atau banyak file

→ File tersimpan di komputer lab

→ Operator melihat, mengunduh, membuka, atau mencetak file

```



\## Tujuan Project



Project ini dibuat untuk membantu proses operasional lab komputer, khususnya saat mahasiswa membutuhkan transfer file cepat untuk keperluan print tanpa harus login ke akun pribadi atau menggunakan media eksternal.



LabSend-Python ditujukan sebagai solusi ringan untuk satu komputer lab, sehingga tidak membutuhkan Docker, server pusat, atau instalasi database eksternal.



\## Fitur Utama



\* Transfer file melalui QR Code

\* QR Code otomatis refresh setiap 2 menit

\* Token QR sekali pakai

\* Form nama lengkap dan NIM mahasiswa

\* Upload satu atau banyak file

\* Progress upload melalui browser

\* Dashboard operator lokal

\* Tombol \*\*Lihat\*\* untuk membuka file di tab browser baru

\* Tombol \*\*Download\*\* untuk mengunduh file dari browser

\* Tombol \*\*Open App\*\* untuk membuka file dengan aplikasi default Windows

\* Tombol \*\*Tandai Sudah Dicetak\*\*

\* Validasi ekstensi dan ukuran file

\* Blokir file berbahaya seperti `.exe`, `.bat`, `.cmd`, `.ps1`, dan sejenisnya

\* Folder penyimpanan upload dapat diatur

\* Riwayat upload menggunakan SQLite

\* System tray menggunakan pystray

\* Build menjadi `.exe` menggunakan PyInstaller



\## Stack Teknologi



```text

Python

FastAPI

Uvicorn

pystray

SQLite

Jinja2

HTML, CSS, JavaScript

qrcode

Pillow

PyInstaller

```



\## Struktur Project



```text

LabSend-Python/

├─ app/

│  ├─ main.py

│  ├─ server.py

│  ├─ tray.py

│  ├─ config.py

│  ├─ database.py

│  ├─ qr\_service.py

│  ├─ upload\_service.py

│  ├─ file\_validator.py

│  ├─ file\_service.py

│  ├─ print\_service.py

│  ├─ firewall\_service.py

│  ├─ cleanup\_service.py

│  ├─ templates/

│  ├─ static/

│  └─ assets/

│

├─ data/

│  ├─ labsend.db

│  ├─ config.json

│  └─ uploads/

│

├─ build/

├─ dist/

├─ requirements.txt

├─ LabSend.spec

└─ README.md

```



\## Instalasi Development



\### 1. Clone Repository



```bash

git clone https://github.com/UmarAziz01/LabSend-Python.git

cd LabSend-Python

```



\### 2. Buat Virtual Environment



```bash

python -m venv .venv

```



Aktifkan virtual environment di Windows PowerShell:



```powershell

.venv\\Scripts\\Activate.ps1

```



Atau di Command Prompt:



```cmd

.venv\\Scripts\\activate.bat

```



\### 3. Install Dependency



```bash

pip install -r requirements.txt

```



\### 4. Jalankan Aplikasi Mode Development



```bash

python -m app.main

```



Jika berhasil, aplikasi akan berjalan di system tray dan server lokal aktif pada port default:



```text

http://localhost:4711

```



\## Halaman Aplikasi



\### Dashboard Operator



```text

http://localhost:4711/admin

```



Dashboard ini digunakan operator atau aslab untuk melihat QR Code, daftar upload masuk, file mahasiswa, dan aksi seperti lihat, download, open app, serta tandai sudah dicetak.



\### Halaman QR



```text

http://localhost:4711/qr

```



Halaman ini digunakan untuk menampilkan QR Code yang dapat discan oleh mahasiswa.



\### Halaman Upload Mahasiswa



Mahasiswa membuka halaman upload dari QR Code. URL QR tidak menggunakan `localhost`, tetapi menggunakan IP LAN komputer lab.



Contoh:



```text

http://192.168.10.25:4711/u/<token>

```



\## Build Menjadi EXE



Project ini menggunakan PyInstaller untuk build aplikasi menjadi `.exe`.



\### 1. Pindah ke Direktori Project



```powershell

cd B:\\LAB\\labsend-py\\labsend

```



\### 2. Jalankan Build Menggunakan PyInstaller dari Virtual Environment



```powershell

\& "B:\\LAB\\labsend-py\\.venv\\Scripts\\pyinstaller.exe" LabSend.spec --clean --noconfirm

```



Hasil build akan berada di folder:



```text

dist/

```



\## Menjalankan Hasil Build



Setelah proses build selesai, jalankan file `.exe` yang ada di folder `dist`.



Contoh:



```text

dist\\LabSend\\LabSend.exe

```



Catatan:



\* Komputer target tidak perlu menginstall Python.

\* Jika menggunakan mode `onedir`, seluruh folder hasil build harus disalin, bukan hanya file `.exe`.

\* Pastikan folder `data/` tersedia untuk menyimpan konfigurasi, database, dan file upload.



\## Troubleshooting



\### Port 4711 Sudah Digunakan



Jika aplikasi gagal berjalan karena port `4711` sudah digunakan, cek proses yang sedang memakai port tersebut.



```cmd

netstat -ano | findstr :4711

```



Contoh output:



```text

TCP    0.0.0.0:4711    0.0.0.0:0    LISTENING    22424

```



Matikan proses berdasarkan PID:



```cmd

taskkill /PID 22424 /F

```



Setelah proses berhasil dihentikan, jalankan ulang aplikasi LabSend.



\### QR Tidak Bisa Dibuka dari HP



Pastikan:



\* HP dan komputer lab berada di jaringan yang sama

\* QR Code menggunakan IP LAN komputer, bukan `localhost`

\* Firewall Windows mengizinkan koneksi masuk ke port aplikasi

\* Port default `4711` tidak diblokir jaringan



\### File Tidak Muncul di Dashboard



Pastikan:



\* Upload berhasil dari browser mahasiswa

\* Folder upload dapat ditulis oleh aplikasi

\* Database SQLite tidak terkunci

\* Aplikasi dijalankan dengan permission yang cukup



\## Catatan Keamanan



\* Token QR hanya berlaku sementara

\* Token QR bersifat sekali pakai

\* File executable dan script berbahaya diblokir

\* File diakses menggunakan `file\_id`, bukan path langsung

\* Folder upload dapat dikonfigurasi oleh operator

\* Dashboard operator sebaiknya hanya digunakan dari komputer lab



\## Collaborator



\*\*Umar Abdul Aziz\*\*

GitHub: \[UmarAziz01](https://github.com/UmarAziz01)

Mahasiswa Sistem Informasi 2023

Website: \[azizlab.my.id](https://azizlab.my.id)



\## Lisensi



Project ini dibuat untuk kebutuhan pengembangan dan operasional lab komputer. Lisensi dapat disesuaikan sesuai kebutuhan repository.



