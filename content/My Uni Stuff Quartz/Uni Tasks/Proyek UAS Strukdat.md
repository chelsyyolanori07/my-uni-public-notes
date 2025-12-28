# Penjelasan Detail Program ChronoMind - Academic Task System

Program ini adalah sistem manajemen tugas akademik yang canggih bernama **ChronoMind** yang menggunakan berbagai struktur data dan fitur pemrograman untuk membantu mahasiswa mengelola tugas-tugas kuliah mereka.

## **Struktur Program Secara Keseluruhan**

### **1. STRUKTUR DATA YANG DIGUNAKAN**

#### **A. Struct Task**
```cpp
struct Task {
    int id;                    // ID unik tugas
    string name;              // Nama tugas
    string course;            // Mata kuliah
    string category;          // Kategori (Quiz/Tugas/Ujian/Proyek)
    string type;              // Tipe (Individu/Kelompok)
    time_t deadline;          // Waktu deadline
    bool completed;           // Status penyelesaian
    time_t createdAt;         // Waktu dibuat
    time_t completedAt;       // Waktu diselesaikan
    time_t lastNotified;      // Waktu notifikasi terakhir
    // Flags untuk notifikasi bertahap
    bool notified3Hours;
    bool notified1Hour;
    bool notified30Min;
    bool notified5Min;
    bool notifiedOverdue;
};
```

#### **B. Double Linked List (DLL) - Database Utama**
```cpp
struct DLLNode {
    Task data;
    DLLNode* prev;
    DLLNode* next;
};
```
- **Fungsi**: Menyimpan semua tugas secara berurutan
- **Keuntungan**: Bisa travers maju dan mundur, mudah insert/delete

#### **C. Binary Search Tree (BST) - Hierarki Akademik**
```cpp
struct BSTNode {
    string key;              // Format: "course_category_type"
    vector<int> taskIds;     // ID tugas yang terkait
    BSTNode* left;
    BSTNode* right;
};
```
- **Fungsi**: Mengorganisir tugas berdasarkan hierarki akademik
- **Keuntungan**: Pencarian cepat, struktur terorganisir

#### **D. Queue - Prioritas Deadline**
```cpp
struct QueueNode {
    int taskId;
    QueueNode* next;
};
```
- **Fungsi**: Menyimpan tugas dengan deadline mendesak (7 hari ke depan)
- **Tipe**: FIFO (First-In-First-Out)

#### **E. Stack - Sistem Undo/Redo**
```cpp
struct StackNode {
    string operation;        // "ADD", "DELETE", "EDIT", "COMPLETE"
    Task taskSnapshot;       // Salinan data tugas
    StackNode* next;
};
```
- **Fungsi**: Menyimpan riwayat operasi untuk undo/redo
- **Maksimal**: 10 operasi undo dan redo

### **2. VARIABEL GLOBAL**

```cpp
DLLNode* g_head = NULL;           // Head DLL
DLLNode* g_tail = NULL;           // Tail DLL  
BSTNode* g_bstRoot = NULL;        // Root BST
QueueNode* g_queueFront = NULL;   // Front queue
QueueNode* g_queueRear = NULL;    // Rear queue
StackNode* g_undoStack = NULL;    // Stack undo
StackNode* g_redoStack = NULL;    // Stack redo (baru)
map<int, DLLNode*> g_taskIndex;   // Map untuk akses cepat
```

### **3. FITUR UTAMA PROGRAM**

#### **A. Sistem Notifikasi Cerdas**
```cpp
void checkSmartNotifications()
```
- **Fungsi**: Memantau deadline dan mengirim notifikasi otomatis
- **Jadwal**: 
  - 3 jam sebelum deadline
  - 1 jam sebelum deadline  
  - 30 menit sebelum deadline
  - 5 menit sebelum deadline
  - Setelah deadline terlewat

#### **B. Threading untuk Monitoring Background**
```cpp
void* monitorThreadFunction(void* param)
```
- **Fungsi**: Berjalan di background untuk mengecek notifikasi
- **Interval**: Setiap 30 detik
- **Cross-platform**: Mendukung Windows dan Linux

