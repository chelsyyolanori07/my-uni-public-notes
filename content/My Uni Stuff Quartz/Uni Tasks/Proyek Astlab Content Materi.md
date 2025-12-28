# **SISTEM PENJADWALAN PERKULIAHAN**

## **3. PENGENALAN PROYEK**

### **Apa Itu Sistem Penjadwalan Perkuliahan?**
Program ini adalah sistem manajemen jadwal kuliah berbasis C++ yang memungkinkan pengelolaan data perkuliahan secara digital. Program ini dirancang untuk membantu administrasi akademik dalam mengatur jadwal perkuliahan, data mahasiswa, dan mata kuliah.

### **Fitur Utama Program:**
1. **Manajemen Data Mata Kuliah** - CRUD (Create, Read, Update, Delete)
2. **Manajemen Data Mahasiswa** - Pendaftaran dan pengelolaan jadwal
3. **Pengecekan Ketersediaan** - Melihat siapa yang kosong pada jam tertentu
4. **Export Data** - Menyimpan jadwal ke file eksternal
5. **Persistence Data** - Penyimpanan data di file untuk penggunaan berulang

### **Tujuan Pengembangan:**
- Menggantikan sistem manual dengan sistem digital
- Mempermudah pencarian jadwal mahasiswa
- Meminimalisir konflik jadwal
- Menyediakan laporan yang terstruktur

## **4. MATERI YANG DIGUNAKAN DALAM PROGRAM**

### **List Materi C++ yang Diimplementasikan:**
1. **Operasi File (File Handling)**
2. **Struktur Data (Struct)**
3. **Array dan String Manipulation**
4. **Fungsi dan Prosedur**
5. **Looping dan Conditional**
6. **Input/Output Formatting**
7. **Vector (STL)**
8. **Error Handling**
9. **Constants dan Global Variables**
10. **Preprocessor Directives**

## **5. IMPLEMENTASI MATERI DALAM KODE**

### **1. OPERASI FILE (FILE HANDLING)**

#### **Penjelasan:**
Operasi file digunakan untuk menyimpan dan membaca data dari file eksternal. Program menggunakan dua file utama:
- `matkul.txt` - Menyimpan data mata kuliah
- `mahasiswa.txt` - Menyimpan data mahasiswa dan jadwalnya

#### **Implementasi dalam Program:**
```cpp
// Membaca data dari file matkul.txt
void muatDataMatkul() {
    ifstream file("matkul.txt");
    string line;
    jumlahMatkulTerdaftar = 0;

    if (!file.is_open()) {
        cout << "File matkul.txt tidak ditemukan, akan dibuat baru." << endl;
        return;
    }

    while (getline(file, line) && jumlahMatkulTerdaftar < MAX_MATKUL_DB) {
        // Lewati baris kosong
        if (trim(line) == "") continue;

        // Parsing format: Nama | JamMulai | JamSelesai | Ruangan | SKS | Dosen
        vector<string> parts;
        size_t start = 0;
        size_t end = 0;

        // Parse dengan delimiter '|'
        while ((end = line.find('|', start)) != string::npos) {
            string part = line.substr(start, end - start);
            parts.push_back(trim(part));
            start = end + 1;
        }
        // Ambil bagian terakhir
        if (start < line.length()) {
            parts.push_back(trim(line.substr(start)));
        }

        // Pastikan ada minimal 6 bagian
        if (parts.size() >= 6) {
            databaseMatkul[jumlahMatkulTerdaftar].namaMatkul = parts[0];
            databaseMatkul[jumlahMatkulTerdaftar].jamMulai = parts[1];
            databaseMatkul[jumlahMatkulTerdaftar].jamSelesai = parts[2];
            databaseMatkul[jumlahMatkulTerdaftar].ruangan = parts[3];
            try {
                databaseMatkul[jumlahMatkulTerdaftar].sks = stoi(parts[4]);
            } catch (...) {
                databaseMatkul[jumlahMatkulTerdaftar].sks = 3;
            }
            databaseMatkul[jumlahMatkulTerdaftar].dosen = parts[5];

            jumlahMatkulTerdaftar++;
        }
    }
    file.close();
}
```

#### **Penjelasan Kode:**
- `ifstream` untuk membaca file (input file stream)
- `ofstream` untuk menulis file (output file stream)
- `getline()` membaca per baris dari file
- Parsing menggunakan delimiter '|' untuk memisahkan data
- Error handling dengan `try-catch` untuk konversi string ke integer

