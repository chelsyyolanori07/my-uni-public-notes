# Konten PPT: Implementasi Struktur Data dalam ChronoMind

---
Diinput setelah bagian slide keunggulan sistem (setelah slide 7).. ini kurang lebih untuk isi konten materi nya nanti kalo mau ada yang ditambahin mangga teteh2

## **1. Doubly Linked List**
### **Penjelasan Singkat**
Struktur data linier dengan node yang memiliki pointer ke node sebelumnya dan berikutnya, memungkinkan traversal dua arah.

### **Kegunaan dalam Program**
- **Penyimpanan utama** semua data tugas
- Mendukung operasi **insert, edit, delete, dan traversal** dua arah
- Menyimpan urutan kronologis tugas berdasarkan waktu pembuatan

### **Implementasi Kode**
```cpp
struct DLLNode {
    Task data;
    DLLNode* prev;
    DLLNode* next;
};

// Global pointers
DLLNode* g_head = NULL;
DLLNode* g_tail = NULL;

// Add to DLL
void addTaskToDLL(Task newTask) {
    DLLNode* newNode = createDLLNode(newTask);
    if (g_head == NULL) {
        g_head = g_tail = newNode;
    } else {
        g_tail->next = newNode;
        newNode->prev = g_tail;
        g_tail = newNode;
    }
}
```

---

## **2. Queue**
### **Penjelasan Singkat**
Struktur data FIFO (First-In First-Out) untuk mengatur antrian prioritas.

### **Kegunaan dalam Program**
- **Penjadwalan prioritas** tugas urgent
- Mengelola tugas dengan **deadline < 7 hari**
- Menampilkan tugas yang perlu segera diselesaikan

### **Implementasi Kode**
```cpp
struct QueueNode {
    int taskId;
    QueueNode* next;
};

QueueNode* g_queueFront = NULL;
QueueNode* g_queueRear = NULL;

// Enqueue tugas prioritas
void enqueue(int taskId) {
    time_t now = time(NULL);
    double daysUntil = difftime(deadline, now) / (60*60*24);
    if (!completed && daysUntil <= 7 && daysUntil >= 0) {
        enqueue(taskId);  // Masuk ke queue prioritas
    }
}
```

---

## **3. Stack**
### **Penjelasan Singkat**
Struktur data LIFO (Last-In First-Out) untuk menyimpan riwayat operasi.

### **Kegunaan dalam Program**
- **Sistem Undo/Redo** operasi terakhir
- Menyimpan snapshot tugas sebelum perubahan
- Maksimal 10 operasi yang bisa di-undo

### **Implementasi Kode**
```cpp
struct StackNode {
    string operation;
    Task taskSnapshot;
    StackNode* next;
};

StackNode* g_undoStack = NULL;
StackNode* g_redoStack = NULL;

void pushUndo(string operation, Task task) {
    StackNode* newNode = new StackNode;
    newNode->operation = operation;
    newNode->taskSnapshot = task;
    newNode->next = g_undoStack;
    g_undoStack = newNode;
}
```

---

## **4. Binary Search Tree (BST)**
### **Penjelasan Singkat**
Struktur data pohon biner dengan sifat: left < root < right untuk pencarian efisien.

### **Kegunaan dalam Program**
- **Hierarki akademik**: Mata Kuliah → Kategori → Jenis
- **Pencarian cepat** berdasarkan kategori
- **Pengelompokan** tugas secara terstruktur

### **Implementasi Kode**
```cpp
struct BSTNode {
    string key;  // Format: "course_category_type"
    vector<int> taskIds;
    BSTNode* left;
    BSTNode* right;
};

BSTNode* g_bstRoot = NULL;

// Insert ke BST
string bstKey = course + "_" + category + "_" + type;
insertBST(g_bstRoot, bstKey, newTask.id);
```

---

## **5. Array/Vector & Indexing System**
### **Penjelasan Singkat**
Koleksi data dengan akses indeks konstan, digunakan untuk sistem indexing cepat.

### **Kegunaan dalam Program**
- **Sistem indexing** untuk pencarian O(1)
- **Mapping ID → Node** dengan `std::map`
- **Pencarian cepat** berdasarkan kata kunci

### **Implementasi Kode**
```cpp
// Index system untuk pencarian cepat
map<int, DLLNode*> g_taskIndex;

// Menambahkan ke index
g_taskIndex[newTask.id] = newNode;

// Pencarian cepat
if (g_taskIndex.find(id) != g_taskIndex.end()) {
    DLLNode* node = g_taskIndex[id];
}
```

---

## **6. Pointer**
### **Penjelasan Singkat**
Variabel yang menyimpan alamat memori, fundamental untuk struktur data dinamis.

### **Kegunaan dalam Program**
- **Menghubungkan node** di Linked List dan BST
- **Dynamic memory allocation** untuk struktur data
- **Traversal** dan manipulasi data

### **Implementasi Kode**
```cpp
// Pointer di berbagai struktur
DLLNode* prev;  // Pointer ke node sebelumnya
DLLNode* next;  // Pointer ke node berikutnya
BSTNode* left;  // Pointer ke child kiri
BSTNode* right; // Pointer ke child kanan

// Dynamic allocation
DLLNode* newNode = new DLLNode;
BSTNode* bstNode = new BSTNode;
```

---

## **7. Standard Library & External Libraries**
### **Penjelasan Singkat**
Modul dan library built-in C++ untuk fungsi tambahan.

### **Kegunaan dalam Program**
1. **`<ctime>` / `<chrono>`**: 
   - Menghitung deadline
   - Sistem reminder otomatis
   - Tracking waktu

2. **Notification Libraries**:
   - Notifikasi desktop real-time
   - Popup warning untuk deadline mendekat

3. **`<algorithm>` / `<map>` / `<vector>`**:
   - Operasi pencarian dan sorting
   - Management koleksi data

### **Implementasi Kode**
```cpp
#include <ctime>    // time(), difftime()
#include <map>      // std::map untuk indexing
#include <vector>   // std::vector untuk koleksi
#include <algorithm>// std::find(), std::transform()

// Contoh penggunaan
time_t now = time(NULL);
double minutesLeft = difftime(deadline, now) / 60.0;
```

---

### **Keunggulan Integrasi**
1. **Efisiensi**: Pencarian cepat dengan BST + Map
2. **Fleksibilitas**: Edit mudah dengan DLL
3. **Prioritas**: Manajemen tugas dengan Queue
4. **Safety**: Backup dengan Stack Undo/Redo

---

### **Keberhasilan Sistem**
- **Performansi**: O(log n) untuk pencarian dengan BST
- **Reliabilitas**: Backup data dengan file I/O
- **Usability**: Interface interaktif dengan notifikasi
- **Scalability**: Struktur data yang mudah dikembangkan