#### **C. Sistem Undo/Redo**
```cpp
void pushUndo(string operation, Task task)
void performUndo()
void pushRedo(string operation, Task task)  
void performRedo()
```
- **Mendukung**: ADD, DELETE, EDIT, COMPLETE
- **Batasan**: Maksimal 10 operasi

#### **D. Persistensi Data dengan File**
```cpp
void saveToFile()
void loadFromFile()
```
- **Format**: File teks dengan delimiter '|'
- **Nama file**: `chronomind_data.txt`

### **4. ALUR KERJA PROGRAM**

#### **A. Inisialisasi**
1. Load data dari file
2. Start background monitoring thread
3. Tampilkan menu utama

#### **B. Operasi pada Tugas**
```cpp
void addTask()        // Tambah tugas baru
void deleteTask()     // Hapus tugas
void editTask()       // Edit tugas  
void markCompleted()  // Tandai selesai
```

#### **C. Tampilan Data**
```cpp
void displayAllTasks()        // Tampilkan semua tugas (DLL)
void displayBSTHierarchy()    // Tampilkan hierarki (BST)
void displayPriorityQueue()   // Tampilkan tugas urgent (Queue)
void displayStatistics()      // Tampilkan statistik
void displayWeeklyTimeline()  // Tampilkan timeline mingguan
```

### **5. KEUNGGULAN ARSITEKTUR**

#### **A. Multi-Structure Approach**
- **DLL**: Untuk traversal sequential
- **BST**: Untuk organisasi hierarkis  
- **Queue**: Untuk prioritas
- **Stack**: Untuk riwayat operasi
- **Map**: Untuk akses cepat

#### **B. Cross-Platform Compatibility**
```cpp
#ifdef _WIN32
    // Windows implementation
#else
    // Linux implementation  
#endif
```

#### **C. Error Handling & Validation**
- Validasi input pengguna
- Handling file I/O errors
- Memory management yang aman

### **6. CONTOH PENGGUNAAN**

**Menambah Tugas:**
```
Nama Tugas: Tugas Struktur Data
Mata Kuliah: Algoritma
Kategori: Tugas
Tipe: Individu  
Deadline: 25/12/2024 23:59
```

**Output:**
- Tersimpan di DLL, BST, dan Queue (jika urgent)
- Dapat di-undo/redo
- Akan dapat notifikasi otomatis

### **7. KEAMANAN DAN STABILITAS**

- **Memory Management**: Cleanup di akhir program
- **Data Integrity**: Validasi input dan handling error
- **Performance**: Akses cepat dengan indexing
- **Reliability**: Auto-save dan backup

Program ini merupakan implementasi komprehensif dari berbagai struktur data untuk menyelesaikan masalah manajemen tugas akademik dengan antarmuka yang user-friendly dan fitur yang lengkap.

## Gambaran Penjelasan Program 

# PENJELASAN SUPER DETAIL PROGRAM CHRONOMIND - ACADEMIC TASK SYSTEM

## **1. PENDAHULUAN DAN OVERVIEW PROGRAM**

### **A. Deskripsi Program**
```cpp
// ChronoMind adalah sistem manajemen tugas akademik yang canggih
// Menggunakan multiple struktur data untuk efisiensi maksimal
// Fitur: notifikasi otomatis, undo/redo, hierarki akademik, dll.
```

### **B. Target Pengguna**
- **Mahasiswa** yang perlu mengelola banyak tugas kuliah
- **Pengguna** yang butuh reminder deadline otomatis
- **Pembelajar** yang ingin melihat progress akademik secara visual

### **C. Keunggulan Sistem**
- ✅ **Multi-platform** (Windows & Linux)
- ✅ **Real-time monitoring** dengan threading
- ✅ **Backup data** otomatis ke file
- ✅ **Antarmuka user-friendly** dengan menu interaktif
- ✅ **Visualisasi data** dengan progress bar dan statistik

## **2. STRUKTUR DATA DETAIL (PER COMPONENT)**