---

### **2. STRUKTUR DATA (STRUCT)**

#### **Penjelasan:**
Struct digunakan untuk membuat tipe data bentukan yang menyimpan beberapa variabel berbeda dalam satu kesatuan.

#### **Implementasi dalam Program:**
```cpp
// Struct untuk mata kuliah
struct MatkulMaster {
    string namaMatkul;
    string jamMulai;
    string jamSelesai;
    string ruangan;
    int sks;
    string dosen;
};

// Struct untuk mahasiswa
struct Mahasiswa {
    string nama;
    string npm;
    string jadwal[MAX_SLOTS + 1];
    string ruangan[MAX_SLOTS + 1];
    string jamMulai[MAX_SLOTS + 1];
    string jamSelesai[MAX_SLOTS + 1];
};

// Array global untuk menyimpan data
MatkulMaster databaseMatkul[MAX_MATKUL_DB];
Mahasiswa databaseMhs[MAX_MAHASISWA];
```

#### **Penjelasan Kode:**
- `MatkulMaster` menyimpan semua informasi tentang mata kuliah
- `Mahasiswa` menyimpan data pribadi dan jadwal dalam array
- Array `databaseMatkul` dan `databaseMhs` sebagai database in-memory
- Menggunakan `string` untuk teks dan `int` untuk numerik

---

### **3. ARRAY DAN STRING MANIPULATION**

#### **Penjelasan:**
Array digunakan untuk menyimpan kumpulan data dengan tipe sama. String manipulation untuk memproses teks.

#### **Implementasi dalam Program:**
```cpp
// Fungsi untuk menghapus spasi di awal/akhir string
string trim(string str) {
    size_t first = str.find_first_not_of(" \t\r\n");
    if (first == string::npos) return "";
    size_t last = str.find_last_not_of(" \t\r\n");
    return str.substr(first, (last - first + 1));
}

// Array untuk jadwal mahasiswa
string jadwal[MAX_SLOTS + 1];
string ruangan[MAX_SLOTS + 1];
string jamMulai[MAX_SLOTS + 1];
string jamSelesai[MAX_SLOTS + 1];

// Validasi format waktu
bool validasiFormatWaktu(string waktu) {
    if (waktu.length() != 5) return false;
    if (waktu[2] != '.') return false;

    string jam = waktu.substr(0, 2);
    string menit = waktu.substr(3, 2);

    for (char c : jam) if (!isdigit(c)) return false;
    for (char c : menit) if (!isdigit(c)) return false;

    int h = stoi(jam);
    int m = stoi(menit);

    return (h >= 7 && h <= 17 && m >= 0 && m < 60);
}
```

#### **Penjelasan Kode:**
- `trim()` menggunakan `find_first_not_of()` dan `substr()`
- Array multidimensi untuk menyimpan jadwal per slot
- `substr()` untuk mengambil bagian dari string
- `stoi()` untuk konversi string ke integer
- Validasi format HH.MM dengan pengecekan karakter

---

### **4. FUNGSI DAN PROSEDUR**

#### **Penjelasan:**
Fungsi digunakan untuk modularisasi kode dan menghindari pengulangan.

#### **Implementasi dalam Program:**
```cpp
// Fungsi untuk menampilkan header dengan border
void printHeader(string title) {
    int panjang = title.length() + 4;
    cout << "\n";
    cout << string(panjang, '=') << endl;
    cout << "  " << title << endl;
    cout << string(panjang, '=') << endl;
}

// Fungsi untuk mencari mata kuliah
bool cariMatkul(string namaDicari, MatkulMaster &result) {
    for (int i = 0; i < jumlahMatkulTerdaftar; i++) {
        if (databaseMatkul[i].namaMatkul == namaDicari) {
            result = databaseMatkul[i];
            return true;
        }
    }
    return false;
}

// Fungsi untuk menambah mahasiswa baru
void tambahMahasiswaBaru() {
    // ... kode lengkap untuk input data mahasiswa
}
```

#### **Penjelasan Kode:**
- Fungsi dengan parameter dan return value
- Pass by reference untuk mengembalikan multiple values
- Void function untuk prosedur yang tidak mengembalikan nilai
- Function overloading konsep dengan nama sama tetapi parameter berbeda

---

