# Program Sistem Penjadwalan Perkuliahan

## **Gambaran Umum Program**
Program ini adalah sistem manajemen jadwal kuliah yang memungkinkan pengguna untuk:
- Mengelola data mata kuliah dan mahasiswa
- Melihat jadwal perkuliahan
- Mengecek ketersediaan waktu
- Mengekspor data ke file

## **Struktur Program**

### **1. Header dan Namespace**
```cpp
#include <iostream>      // Input/Output
#include <fstream>       // File handling
#include <string>        // String manipulation
#include <cstdlib>       // System functions
#include <iomanip>       // Output formatting
#include <vector>        // Dynamic arrays
#include <algorithm>     // Algorithm functions
using namespace std;     // Standard namespace
```

### **2. Konstanta Kapasitas**
```cpp
const int MAX_SLOTS = 10;        // Maksimal slot jadwal per mahasiswa
const int MAX_MATKUL_DB = 100;   // Maksimal mata kuliah
const int MAX_MAHASISWA = 50;    // Maksimal mahasiswa
```

### **3. Struktur Data**

#### **MatkulMaster**
Menyimpan informasi mata kuliah:
- `namaMatkul`: Nama mata kuliah
- `jamMulai`: Waktu mulai (format HH.MM)
- `jamSelesai`: Waktu selesai
- `ruangan`: Lokasi kelas
- `sks`: Jumlah SKS
- `dosen`: Nama pengajar

#### **Mahasiswa**
Menyimpan data mahasiswa dan jadwalnya:
- `nama`: Nama mahasiswa
- `npm`: Nomor Pokok Mahasiswa
- `jadwal[]`: Array nama mata kuliah (10 slot)
- `ruangan[]`: Array ruangan untuk setiap slot
- `jamMulai[]`: Array waktu mulai
- `jamSelesai[]`: Array waktu selesai

### **4. Variabel Global**
```cpp
MatkulMaster databaseMatkul[MAX_MATKUL_DB];  // Database mata kuliah
Mahasiswa databaseMhs[MAX_MAHASISWA];        // Database mahasiswa
int jumlahMatkulTerdaftar = 0;               // Counter mata kuliah
int jumlahMhsTerdaftar = 0;                  // Counter mahasiswa
```

## **Fungsi-Fungsi Utama**

### **A. Fungsi Bantuan (Helper Functions)**
1. **`trim(string str)`** - Menghapus spasi di awal/akhir string
2. **`printHeader()`** - Mencetak header dengan border
3. **`validasiFormatWaktu()`** - Validasi format waktu HH.MM
4. **`cariMatkul()`** & **`cariMahasiswaByNama()`** - Fungsi pencarian

### **B. Fungsi File Handling**
1. **`muatDataMatkul()`** - Membaca data dari `matkul.txt`
   - Format: `Nama | JamMulai | JamSelesai | Ruangan | SKS | Dosen`
   - Parsing menggunakan delimiter '|'

2. **`muatDataMahasiswa()`** - Membaca data dari `mahasiswa.txt`
   - Format: `Nama | NPM | Matkul1|Matkul2|...`
   - Menghubungkan mahasiswa dengan data mata kuliah

### **C. CRUD Operations**

#### **1. Create (Tambah Data)**
- **`tambahMatkulBaru()`**: Menambah mata kuliah baru
- **`tambahMahasiswaBaru()`**: Mendaftarkan mahasiswa baru dengan mata kuliah yang diambil

#### **2. Read (Tampilkan Data)**
- **`tampilkanSemuaMatkul()`**: Menampilkan semua mata kuliah
- **`tampilkanJadwal()`**: Menampilkan jadwal per mahasiswa
- **`tampilkanSemuaJadwal()`**: Daftar semua mahasiswa dengan jumlah kelas

#### **3. Update (Edit Data)**
- **`editMatkul()`**: Mengedit data mata kuliah
- **`editMahasiswa()`**: Mengedit nama/NPM mahasiswa

#### **4. Delete (Hapus Data)**
- **`hapusMatkul()`**: Menghapus mata kuliah (dengan konfirmasi)
- **`hapusMahasiswa()`**: Menghapus data mahasiswa

### **D. Fitur Khusus**
1. **`cekKetersediaan()`** - Mengecek siapa yang kosong pada jam tertentu
2. **`exportJadwal()`** - Mengekspor semua jadwal ke file `export_jadwal.txt`
3. **`infoFile()`** - Menampilkan informasi format file

## **Alur Program**

### **1. Inisialisasi**
```cpp
int main() {
    // Buat file jika belum ada
    muatDataMatkul();    // Muat mata kuliah dulu
    muatDataMahasiswa(); // Lalu mahasiswa (bergantung pada data matkul)
    // Menu utama
}
```

### **2. Menu Utama**
Program menampilkan menu dengan 10 opsi:
1. Input Data Baru (mata kuliah/mahasiswa)
2. Lihat Jadwal per Mahasiswa
3. Lihat Semua Mahasiswa
4. Cek Ketersediaan Jam
5. Edit Data
6. Hapus Data
7. Lihat Database Mata Kuliah
8. Export Jadwal
9. Informasi File
10. Clear Terminal
11. Keluar

### **3. Data Persistence**
Data disimpan dalam file teks:
- **`matkul.txt`** - Database mata kuliah
- **`mahasiswa.txt`** - Database mahasiswa dan jadwal mereka

## **Fitur Keamanan dan Validasi**

### **1. Validasi Input**
- Format waktu harus HH.MM (contoh: 07.30)
- Validasi jam (7-17) dan menit (0-59)
- Pencegahan buffer overflow dengan batasan array

### **2. Error Handling**
- Pengecekan keberadaan file
- Handling stoi() exception
- Konfirmasi untuk operasi hapus

### **3. Konsistensi Data**
- Penghapusan mata kuliah otomatis menghapus dari jadwal mahasiswa
- Relasi referensial antara mahasiswa dan mata kuliah

## **Contoh Penggunaan**

### **A. Menambah Mata Kuliah**
```
Nama Matkul    : Algoritma dan Pemrograman
Jam Mulai      : 07.30
Jam Selesai    : 09.00
Ruangan        : R101
SKS            : 3
Nama Dosen     : Dr. Budi
```

### **B. Menambah Mahasiswa**
```
Nama Mahasiswa: Andi
NPM Mahasiswa : 1234567890

Matkul ke-1: Algoritma dan Pemrograman
   [OK] 07.30-09.00 | 3 SKS | R101 | Dosen: Dr. Budi
```

## **Keunggulan Program**
1. **Modular** - Fungsi terpisah dengan tanggung jawab jelas
2. **Persistent** - Data tersimpan di file
3. **User-friendly** - Menu interaktif dengan validasi
4. **Extensible** - Struktur data mudah dikembangkan

## **Keterbatasan**
1. Kapasitas tetap (array statis)
2. Tidak ada enkripsi data
3. Single-user system
4. Tidak ada backup otomatis

Program ini cocok untuk penggunaan skala kecil seperti kelas dengan maksimal 50 mahasiswa dan 100 mata kuliah.