### **A. STRUCT TASK - Model Data Utama**
```cpp
struct Task {
    int id;                    // ID unik, auto-increment
    string name;              // Contoh: "Tugas Array Dynamic"
    string course;            // Contoh: "Struktur Data"
    string category;          // Contoh: "Tugas", "Quiz", "Ujian"
    string type;              // Contoh: "Individu", "Kelompok"
    time_t deadline;          // Format UNIX timestamp
    bool completed;           // false = pending, true = selesai
    time_t createdAt;         // Waktu tugas dibuat di system
    time_t completedAt;       // Waktu ditandai selesai (0 jika belum)
    time_t lastNotified;      // Mencegah notifikasi spam
    // Sistem notifikasi bertahap:
    bool notified3Hours;      // Sudah notif 3 jam sebelumnya?
    bool notified1Hour;       // Sudah notif 1 jam sebelumnya?
    bool notified30Min;       // Sudah notif 30 menit sebelumnya?
    bool notified5Min;        // Sudah notif 5 menit sebelumnya?
    bool notifiedOverdue;     // Sudah notif deadline lewat?
};
```

**ALASAN DESIGN:**
- `time_t` digunakan karena kompatibel dengan fungsi waktu C++
- `bool` flags untuk mencegah notifikasi berulang
- `createdAt` dan `completedAt` untuk tracking progress

### **B. DOUBLE LINKED LIST - Penyimpanan Linear**
```cpp
struct DLLNode {
    Task data;           // Data tugas yang disimpan
    DLLNode* prev;       // Pointer ke node sebelumnya
    DLLNode* next;       // Pointer ke node berikutnya
};
```

**OPERASI YANG DILAKUKAN:**
```cpp
// 1. CREATE: Membuat node baru
DLLNode* createDLLNode(Task t) {
    DLLNode* node = new DLLNode;  // Alokasi memory
    node->data = t;               // Copy data
    node->prev = NULL;            // Inisialisasi pointer
    node->next = NULL;
    return node;
}

// 2. ADD: Menambah node di akhir
void addTaskToDLL(Task newTask) {
    DLLNode* newNode = createDLLNode(newTask);
    
    if (g_head == NULL) {
        // Jika list kosong, jadi head dan tail
        g_head = g_tail = newNode;
    } else {
        // Jika ada data, tambah di akhir
        g_tail->next = newNode;
        newNode->prev = g_tail;
        g_tail = newNode;
    }
    
    // Simpan ke index untuk akses cepat
    g_taskIndex[newTask.id] = newNode;
}

// 3. DELETE: Menghapus node berdasarkan ID
void deleteTaskFromDLL(int id) {
    // Cari node di index
    DLLNode* node = g_taskIndex[id];
    
    // Update pointer node sebelumnya
    if (node->prev) {
        node->prev->next = node->next;
    } else {
        g_head = node->next;  // Jika hapus head
    }
    
    // Update pointer node setelahnya  
    if (node->next) {
        node->next->prev = node->prev;
    } else {
        g_tail = node->prev;  // Jika hapus tail
    }
    
    // Hapus dari memory
    delete node;
    g_taskIndex.erase(id);  // Hapus dari index
}
```

**KEUNTUNGAN DLL:**
- ✅ Bisa travers dua arah (maju/mundur)
- ✅ Insert/delete di posisi manapun O(1) jika tahu lokasi
- ✅ Tidak perlu shifting seperti array

### **C. BINARY SEARCH TREE - Hierarki Akademik**
```cpp
struct BSTNode {
    string key;              // Format: "MataKuliah_Kategori_Tipe"
    vector<int> taskIds;     // Kumpulan ID tugas dengan key sama
    BSTNode* left;           // Subtree kiri (key lebih kecil)
    BSTNode* right;          // Subtree kanan (key lebih besar)
};
```

**CONTOH STRUCTURE BST:**
```
                    "Algoritma_Tugas_Individu"
                   /                         \
  "AI_Quiz_Individu"                         "StrukturData_Proyek_Kelompok"
  /             \                           /                             \
NULL           "AI_Ujian_Individu"   "StrukturData_Tugas_Individu"   "Web_Proyek_Kelompok"
```

