### Penjelasan Kode Tugas Latihan 10

```cpp
/*
=============== Kelompok 3 Strukdat ==================

Nama Program            : Program Doubly Linked List untuk Pengelolaan Barang

Tanggal Buat : 02 November 2025

Deskripsi    : Program ini dibuat untuk membuat pengelolaan barang seperti latihan 9 yang lalu namun menggunakan semua primitive list doubly linked list

*/

#include <iostream>
#include <string>
#include <iomanip>

using namespace std;

// Struktur data untuk barang (doubly linked list)
struct Barang {
    string kodeBarang;
    string namaBarang;
    float harga;
    int banyak;
    float discount;
    Barang* next;
    Barang* prev;
};

typedef Barang* pointer;

// Pointer ke head list
pointer head = NULL;

// Fungsi untuk menghitung discount
float hitungDiscount(float harga, int banyak) {
    float total = harga * banyak;
    float discount = 0;

    if (total > 200000) {
        discount = 0.15; // 15%
    } else if (total > 100000) {
        discount = 0.10; // 10%
    } else if (total > 75000) {
        discount = 0.05; // 5%
    }
    // Total <= 75000 tidak dapat discount (discount tetap 0)

    return discount;
}

// Fungsi untuk menghitung jumlah setelah discount
float hitungJumlah(float harga, int banyak, float discount) {
    float total = harga * banyak;
    return total - (total * discount);
}

// 1. INSERT FIRST - Menambah node di awal list
void insertFirst() {
    /*
    ALUR KERJA INSERT FIRST:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    
    Langkah-langkah:
    1. Input data barang baru (X)
    2. Buat node baru untuk barang X
    3. Atur pointer baru:
       - newBarang->prev = NULL (karena akan menjadi head baru)
       - newBarang->next = head (menunjuk ke head lama)
    4. Jika head != NULL:
       - head->prev = newBarang (head lama menunjuk balik ke baru)
    5. head = newBarang (update head ke node baru)
    
    Kondisi Akhir: [HEAD] -> [X] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [X] <- [A] <- [B] <- [C]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [X] → [A] → [B] → [C] → NULL
                    ↓      ↑
                  HEAD ────┘
    */
    
    string kode, nama;
    float harga;
    int banyak;

    cout << "=== INSERT FIRST ===\n";
    cout << "Masukkan kode barang: ";
    cin >> kode;
    cout << "Masukkan nama barang: ";
    cin.ignore();
    getline(cin, nama);
    cout << "Masukkan harga: ";
    cin >> harga;
    cout << "Masukkan jumlah: ";
    cin >> banyak;

    float discount = hitungDiscount(harga, banyak);
    float totalSebelum = harga * banyak;
    float jumlahAkhir = hitungJumlah(harga, banyak, discount);

    pointer newBarang = new Barang();
    newBarang->kodeBarang = kode;
    newBarang->namaBarang = nama;
    newBarang->harga = harga;
    newBarang->banyak = banyak;
    newBarang->discount = discount;
    newBarang->prev = NULL;
    newBarang->next = head;

    if (head != NULL) {
        head->prev = newBarang;
    }
    head = newBarang;

    cout << "Barang berhasil ditambahkan di awal list!\n";
    cout << "Total sebelum discount: " << totalSebelum << endl;
    cout << "Discount: " << discount * 100 << "%" << endl;
    cout << "Jumlah akhir: " << jumlahAkhir << endl;
}

// 2. INSERT LAST - Menambah node di akhir list
void insertLast() {
    /*
    ALUR KERJA INSERT LAST:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    
    Langkah-langkah:
    1. Input data barang baru (X)
    2. Buat node baru untuk barang X
    3. Jika list kosong (head == NULL):
       - newBarang->prev = NULL
       - head = newBarang
    4. Jika list tidak kosong:
       - Traverse sampai node terakhir (temp->next == NULL)
       - temp->next = newBarang
       - newBarang->prev = temp
       - newBarang->next = NULL
    
    Kondisi Akhir: [HEAD] -> [A] -> [B] -> [C] -> [X] -> NULL
                   NULL <- [A] <- [B] <- [C] <- [X]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [A] → [B] → [C] → [X] → NULL
                    ↓              ↑
                  HEAD             │
                              last node
    */
    
    string kode, nama;
    float harga;
    int banyak;

    cout << "=== INSERT LAST ===\n";
    cout << "Masukkan kode barang: ";
    cin >> kode;
    cout << "Masukkan nama barang: ";
    cin.ignore();
    getline(cin, nama);
    cout << "Masukkan harga: ";
    cin >> harga;
    cout << "Masukkan jumlah: ";
    cin >> banyak;

    float discount = hitungDiscount(harga, banyak);
    float totalSebelum = harga * banyak;
    float jumlahAkhir = hitungJumlah(harga, banyak, discount);

    pointer newBarang = new Barang();
    newBarang->kodeBarang = kode;
    newBarang->namaBarang = nama;
    newBarang->harga = harga;
    newBarang->banyak = banyak;
    newBarang->discount = discount;
    newBarang->next = NULL;

    if (head == NULL) {
        newBarang->prev = NULL;
        head = newBarang;
    } else {
        pointer temp = head;
        while (temp->next != NULL) {
            temp = temp->next;
        }
        newBarang->prev = temp;
        temp->next = newBarang;
    }

    cout << "Barang berhasil ditambahkan di akhir list!\n";
    cout << "Total sebelum discount: " << totalSebelum << endl;
    cout << "Discount: " << discount * 100 << "%" << endl;
    cout << "Jumlah akhir: " << jumlahAkhir << endl;
}

// 3. INSERT AFTER - Menambah node setelah node tertentu
void insertAfter() {
    /*
    ALUR KERJA INSERT AFTER:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    Target: Insert X setelah B
    
    Langkah-langkah:
    1. Cari node target (B)
    2. Buat node baru (X)
    3. Atur pointer:
       - X->prev = B
       - X->next = B->next (C)
    4. Jika B->next != NULL (B bukan tail):
       - C->prev = X
    5. B->next = X
    
    Kondisi Akhir: [HEAD] -> [A] -> [B] -> [X] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [X] <- [C]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [A] → [B] → [X] → [C] → NULL
                    ↓      ↑    ↓    ↑
                  HEAD     │    └────┘
                           └─────────┘
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak bisa insert after.\n";
        return;
    }

    string kodeSebelum;
    cout << "Masukkan kode barang setelah mana ingin menambah: ";
    cin >> kodeSebelum;

    pointer temp = head;
    while (temp != NULL && temp->kodeBarang != kodeSebelum) {
        temp = temp->next;
    }

    if (temp == NULL) {
        cout << "Kode barang " << kodeSebelum << " tidak ditemukan!\n";
        return;
    }

    string kode, nama;
    float harga;
    int banyak;

    cout << "Masukkan kode barang baru: ";
    cin >> kode;
    cout << "Masukkan nama barang: ";
    cin.ignore();
    getline(cin, nama);
    cout << "Masukkan harga: ";
    cin >> harga;
    cout << "Masukkan jumlah: ";
    cin >> banyak;

    float discount = hitungDiscount(harga, banyak);
    float totalSebelum = harga * banyak;
    float jumlahAkhir = hitungJumlah(harga, banyak, discount);

    pointer newBarang = new Barang();
    newBarang->kodeBarang = kode;
    newBarang->namaBarang = nama;
    newBarang->harga = harga;
    newBarang->banyak = banyak;
    newBarang->discount = discount;

    // Atur pointer untuk doubly linked list
    newBarang->prev = temp;
    newBarang->next = temp->next;

    if (temp->next != NULL) {
        temp->next->prev = newBarang;
    }
    temp->next = newBarang;

    cout << "Barang berhasil ditambahkan setelah " << kodeSebelum << "!\n";
    cout << "Total sebelum discount: " << totalSebelum << endl;
    cout << "Discount: " << discount * 100 << "%" << endl;
    cout << "Jumlah akhir: " << jumlahAkhir << endl;
}

// 4. INSERT BEFORE - Menambah node sebelum node tertentu (TAMBAHAN DOUBLY LINKED LIST)
void insertBefore() {
    /*
    ALUR KERJA INSERT BEFORE:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    Target: Insert X sebelum B
    
    Langkah-langkah:
    1. Cari node target (B)
    2. Buat node baru (X)
    3. Atur pointer:
       - X->prev = B->prev (A)
       - X->next = B
    4. Jika B->prev != NULL (B bukan head):
       - A->next = X
    5. Jika B->prev == NULL (B adalah head):
       - head = X
    6. B->prev = X
    
    Kondisi Akhir: [HEAD] -> [A] -> [X] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [X] <- [B] <- [C]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [A] → [X] → [B] → [C] → NULL
                    ↓      ↑    ↓    ↑
                  HEAD     │    └────┘
                           └─────────┘
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak bisa insert before.\n";
        return;
    }

    string kodeSetelah;
    cout << "Masukkan kode barang sebelum mana ingin menambah: ";
    cin >> kodeSetelah;

    pointer temp = head;
    while (temp != NULL && temp->kodeBarang != kodeSetelah) {
        temp = temp->next;
    }

    if (temp == NULL) {
        cout << "Kode barang " << kodeSetelah << " tidak ditemukan!\n";
        return;
    }

    string kode, nama;
    float harga;
    int banyak;

    cout << "Masukkan kode barang baru: ";
    cin >> kode;
    cout << "Masukkan nama barang: ";
    cin.ignore();
    getline(cin, nama);
    cout << "Masukkan harga: ";
    cin >> harga;
    cout << "Masukkan jumlah: ";
    cin >> banyak;

    float discount = hitungDiscount(harga, banyak);
    float totalSebelum = harga * banyak;
    float jumlahAkhir = hitungJumlah(harga, banyak, discount);

    pointer newBarang = new Barang();
    newBarang->kodeBarang = kode;
    newBarang->namaBarang = nama;
    newBarang->harga = harga;
    newBarang->banyak = banyak;
    newBarang->discount = discount;

    // Atur pointer untuk doubly linked list
    newBarang->prev = temp->prev;
    newBarang->next = temp;

    if (temp->prev != NULL) {
        temp->prev->next = newBarang;
    } else {
        // Jika menambahkan sebelum head, update head
        head = newBarang;
    }
    temp->prev = newBarang;

    cout << "Barang berhasil ditambahkan sebelum " << kodeSetelah << "!\n";
    cout << "Total sebelum discount: " << totalSebelum << endl;
    cout << "Discount: " << discount * 100 << "%" << endl;
    cout << "Jumlah akhir: " << jumlahAkhir << endl;
}

// 5. DELETE FIRST - Menghapus node di awal list
void deleteFirst() {
    /*
    ALUR KERJA DELETE FIRST:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    
    Langkah-langkah:
    1. Simpan head lama di temp
    2. head = head->next (pindah ke node berikutnya)
    3. Jika head baru != NULL:
       - head->prev = NULL
    4. Hapus temp (node A)
    
    Kondisi Akhir: [HEAD] -> [B] -> [C] -> NULL
                   NULL <- [B] <- [C]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
                    ↓
                  HEAD
    AFTER:   NULL ← [B] → [C] → NULL
                    ↓
                  HEAD
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada yang dihapus.\n";
        return;
    }

    pointer temp = head;
    head = head->next;
    if (head != NULL) {
        head->prev = NULL;
    }
    cout << "Barang dengan kode " << temp->kodeBarang << " dihapus dari list.\n";
    delete temp;
}

// 6. DELETE LAST - Menghapus node di akhir list
void deleteLast() {
    /*
    ALUR KERJA DELETE LAST:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    
    Langkah-langkah:
    1. Traverse sampai node terakhir (C)
    2. Jika hanya ada 1 node:
       - Hapus head, set head = NULL
    3. Jika lebih dari 1 node:
       - node sebelum terakhir (B)->next = NULL
       - Hapus node terakhir (C)
    
    Kondisi Akhir: [HEAD] -> [A] -> [B] -> NULL
                   NULL <- [A] <- [B]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [A] → [B] → NULL
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada yang dihapus.\n";
        return;
    }

    if (head->next == NULL) {
        cout << "Barang dengan kode " << head->kodeBarang << " dihapus dari list.\n";
        delete head;
        head = NULL;
        return;
    }

    pointer temp = head;
    while (temp->next != NULL) {
        temp = temp->next;
    }

    temp->prev->next = NULL;
    cout << "Barang dengan kode " << temp->kodeBarang << " dihapus dari list.\n";
    delete temp;
}

// 7. DELETE AFTER - Menghapus node setelah node tertentu
void deleteAfter() {
    /*
    ALUR KERJA DELETE AFTER:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    Target: Hapus node setelah A (yaitu B)
    
    Langkah-langkah:
    1. Cari node sebelum target (A)
    2. Simpan node target (B) di toDelete
    3. A->next = B->next (C)
    4. Jika C != NULL:
       - C->prev = A
    5. Hapus B
    
    Kondisi Akhir: [HEAD] -> [A] -> [C] -> NULL
                   NULL <- [A] <- [C]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [A] → [C] → NULL
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada yang dihapus.\n";
        return;
    }

    string kodeSebelum;
    cout << "Masukkan kode barang setelah mana ingin menghapus: ";
    cin >> kodeSebelum;

    pointer temp = head;
    while (temp != NULL && temp->kodeBarang != kodeSebelum) {
        temp = temp->next;
    }

    if (temp == NULL) {
        cout << "Kode barang " << kodeSebelum << " tidak ditemukan!\n";
        return;
    }

    if (temp->next == NULL) {
        cout << "Tidak ada barang setelah " << kodeSebelum << " untuk dihapus!\n";
        return;
    }

    pointer toDelete = temp->next;
    temp->next = toDelete->next;
    if (toDelete->next != NULL) {
        toDelete->next->prev = temp;
    }
    cout << "Barang dengan kode " << toDelete->kodeBarang << " dihapus dari list setelah " << kodeSebelum << "!\n";
    delete toDelete;
}

// 8. DELETE BEFORE - Menghapus node sebelum node tertentu (TAMBAHAN DOUBLY LINKED LIST)
void deleteBefore() {
    /*
    ALUR KERJA DELETE BEFORE:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    Target: Hapus node sebelum B (yaitu A)
    
    Langkah-langkah:
    1. Cari node target (B)
    2. Simpan node sebelum target (A) di toDelete
    3. Jika A->prev != NULL (A bukan head):
       - node sebelum A->next = B
    4. Jika A->prev == NULL (A adalah head):
       - head = B
    5. B->prev = A->prev
    6. Hapus A
    
    Kondisi Akhir: [HEAD] -> [B] -> [C] -> NULL
                   NULL <- [B] <- [C]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [B] → [C] → NULL
                    ↓
                  HEAD
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada yang dihapus.\n";
        return;
    }

    string kodeSetelah;
    cout << "Masukkan kode barang sebelum mana ingin menghapus: ";
    cin >> kodeSetelah;

    pointer temp = head;
    while (temp != NULL && temp->kodeBarang != kodeSetelah) {
        temp = temp->next;
    }

    if (temp == NULL) {
        cout << "Kode barang " << kodeSetelah << " tidak ditemukan!\n";
        return;
    }

    if (temp->prev == NULL) {
        cout << "Tidak ada barang sebelum " << kodeSetelah << " untuk dihapus!\n";
        return;
    }

    pointer toDelete = temp->prev;
    if (toDelete->prev != NULL) {
        toDelete->prev->next = temp;
    } else {
        // Jika menghapus node sebelum head, update head
        head = temp;
    }
    temp->prev = toDelete->prev;
    cout << "Barang dengan kode " << toDelete->kodeBarang << " dihapus dari list sebelum " << kodeSetelah << "!\n";
    delete toDelete;
}

// 9. DELETE BY CODE - Menghapus node berdasarkan kode
void deleteByCode() {
    /*
    ALUR KERJA DELETE BY CODE:
    
    Kondisi Awal: [HEAD] -> [A] -> [B] -> [C] -> NULL
                   NULL <- [A] <- [B] <- [C]
    Target: Hapus node B
    
    Langkah-langkah:
    1. Cari node target (B)
    2. Jika B->prev != NULL (B bukan head):
       - A->next = B->next (C)
    3. Jika B->prev == NULL (B adalah head):
       - head = B->next (C)
    4. Jika B->next != NULL (B bukan tail):
       - C->prev = B->prev (A)
    5. Hapus B
    
    Kondisi Akhir: [HEAD] -> [A] -> [C] -> NULL
                   NULL <- [A] <- [C]
    
    Visualisasi:
    BEFORE:  NULL ← [A] → [B] → [C] → NULL
    AFTER:   NULL ← [A] → [C] → NULL
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada yang dihapus.\n";
        return;
    }

    string kode;
    cout << "Masukkan kode barang yang akan dihapus: ";
    cin >> kode;

    pointer temp = head;
    while (temp != NULL && temp->kodeBarang != kode) {
        temp = temp->next;
    }

    if (temp == NULL) {
        cout << "Kode barang " << kode << " tidak ditemukan!\n";
        return;
    }

    if (temp->prev != NULL) {
        temp->prev->next = temp->next;
    } else {
        // Jika menghapus head, update head
        head = temp->next;
    }

    if (temp->next != NULL) {
        temp->next->prev = temp->prev;
    }

    cout << "Barang dengan kode " << kode << " dihapus dari list.\n";
    delete temp;
}

// 10. SEARCHING - Mencari node berdasarkan kode
void searchBarang() {
    /*
    ALUR KERJA SEARCHING:
    
    Langkah-langkah:
    1. Mulai dari head
    2. Traverse list sambil mengecek kode barang
    3. Jika ditemukan, tampilkan informasi
    4. Jika sampai akhir tidak ditemukan, beri pesan error
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada yang dicari.\n";
        return;
    }

    string kode;
    cout << "Masukkan kode barang yang dicari: ";
    cin >> kode;

    pointer temp = head;
    int posisi = 1;
    while (temp != NULL) {
        if (temp->kodeBarang == kode) {
            float totalAwal = temp->harga * temp->banyak;
            float jumlahAkhir = hitungJumlah(temp->harga, temp->banyak, temp->discount);

            cout << "Barang ditemukan pada posisi " << posisi << ":\n";
            cout << "Kode: " << temp->kodeBarang << endl;
            cout << "Nama: " << temp->namaBarang << endl;
            cout << "Harga: " << temp->harga << endl;
            cout << "Banyak: " << temp->banyak << endl;
            cout << "Total Awal: " << totalAwal << endl;
            cout << "Discount: " << temp->discount * 100 << "%" << endl;
            cout << "Jumlah Akhir: " << jumlahAkhir << endl;
            return;
        }
        temp = temp->next;
        posisi++;
    }

    cout << "Kode barang " << kode << " tidak ditemukan!\n";
}

// 11. TRAVERSAL FORWARD - Menampilkan semua barang dari depan ke belakang
void traversalForward() {
    /*
    ALUR KERJA TRAVERSAL FORWARD:
    
    Langkah-langkah:
    1. Mulai dari head
    2. Tampilkan data node saat ini
    3. Pindah ke node berikutnya (next)
    4. Ulangi sampai node == NULL
    */
    
    if (head == NULL) {
        cout << "List kosong!\n";
        return;
    }

    pointer temp = head;
    int no = 1;

    cout << "\n=== DAFTAR SEMUA BARANG (FORWARD) ===\n";
    cout << "=============================================================================================================\n";
    while (temp != NULL) {
        float totalSebelum = temp->harga * temp->banyak;
        float jumlahAkhir = hitungJumlah(temp->harga, temp->banyak, temp->discount);

        cout << no++ << ". Kode: " << temp->kodeBarang
             << ", Nama: " << temp->namaBarang
             << ", Harga: " << temp->harga
             << ", Banyak: " << temp->banyak
             << ", Total: " << totalSebelum
             << ", Discount: " << temp->discount * 100 << "%"
             << ", Jumlah Akhir: " << jumlahAkhir << endl;
        temp = temp->next;
    }
    cout << "=============================================================================================================\n";
}

// 12. TRAVERSAL BACKWARD - Menampilkan semua barang dari belakang ke depan (TAMBAHAN DOUBLY LINKED LIST)
void traversalBackward() {
    /*
    ALUR KERJA TRAVERSAL BACKWARD:
    
    Langkah-langkah:
    1. Traverse sampai node terakhir
    2. Tampilkan data node saat ini
    3. Pindah ke node sebelumnya (prev)
    4. Ulangi sampai node == NULL
    */
    
    if (head == NULL) {
        cout << "List kosong!\n";
        return;
    }

    // Cari node terakhir
    pointer temp = head;
    while (temp->next != NULL) {
        temp = temp->next;
    }

    int no = 1;
    cout << "\n=== DAFTAR SEMUA BARANG (BACKWARD) ===\n";
    cout << "=============================================================================================================\n";
    while (temp != NULL) {
        float totalSebelum = temp->harga * temp->banyak;
        float jumlahAkhir = hitungJumlah(temp->harga, temp->banyak, temp->discount);

        cout << no++ << ". Kode: " << temp->kodeBarang
             << ", Nama: " << temp->namaBarang
             << ", Harga: " << temp->harga
             << ", Banyak: " << temp->banyak
             << ", Total: " << totalSebelum
             << ", Discount: " << temp->discount * 100 << "%"
             << ", Jumlah Akhir: " << jumlahAkhir << endl;
        temp = temp->prev;
    }
    cout << "=============================================================================================================\n";
}

// 13. COUNT NODES - Menghitung jumlah node dalam list
void countNodes() {
    /*
    ALUR KERJA COUNT NODES:
    
    Langkah-langkah:
    1. Mulai dari head dengan count = 0
    2. Traverse list, increment count untuk setiap node
    3. Tampilkan total count
    */
    
    int count = 0;
    pointer temp = head;

    while (temp != NULL) {
        count++;
        temp = temp->next;
    }

    cout << "Jumlah barang dalam list: " << count << endl;
}

// 14. UPDATE - Mengupdate data barang berdasarkan kode barang
void updateBarang() {
    /*
    ALUR KERJA UPDATE:
    
    Langkah-langkah:
    1. Cari node berdasarkan kode
    2. Tampilkan data lama
    3. Input data baru
    4. Update data dan hitung ulang discount
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada yang diupdate.\n";
        return;
    }

    string kode;
    cout << "Masukkan kode barang yang akan diupdate: ";
    cin >> kode;

    pointer temp = head;
    while (temp != NULL) {
        if (temp->kodeBarang == kode) {
            cout << "Data lama:\n";
            cout << "Nama: " << temp->namaBarang << endl;
            cout << "Harga: " << temp->harga << endl;
            cout << "Banyak: " << temp->banyak << endl;

            cout << "\nMasukkan data baru:\n";
            cout << "Masukkan nama barang: ";
            cin.ignore();
            getline(cin, temp->namaBarang);
            cout << "Masukkan harga: ";
            cin >> temp->harga;
            cout << "Masukkan jumlah: ";
            cin >> temp->banyak;

            temp->discount = hitungDiscount(temp->harga, temp->banyak);

            cout << "Data barang berhasil diupdate!\n";
            return;
        }
        temp = temp->next;
    }

    cout << "Kode barang " << kode << " tidak ditemukan!\n";
}

// 15. LAPORAN LENGKAP
void laporanLengkap() {
    /*
    ALUR KERJA LAPORAN LENGKAP:
    
    Langkah-langkah:
    1. Traverse seluruh list
    2. Hitung statistik: total harga, discount, harga tertinggi/terendah
    3. Tampilkan dalam format tabel
    4. Tampilkan ringkasan dan aturan discount
    */
    
    if (head == NULL) {
        cout << "List kosong! Tidak ada laporan.\n";
        return;
    }

    pointer temp = head;
    int no = 1;
    float totalHarga = 0;
    float hargaTertinggi = 0;
    float hargaTerendah = head->harga;
    float totalDiscount = 0;

    cout << "\n=== LAPORAN HARGA BARANG ===\n";
    cout << "========================================================================================================\n";
    cout << left << setw(5) << "No"
         << setw(12) << "Kode Barang"
         << setw(20) << "Nama Barang"
         << setw(10) << "Harga"
         << setw(8) << "Banyak"
         << setw(12) << "Total Awal"
         << setw(10) << "Disc"
         << setw(12) << "Jumlah Akhir" << endl;
    cout << "========================================================================================================\n";

    while (temp != NULL) {
        float totalAwal = temp->harga * temp->banyak;
        float jumlahAkhir = hitungJumlah(temp->harga, temp->banyak, temp->discount);
        float discountAmount = totalAwal * temp->discount;

        totalHarga += jumlahAkhir;
        totalDiscount += discountAmount;

        if (temp->harga > hargaTertinggi) {
            hargaTertinggi = temp->harga;
        }
        if (temp->harga < hargaTerendah) {
            hargaTerendah = temp->harga;
        }

        cout << left << setw(5) << no++
             << setw(12) << temp->kodeBarang
             << setw(20) << temp->namaBarang
             << setw(10) << temp->harga
             << setw(8) << temp->banyak
             << setw(12) << totalAwal
             << setw(10) << temp->discount * 100 << "%"
             << setw(12) << jumlahAkhir << endl;

        temp = temp->next;
    }

    cout << "========================================================================================================\n";
    cout << "Total Harga Setelah Discount: " << totalHarga << endl;
    cout << "Total Discount yang Diberikan: " << totalDiscount << endl;
    cout << "Harga Tertinggi per item: " << hargaTertinggi << endl;
    cout << "Harga Terendah per item: " << hargaTerendah << endl;

    // Tampilkan aturan discount
    cout << "\n=== ATURAN DISCOUNT ===\n";
    cout << "Total > 200.000 : Discount 15%\n";
    cout << "Total > 100.000 : Discount 10%\n";
    cout << "Total > 75.000  : Discount 5%\n";
    cout << "Total ≤ 75.000  : Tidak ada discount (0%)\n";
}

// 16. CHECK EMPTY - Mengecek apakah list kosong
void checkEmpty() {
    /*
    ALUR KERJA CHECK EMPTY:
    
    Langkah-langkah:
    1. Cek jika head == NULL
    2. Tampilkan status sesuai kondisi
    */
    
    if (head == NULL) {
        cout << "List KOSONG\n";
    } else {
        cout << "List TIDAK kosong\n";
    }
}

// Menu utama
void menu() {
    int pilihan;

    do {
        cout << "\n=== MENU MANAJEMEN BARANG (DOUBLY LINKED LIST) ===\n";
        cout << "1. Masukkan Barang Melalui Insert First\n";
        cout << "2. Masukkan Barang Melalui Insert Last\n";
        cout << "3. Masukkan Barang Melalui Insert After\n";
        cout << "4. Masukkan Barang Melalui Insert Before\n";
        cout << "5. Menghapus Barang Melalui Delete First\n";
        cout << "6. Menghapus Barang Melalui Delete Last\n";
        cout << "7. Menghapus Barang Melalui Delete After\n";
        cout << "8. Menghapus Barang Melalui Delete Before\n";
        cout << "9. Menghapus Barang Melalui Delete By Code\n";
        cout << "10. Mencari Barang\n";
        cout << "11. Tampilkan Semua Barang (Forward)\n";
        cout << "12. Tampilkan Semua Barang (Backward)\n";
        cout << "13. Jumlah Barang dalam List\n";
        cout << "14. Update Barang\n";
        cout << "15. Laporan Lengkap\n";
        cout << "16. Check List (Apakah List Kosong Atau Tidak)\n";
        cout << "0. Keluar\n";
        cout << "Pilihan: ";
        cin >> pilihan;

        switch (pilihan) {
            case 1:
                insertFirst();
                break;
            case 2:
                insertLast();
                break;
            case 3:
                insertAfter();
                break;
            case 4:
                insertBefore();
                break;
            case 5:
                deleteFirst();
                break;
            case 6:
                deleteLast();
                break;
            case 7:
                deleteAfter();
                break;
            case 8:
                deleteBefore();
                break;
            case 9:
                deleteByCode();
                break;
            case 10:
                searchBarang();
                break;
            case 11:
                traversalForward();
                break;
            case 12:
                traversalBackward();
                break;
            case 13:
                countNodes();
                break;
            case 14:
                updateBarang();
                break;
            case 15:
                laporanLengkap();
                break;
            case 16:
                checkEmpty();
                break;
            case 0:
                cout << "Terima kasih banyak telah menggunakan program ini. Have a great day :)\n"; break;
            default:
                cout << "Maaf, pilihan tidak valid :(\n";
        }
    } while (pilihan != 0);
}

int main() {
    menu();
    return 0;
}
```