### **5. LOOPING DAN CONDITIONAL**

#### **Penjelasan:**
Digunakan untuk iterasi dan pengambilan keputusan dalam program.

#### **Implementasi dalam Program:**
```cpp
// Do-while untuk validasi input
do {
    cout << "Masukkan Jam Mulai      : ";
    getline(cin, jamMulai);
    if (!validasiFormatWaktu(jamMulai)) {
        cout << "   [!] Format salah! Gunakan format HH.MM (contoh: 07.30)\n" << endl;
    }
} while (!validasiFormatWaktu(jamMulai));

// While loop untuk input matkul
while(true) {
    cout << "Matkul ke-" << count << ": ";
    getline(cin, matkulInput);

    if (matkulInput == "0") break;
    // ... proses data
}

// For loop untuk iterasi array
for (int i = 0; i < jumlahMatkulTerdaftar; i++) {
    cout << (i+1) << ". " << databaseMatkul[i].namaMatkul
         << " (" << databaseMatkul[i].jamMulai << "-"
         << databaseMatkul[i].jamSelesai << ")" << endl;
}

// Switch-case untuk menu utama
switch(pilihan) {
    case 1: { /* tambah data */ break; }
    case 2: { /* lihat jadwal */ break; }
    // ... cases lainnya
}
```

#### **Penjelasan Kode:**
- `do-while` untuk validasi input wajib
- `while(true)` dengan `break` untuk loop tak terbatas
- `for` loop untuk iterasi array dengan index
- `switch-case` untuk menu selection
- `if-else` untuk conditional logic

---

### **6. INPUT/OUTPUT FORMATTING**

#### **Penjelasan:**
Memformat tampilan output agar lebih rapi dan mudah dibaca.

#### **Implementasi dalam Program:**
```cpp
// Menggunakan iomanip untuk formatting
#include <iomanip>

// Menampilkan data dalam format tabel
void tampilkanSemuaMatkul() {
    printHeader("DATABASE MATA KULIAH");

    if (jumlahMatkulTerdaftar == 0) {
        cout << "\n(Database masih kosong)\n" << endl;
        return;
    }

    cout << "\n" << left << setw(4) << "No"
         << setw(30) << "Mata Kuliah"
         << setw(8) << "SKS"
         << setw(20) << "Waktu"
         << setw(10) << "Ruangan"
         << "Dosen" << endl;
    printLine();

    for (int i = 0; i < jumlahMatkulTerdaftar; i++) {
        string waktu = databaseMatkul[i].jamMulai + "-" + databaseMatkul[i].jamSelesai;
        cout << left << setw(4) << (i + 1)
             << setw(30) << databaseMatkul[i].namaMatkul
             << setw(8) << databaseMatkul[i].sks
             << setw(20) << waktu
             << setw(10) << databaseMatkul[i].ruangan
             << databaseMatkul[i].dosen << endl;
    }
    printLine();
    cout << "\nTotal: " << jumlahMatkulTerdaftar << " mata kuliah terdaftar.\n" << endl;
}
```

#### **Penjelasan Kode:**
- `setw()` untuk mengatur lebar kolom
- `left` untuk perataan kiri
- `cout << endl` untuk newline
- String concatenation dengan operator `+`
- Membuat border dengan `string(panjang, '=')`

---

### **7. VECTOR (STANDARD TEMPLATE LIBRARY)**

#### **Penjelasan:**
Vector digunakan sebagai array dinamis untuk menampung data sementara.

#### **Implementasi dalam Program:**
```cpp
// Menggunakan vector untuk parsing
#include <vector>

// Parsing line dari file
vector<string> parts;
size_t start = 0;
size_t end = 0;

// Parse dengan delimiter '|'
while ((end = line.find('|', start)) != string::npos) {
    string part = line.substr(start, end - start);
    parts.push_back(trim(part));  // Menambahkan ke vector
    start = end + 1;
}
// Ambil bagian terakhir
if (start < line.length()) {
    parts.push_back(trim(line.substr(start)));
}

// Vector untuk daftar matkul
vector<string> matkulList;
matkulList.push_back(matkul);  // Menambah elemen
```

#### **Penjelasan Kode:**
- `vector<string>` untuk array string dinamis
- `push_back()` untuk menambah elemen
- Tidak perlu menentukan ukuran awal
- Dapat diakses seperti array dengan operator `[]`