**OPERASI BST:**
```cpp
// 1. INSERT: Menambah node BST
void insertBST(BSTNode*& node, string key, int taskId) {
    if (node == NULL) {
        // Buat node baru jika kosong
        node = createBSTNode(key);
        node->taskIds.push_back(taskId);
    } 
    else if (key < node->key) {
        // Jika key lebih kecil, ke kiri
        insertBST(node->left, key, taskId);
    }
    else if (key > node->key) {
        // Jika key lebih besar, ke kanan  
        insertBST(node->right, key, taskId);
    }
    else {
        // Key sama, tambahkan taskId ke vector
        node->taskIds.push_back(taskId);
    }
}

// 2. TRAVERSAL: In-order traversal (sorted)
void displayBSTInOrder(BSTNode* node) {
    if (node == NULL) return;
    
    displayBSTInOrder(node->left);    // Kunjungi kiri
    
    // Process current node
    if (!node->taskIds.empty()) {
        cout << "Kategori: " << node->key << endl;
        for (int id : node->taskIds) {
            // Tampilkan detail tugas
        }
    }
    
    displayBSTInOrder(node->right);   // Kunjungi kanan
}
```

**KEUNTUNGAN BST:**
- ✅ Pencarian cepat O(log n) jika balanced
- ✅ Data terurut secara otomatis
- ✅ Efisien untuk range queries

### **D. QUEUE - Sistem Prioritas**
```cpp
struct QueueNode {
    int taskId;           // ID tugas yang urgent
    QueueNode* next;      // Pointer ke node berikutnya
};

// Global variables untuk queue
QueueNode* g_queueFront = NULL;  // Node paling depan
QueueNode* g_queueRear = NULL;   // Node paling belakang
```

**OPERASI QUEUE:**
```cpp
// 1. ENQUEUE: Masukkan ke belakang
void enqueue(int taskId) {
    QueueNode* newNode = new QueueNode;
    newNode->taskId = taskId;
    newNode->next = NULL;

    if (g_queueRear == NULL) {
        // Queue kosong
        g_queueFront = g_queueRear = newNode;
    } else {
        // Tambah di belakang
        g_queueRear->next = newNode;
        g_queueRear = newNode;
    }
}

// 2. DEQUEUE: Keluarkan dari depan  
int dequeue() {
    if (g_queueFront == NULL) return -1;  // Queue kosong
    
    QueueNode* temp = g_queueFront;
    int taskId = temp->taskId;
    
    g_queueFront = g_queueFront->next;
    if (g_queueFront == NULL) {
        g_queueRear = NULL;  // Queue sekarang kosong
    }
    
    delete temp;
    return taskId;
}
```

**KRITERIA MASUK QUEUE:**
- Deadline dalam 7 hari ke depan
- Status belum selesai
- Otomatis di-enqueue saat tambah/edit tugas

### **E. STACK - Sistem Undo/Redo**
```cpp
struct StackNode {
    string operation;      // "ADD", "DELETE", "EDIT", "COMPLETE"  
    Task taskSnapshot;     // Salinan data sebelum/sesudah operasi
    StackNode* next;       // Pointer ke node berikutnya
};

// Global stacks
StackNode* g_undoStack = NULL;  // Stack untuk undo
StackNode* g_redoStack = NULL;  // Stack untuk redo (NEW)
int g_undoStackSize = 0;
int g_redoStackSize = 0;
const int MAX_UNDO = 10;   // Batas maksimal undo
const int MAX_REDO = 10;   // Batas maksimal redo
```

**OPERASI STACK:**
```cpp
// 1. PUSH UNDO: Simpan operasi untuk undo
void pushUndo(string operation, Task task) {
    StackNode* newNode = new StackNode;
    newNode->operation = operation;
    newNode->taskSnapshot = task;  // Simpan salinan data
    newNode->next = g_undoStack;   // Push ke top
    g_undoStack = newNode;
    g_undoStackSize++;

    // Maintain size limit
    if (g_undoStackSize > MAX_UNDO) {
        // Hapus operasi paling lama (bottom of stack)
        StackNode* temp = g_undoStack;
        for (int i = 0; i < MAX_UNDO - 1; i++) {
            temp = temp->next;
        }
        delete temp->next;
        temp->next = NULL;
        g_undoStackSize = MAX_UNDO;
    }

    // Clear redo stack ketika ada operasi baru
    while (g_redoStack != NULL) {
        StackNode* temp = g_redoStack;
        g_redoStack = g_redoStack->next;
        delete temp;
    }
    g_redoStackSize = 0;
}

// 2. PERFORM UNDO: Batalkan operasi terakhir
void performUndo() {
    if (g_undoStack == NULL) return;
    
    StackNode* temp = g_undoStack;
    string op = temp->operation;
    Task task = temp->taskSnapshot;

    // Logic undo berdasarkan jenis operasi
    if (op == "ADD") {
        // Undo ADD = DELETE task yang ditambahkan
        if (g_taskIndex.find(task.id) != g_taskIndex.end()) {
            pushRedo("ADD", g_taskIndex[task.id]->data);
            deleteTaskFromDLL(task.id);
        }
    }
    else if (op == "DELETE") {
        // Undo DELETE = ADD task yang dihapus  
        addTaskToDLL(task);
        string bstKey = task.course + "_" + task.category + "_" + task.type;
        insertBST(g_bstRoot, bstKey, task.id);
        pushRedo("DELETE", task);
    }
    else if (op == "EDIT" || op == "COMPLETE") {
        // Undo EDIT/COMPLETE = restore data lama
        if (g_taskIndex.find(task.id) != g_taskIndex.end()) {
            pushRedo(op, g_taskIndex[task.id]->data);
            g_taskIndex[task.id]->data = task;  // Restore snapshot
        }
    }

    // Pop dari undo stack
    g_undoStack = g_undoStack->next;
    delete temp;
    g_undoStackSize--;
    
    saveToFile();  // Simpan perubahan
}
```

## **3. SISTEM NOTIFIKASI DETAIL**

### **A. Background Monitoring Thread**
```cpp
#ifdef _WIN32
void monitorThreadFunction(void* param) {
#else  
void* monitorThreadFunction(void* param) {
#endif
    while (g_monitoringActive) {
        checkSmartNotifications();  // Cek notifikasi
        Sleep(30000);  // Tunggu 30 detik (Windows)
        // sleep(30);  // Tunggu 30 detik (Linux)
    }
#ifndef _WIN32
    return NULL;
#endif
}
```

**IMPLEMENTASI CROSS-PLATFORM:**
```cpp
void startBackgroundMonitor() {
    g_monitoringActive = true;
#ifdef _WIN32
    // Windows thread
    g_monitorThread = (HANDLE)_beginthread(monitorThreadFunction, 0, NULL);
#else
    // Linux pthread  
    pthread_create(&g_monitorThread, NULL, monitorThreadFunction, NULL);
#endif
}
```

### **B. Smart Notification Logic**
```cpp
void checkSmartNotifications() {
    if (g_head == NULL) return;

    time_t now = time(NULL);
    DLLNode* current = g_head;
    bool notificationSent = false;

    while (current != NULL) {
        if (!current->data.completed) {
            double secondsLeft = difftime(current->data.deadline, now);
            double minutesLeft = secondsLeft / 60.0;

            Task& t = current->data;

            // Cooldown 15 detik antara notifikasi untuk task sama
            bool canNotify = (now - t.lastNotified) >= 15;

            // Logic notifikasi bertahap
            if (minutesLeft <= 180 && minutesLeft > 60 && 
                !t.notified3Hours && canNotify) {
                sendTimeBasedNotification(t, "3 JAM", minutesLeft);
                t.notified3Hours = true;
                t.lastNotified = now;
                notificationSent = true;
            }
            // ... kondisi notifikasi lainnya
        }
        current = current->next;
    }

    // Jika ada notifikasi dikirim, cooldown lebih lama
    if (notificationSent) {
#ifdef _WIN32
        Sleep(15000);  // 15 detik
#else
        sleep(15);
#endif
    }
}
```