---

### **8. ERROR HANDLING**

#### **Penjelasan:**
Menangani kemungkinan error untuk mencegah crash program.

#### **Implementasi dalam Program:**
```cpp
// Try-catch untuk konversi string ke int
try {
    databaseMatkul[jumlahMatkulTerdaftar].sks = stoi(parts[4]);
} catch (...) {
    databaseMatkul[jumlahMatkulTerdaftar].sks = 3;  // Default value
}

// Pengecekan file
if (!file.is_open()) {
    cout << "File matkul.txt tidak ditemukan, akan dibuat baru." << endl;
    return;
}

// Validasi input
if (pilih < 1 || pilih > jumlahMatkulTerdaftar) {
    cout << "\n>> Batal edit.\n" << endl;
    return;
}

// Konfirmasi sebelum hapus
cout << "\nApakah Anda yakin ingin menghapus mata kuliah '" 
     << namaMatkul << "'? (y/n): ";
char konfirmasi;
cin >> konfirmasi;
cin.ignore();

if (konfirmasi != 'y' && konfirmasi != 'Y') {
    cout << "\n>> Batal hapus.\n" << endl;
    return;
}
```

#### **Penjelasan Kode:**
- `try-catch` untuk menangani exception
- Validasi range input dengan `if` condition
- Konfirmasi user untuk operasi destruktif
- Pengecekan keberadaan file sebelum operasi

---

### **9. CONSTANTS DAN GLOBAL VARIABLES**

#### **Penjelasan:**
Constants untuk nilai tetap, global variables untuk data yang diakses banyak fungsi.

#### **Implementasi dalam Program:**
```cpp
// Constants untuk batasan
const int MAX_SLOTS = 10;
const int MAX_MATKUL_DB = 100;
const int MAX_MAHASISWA = 50;

// Global variables (database)
MatkulMaster databaseMatkul[MAX_MATKUL_DB];
int jumlahMatkulTerdaftar = 0;

Mahasiswa databaseMhs[MAX_MAHASISWA];
int jumlahMhsTerdaftar = 0;

// Preprocessor directives untuk clear terminal
#ifdef _WIN32
    system("cls");
#else
    system("clear");
#endif
```

#### **Penjelasan Kode:**
- `const` untuk nilai yang tidak berubah
- Global array sebagai database program
- Counter variables untuk tracking jumlah data
- `#ifdef` untuk conditional compilation berdasarkan OS

---

### **10. MAIN FUNCTION DAN PROGRAM FLOW**

#### **Penjelasan:**
Fungsi main sebagai entry point program dengan alur kontrol utama.

#### **Implementasi dalam Program:**
```cpp
int main() {
    // Inisialisasi file
    ofstream f1("matkul.txt", ios::app); f1.close();
    ofstream f2("mahasiswa.txt", ios::app); f2.close();

    // Muat data dari file
    muatDataMatkul();
    muatDataMahasiswa();

    int pilihan;
    do {
        // Tampilkan menu
        printHeader("SISTEM PENJADWALAN PERKULIAHAN");
        cout << "\nMENU UTAMA:\n" << endl;
        cout << "  1. Input Data Baru" << endl;
        // ... opsi menu lainnya
        
        // Input pilihan
        cout << "\n------------------------------------------------------------" << endl;
        cout << "Pilihan Anda: ";
        cin >> pilihan;
        cin.ignore();

        // Switch-case untuk menu
        switch(pilihan) {
            case 1: { /* ... */ break; }
            case 2: { /* ... */ break; }
            // ... cases lainnya
            case 0: {
                printHeader("TERIMA KASIH");
                cout << "\nProgram selesai. Terima kasih banyak telah menggunakan program ini :)\n" << endl;
                break;
            }
            default: {
                cout << "\n>> Pilihan tidak valid. Silakan coba lagi..\n" << endl;
            }
        }

        // Pause sebelum kembali ke menu
        if (pilihan != 0 && pilihan != 10) {
            cout << "\nTekan Enter untuk melanjutkan...";
            cin.get();
        }

    } while (pilihan != 0);

    return 0;
}
```

#### **Penjelasan Kode:**
- `do-while` loop untuk menu berulang
- `switch-case` untuk routing berdasarkan pilihan
- Modular function calls untuk setiap fitur
- Exit condition dengan `pilihan != 0`
- User-friendly prompts dan feedback