### **C. Popup Notification (Windows)**
```cpp
void sendNotificationPopup(string title, string message, bool isUrgent) {
    // Tampilkan di console
    cout << "\n========================================" << endl;
    cout << "   NOTIFIKASI: " << title << endl;
    cout << "========================================" << endl;
    cout << message << endl;
    cout << "========================================" << endl;

#ifdef _WIN32
    // Popup window di Windows
    string fullMessage = message;
    UINT iconType = isUrgent ? MB_ICONWARNING : MB_ICONERROR;

    // MessageBox dengan priority tinggi
    HWND hwnd = GetForegroundWindow();
    MessageBoxA(hwnd, fullMessage.c_str(), title.c_str(),
                MB_OK | iconType | MB_TOPMOST | MB_SETFOREGROUND | MB_SYSTEMMODAL);
#endif

    // Jeda singkat tanpa blocking
#ifdef _WIN32
    Sleep(1000);
#else
    sleep(1);
#endif
}
```

## **4. SISTEM FILE DAN PERSISTENSI DATA**

### **A. Format File Penyimpanan**
```
chronomind_data.txt

Format:
[taskCounter]
[id]|[name]|[course]|[category]|[type]|[deadline]|[completed]|[createdAt]|[completedAt]|[lastNotified]|[notified3Hours]|[notified1Hour]|[notified30Min]|[notified5Min]|[notifiedOverdue]
```

**CONTOH DATA:**
```
3
1|Tugas Array|Struktur Data|Tugas|Individu|1735151940|0|1735065540|0|0|0|0|0|0|0
2|Quiz Sorting|Algoritma|Quiz|Individu|1735238340|1|1735065540|1735151940|0|1|1|1|1|0
```

### **B. Load Data dari File**
```cpp
void loadFromFile() {
    ifstream file("chronomind_data.txt");
    if (!file) return;  // File tidak ada, skip

    file >> g_taskCounter;  // Baca counter
    file.ignore();  // Ignore newline

    string line;
    while (getline(file, line)) {
        if (line.empty()) continue;

        stringstream ss(line);
        Task t;
        char delimiter;

        // Parse semua field
        ss >> t.id >> delimiter;
        getline(ss, t.name, '|');
        getline(ss, t.course, '|');
        // ... parsing lainnya

        // Konversi timestamp
        long long deadlineLL;
        ss >> deadlineLL >> delimiter;
        t.deadline = (time_t)deadlineLL;

        // Tambahkan ke semua struktur data
        addTaskToDLL(t);
        
        string bstKey = t.course + "_" + t.category + "_" + t.type;
        insertBST(g_bstRoot, bstKey, t.id);

        // Cek jika perlu masuk priority queue
        time_t now = time(NULL);
        double daysUntil = difftime(t.deadline, now) / (60 * 60 * 24);
        if (!t.completed && daysUntil <= 7 && daysUntil >= 0) {
            enqueue(t.id);
        }

        if (t.completed) g_totalCompleted++;
    }

    file.close();
}
```

## **5. ALUR PROGRAM UTAMA (MAIN FUNCTION)**

### **A. Inisialisasi Program**
```cpp
int main() {
    // 1. LOAD DATA SEBELUMNYA
    loadFromFile();
    
    // 2. START BACKGROUND MONITORING
    startBackgroundMonitor();

    // 3. TAMPILKAN WELCOME MESSAGE
    cout << "\n>> Selamat datang di ChronoMind!" << endl;
    cout << ">> Background monitoring aktif (check setiap 30 detik)" << endl;
    waitForEnter();

    // 4. MAIN PROGRAM LOOP
    int choice;
    do {
        displayMenu();      // Tampilkan menu
        choice = getValidChoice();  // Baca input user

        // Process pilihan user
        switch (choice) {
            case 1: handleAddTask(); break;
            case 2: displayAllTasks(); break;
            // ... cases lainnya
            case 0: 
                // EXIT PROGRAM
                cout << "\n>> Menghentikan background monitor..." << endl;
                stopBackgroundMonitor();
                cout << ">> Menyimpan data..." << endl;
                saveToFile();
                cout << ">> Data berhasil disimpan!" << endl;
                cout << "\n>> Terima kasih telah menggunakan ChronoMind." << endl;
                break;
            default:
                cout << "\n>> ERROR: Pilihan tidak valid!" << endl;
                waitForEnter();
        }

    } while (choice != 0);

    // 5. CLEANUP RESOURCES
    cleanup();
    return 0;
}
```

### **B. Menu System**
```
==========================================================
           CHRONOMIND - ACADEMIC TASK SYSTEM            
      Adaptive Academic Task Intelligence System        
==========================================================

MENU UTAMA:
  1. Tambah Tugas Baru
  2. Lihat Semua Tugas (Double Linked List)
  3. Lihat Hierarki Akademik (BST)
  4. Lihat Priority Queue (Urgent Tasks)
  5. Tandai Tugas Selesai
  6. Edit Tugas
  7. Hapus Tugas Berdasarkan ID
  8. Hapus SEMUA Tugas
  9. Undo Operasi Terakhir
 10. Redo Operasi Terakhir
 11. Lihat Statistik & Analytics
 12. Timeline Mingguan
 13. Cek Notifikasi Manual
 14. Cari Tugas
 15. Clear Terminal
  16. Keluar & Simpan
```

## **6. FITUR VISUALISASI DAN STATISTIK**

### **A. Progress Bar System**
```cpp
void displayProgressBar(Task& t) {
    time_t now = time(NULL);
    double total = difftime(t.deadline, t.createdAt);
    double elapsed = difftime(now, t.createdAt);
    double progress = (elapsed / total) * 100.0;

    // Clamping values
    if (progress > 100) progress = 100;
    if (progress < 0) progress = 0;

    int filled = (int)(progress / 5);  // 20 karakter = 5% per karakter

    cout << "   Progress: [";
    for (int i = 0; i < 20; i++) {
        if (i < filled) cout << "#";  // Bagian yang sudah lewat
        else cout << "-";             // Bagian sisa
    }
    cout << "] " << (int)progress << "% waktu berlalu" << endl;
}
```

### **B. Statistik Komprehensif**
```cpp
void displayStatistics() {
    // Hitung berbagai metrics
    int total = 0, pending = 0, completed = 0, overdue = 0;
    map<string, int> courseStats;  // Statistik per mata kuliah

    // Iterasi melalui semua tugas
    DLLNode* current = g_head;
    time_t now = time(NULL);

    while (current != NULL) {
        total++;
        Task& t = current->data;
        courseStats[t.course]++;  // Hitung per mata kuliah

        if (t.completed) {
            completed++;
        } else {
            pending++;
            double minutesLeft = difftime(t.deadline, now) / 60.0;
            if (minutesLeft < 0) overdue++;
        }
        current = current->next;
    }

    // Tampilkan ringkasan
    cout << "\nRINGKASAN TUGAS:" << endl;
    cout << "   Total Tugas      : " << total << endl;
    cout << "   [SELESAI]        : " << completed << endl;
    cout << "   [PENDING]        : " << pending << endl;
    cout << "   [TERLAMBAT]      : " << overdue << endl;

    // Hitung completion rate
    if (total > 0) {
        double completionRate = ((double)completed / total) * 100.0;
        cout << "\n   Tingkat Penyelesaian: " << completionRate << "%" << endl;
        
        // Progress bar overall
        cout << "   ";
        int filled = (int)(completionRate / 5);
        for (int i = 0; i < 20; i++) {
            if (i < filled) cout << "#";
            else cout << "-";
        }
        cout << " " << (int)completionRate << "%" << endl;
    }
}
```

## **7. ERROR HANDLING DAN VALIDATION**

### **A. Input Validation**
```cpp
int getValidChoice() {
    int choice;
    while (true) {
        if (cin >> choice) {
            clearInputBuffer();
            return choice;
        } else {
            cout << "\n>> ERROR: Input harus berupa angka!" << endl;
            cout << ">> Silakan masukkan pilihan menu: ";
            clearInputBuffer();
        }
    }
}

void clearInputBuffer() {
    cin.clear();
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
}
```

### **B. Data Validation**
```cpp
void addTask(string name, string course, string category, string type, time_t deadline) {
    // Validasi input tidak kosong
    name = trim(name);
    course = trim(course);
    category = trim(category);
    type = trim(type);

    if (name.empty() || course.empty() || category.empty() || type.empty()) {
        cout << "\n>> ERROR: Semua field harus diisi! Tidak boleh kosong." << endl;
        waitForEnter();
        return;
    }

    if (deadline <= 0) {
        cout << "\n>> ERROR: Format tanggal tidak valid!" << endl;
        waitForEnter();
        return;
    }

    // ... lanjut proses tambah task
}
```

## **8. MEMORY MANAGEMENT DAN CLEANUP**

### **A. Comprehensive Cleanup**
```cpp
void cleanup() {
    // 1. Stop background thread
    stopBackgroundMonitor();
    
    // 2. Free DLL memory
    DLLNode* current = g_head;
    while (current != NULL) {
        DLLNode* next = current->next;
        delete current;
        current = next;
    }

    // 3. Free BST memory
    clearBST(g_bstRoot);
    
    // 4. Free Queue memory  
    clearQueue();

    // 5. Free Undo/Redo stacks
    while (g_undoStack != NULL) {
        StackNode* temp = g_undoStack;
        g_undoStack = g_undoStack->next;
        delete temp;
    }

    while (g_redoStack != NULL) {
        StackNode* temp = g_redoStack;
        g_redoStack = g_redoStack->next;
        delete temp;
    }
}
```

### **B. Recursive BST Cleanup**
```cpp
void clearBST(BSTNode* node) {
    if (node == NULL) return;
    clearBST(node->left);   // Free left subtree
    clearBST(node->right);  // Free right subtree
    delete node;            // Free current node
}
```

## **9. KEUNGGULAN ARSITEKTUR PROGRAM**

### **A. Separation of Concerns**
- **Data Structure**: DLL, BST, Queue, Stack terpisah
- **Business Logic**: Operasi tugas terisolasi
- **UI Layer**: Menu dan display terpisah
- **Persistence**: File I/O terisolasi

### **B. Scalability**
- **Modular Design**: Mudah tambah fitur baru
- **Memory Efficient**: Dynamic allocation
- **Performance**: Akses cepat dengan multiple structures

### **C. Maintainability**
- **Clear Structure**: Code terorganisir rapi
- **Comments**: Dokumentasi lengkap
- **Error Handling**: Robust terhadap input invalid

## **10. CONTOH USE CASE KOMPLIT**

### **Scenario: Mahasiswa Semester 4**
```
1. Tambah tugas:
   - "Tugas BST Implementation" untuk "Struktur Data"
   - Deadline: 25/12/2024 23:59

2. Program akan:
   - Simpan di DLL sebagai node baru
   - Masukkan ke BST dengan key "StrukturData_Tugas_Individu" 
   - Cek deadline, jika <7 hari masuk priority queue
   - Simpan snapshot di undo stack
   - Save ke file

3. Notifikasi otomatis:
   - 3 jam sebelum: "REMINDER! Deadline tinggal 3 jam!"
   - 1 jam sebelum: "PERINGATAN! Deadline tinggal 1 jam!"
   - 30 menit sebelum: "PERINGATAN! Deadline tinggal 30 menit!"
   - 5 menit sebelum: "URGENT! Deadline tinggal 5 menit!"
   - Setelah lewat: "OVERDUE! Deadline sudah terlewat!"

4. User bisa:
   - Lihat di menu 2 (DLL view)
   - Lihat hierarki di menu 3 (BST view) 
   - Lihat prioritas di menu 4 (Queue view)
   - Undo jika salah input
   - Lihat statistik progress
```

