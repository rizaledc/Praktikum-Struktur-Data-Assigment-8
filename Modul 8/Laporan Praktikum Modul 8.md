# <h1 align="center">Laporan Praktikum Modul Stack dan Queue</h1>

<p align="center">Rizal Wahyu Pratama</p>
<p align="center">2311110029</p>
<p align="center">S1SD-04-B</p>

## Tujuan Praktikum

 a. Mahasiswa mampu menjelaskan definisi dan konsep dari priority queue dan heaps
 
 b. Mahasiswa mampu menerapkan operasi-operasi pada heaps
 
 c. Mahasiswa mampu mengimplementasikan konsep priority queue dan heaps pada
 contoh kasus nyata

## Dasar Teori

Jenis antrian yang dapat mengatur elemen berdasarkan nilai prioritasnya dapat disebut dengan Priority Queue dalam bahasa pemrograman. Antrean prioritas (juga dikenal sebagai antrean min/maks) lebih populer untuk implementasi dalam perangkat lunak. Meskipun implementasi perangkat lunak sering untuk banyak aplikasi, dalam beberapa kasus perlu menyortir/mengantri lebih banyak data dengan lebih cepat menilai atau menjamin waktu respons yang konstan [2]. Sistemnya sangatlah mudah, memanfaatkan elemen dengan nilai prioritas tinggi yang diambil pertama sebelum elemen dengan prioritas rendah. Dimana ketika ditambahkan suatu elemen ke dalam antrian dengan nilainya, maka elemen akan memasukkan nilai berdasarkan nilai prioritasnya.

Priority Queue berdasarkan aturan elemen yang prioritasnya lebih tinggi, diproses lebih dahulu dibandingkan dengan elemen yang prioritasnya lebih rendah dan dua elemen dengan prioritas yang sama, diproses sesuai dengan urutan mereka sewaktu dimasukkan ke dalam priority queue [1]. Berikut ilustrasi Priority Queue:

<p align="center">
  <img src="https://github.com/rizaledc/Praktikum-Struktur-Data-Assigment-Modul-8/blob/main/Modul%208/SS/Screenshot%202024-05-22%20023832.png" alt="Alt Text"> Gambar 1. Ilustrasi Priority Queue
</p> 

Pada gambar di atas terdapat angka 1-9 yang dipilih secara acak. Dimana angka 9 akan di dequeue sementara angka 1 akan di enqueue. Angka 9 di dequeue karena angka 9 merupakan angka dengan nilai tertinggi sehingga memiliki nilai prioritas yang tinggi. Terdapat berbagai macam cara untuk implementasi priority queue ini, dimana terdapat dalam penggunaan array, linked list, heap, serta binary search tree. 

Terdapat dua jenis heap, diantaranya Min heap (nilai terkecil menjadi prioritas) serta Max heap (nilai terbesar menjadi prioritas), berikut ilustrasinya:

<p align="center">
  <img src="https://github.com/rizaledc/Praktikum-Struktur-Data-Assigment-Modul-8/blob/main/Modul%208/SS/Screenshot%202024-05-22%20023853.png" alt="Alt Text">2. Ilustrasi dua jenis priority queue
</p> 

Heap di dalam struktur data merupakan struktur berbasis binary tree dengan aturannya. Dimana min heap pada gambar di atas, nilai parentsnya harus lebih kecil atau sama dengan dibandingkan nilai childnya pada contoh di atas angka 2 sebagai parent akan lebih kecil dibandingkan nilai childnya. Pada max heap di atas, nilai parentsnya harus lebih besar atau sama dengan dibandingkan nilai childnya, contohnya di atas angka 25 menjadi parents akan lebih besar jika dibandingkan dengan childnya.

Dalam hal kecepatan, heap sangatlah cepat. Dimana dalam menambah dan menghapus elemen hanya dibutuhkan waktu O(log N), untuk mencari elemen dibutuhkan waktu konstan O(1). Pada Guided 1 dan Unguided 1 kita akan memberikan contoh serta penjelasan dari kode heap yang dibuat.

## Guided 

### 1. Guided 1

```C++
#include <iostream>
#include <algorithm>

int H[50]; // Array untuk menyimpan elemen heap
int heapSize =-1; // Ukuran heap saat ini

// Fungsi untuk mendapatkan indeks parent dari node saat ini
int parent(int i) {
        return (i-1) / 2;
}

// Fungsi untuk mendapatkan indeks anak kiri dari node saat ini
int leftChild(int i) {
        return ((2 * i) + 1);
}

// Fungsi untuk mendapatkan indeks anak kanan dari node saat ini
int rightChild(int i) {
        return ((2 * i) + 2);
}

// Fungsi untuk memperbaiki posisi node ke atas heap jika diperlukan
void shiftUp(int i) {
        while (i > 0 && H[parent(i)] < H[i]) {
            std::swap(H[parent(i)], H[i]);
            i = parent(i);
        }
}

// Fungsi untuk memperbaiki posisi node ke bawah heap jika diperlukan
void shiftDown(int i) {
    int maxIndex = i;
    int l = leftChild(i);
    if (l <= heapSize && H[l] > H[maxIndex]) {
        maxIndex = l;
    }

    int r = rightChild(i);
    if (r <= heapSize && H[r] > H[maxIndex]) {
        maxIndex = r;
    }
    if (i != maxIndex) {
        std::swap(H[i], H[maxIndex]);
        shiftDown(maxIndex);
    }
}

// Fungsi untuk memasukkan elemen baru ke dalam heap
void insert(int p) {
        heapSize = heapSize + 1;
        H[heapSize] = p;
        shiftUp(heapSize);
}

// Fungsi untuk mengambil elemen maksimum dari heap dan memperbaiki struktur heap
int extractMax() {
    int result = H[0];
    H[0] = H[heapSize];
    heapSize = heapSize - 1;
    shiftDown(0);
    return result;
}

// Fungsi untuk mengubah prioritas elemen di heap dan memperbaiki struktur heap
void changePriority(int i, int p) {
    int oldp = H[i];
    H[i] = p;
    if (p > oldp) {
        shiftUp(i);
    } else {
        shiftDown(i);
    }
}

// Fungsi untuk mendapatkan elemen maksimum dari heap tanpa menghapusnya
int getMax() {
    return H[0];
}

// Fungsi untuk menghapus elemen dari heap
void remove(int i) {
    H[i] = getMax() + 1;
    shiftUp(i);
    extractMax();
}

// Fungsi utama untuk menjalankan operasi heap
int main() {
    insert(45);
    insert(20);
    insert(14);
    insert(12);
    insert(31);
    insert(7);
    insert(11);
    insert(13);
    insert(7);
    std::cout << "Priority Queue : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";
    std::cout << "Node with maximum priority : " << extractMax() << "\n";
    std::cout << "Priority queue after extracting maximum : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";
    changePriority(2, 49);
    std::cout << "Priority queue after priority change : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";
    remove(3);
    std::cout << "Priority queue after removing the element : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    return 0;
}
```

**Penjelasan:**

#### Bagian 1

```C++
#include <iostream>
#include <algorithm>

int H[50];         // Array untuk menyimpan elemen heap
int heapSize = -1; // Ukuran heap saat ini
```

include iosteam digunakan agar dapat membuat operasi input dan output pada program C++, include <algorithm> digunakan untuk menyertakan pustaka algoritma standar, terutama untuk penggunaan fungsi std::swap.

int H[50] digunakan untuk mendeklarasikan array H dengan ukuran 50 untuk menyimpan heap. Inisiasi heap -1 yang menunjukkan heap kosong.

#### Bagian 2

```C++
int parent(int i) {
    return (i - 1) / 2;
}

int leftChild(int i) {
    return (2 * i) + 1;
}

int rightChild(int i) {
    return (2 * i) + 2;
}
```

3 kode di atas digunakan untuk mengembalikan indeks parent, mengembalikan indeks kiri, dan mengembalikan indeks kanan dengan indeks i.

#### Bagian 3

```C++
void shiftUp(int i) {
    while (i > 0 && H[parent(i)] < H[i]) {
        std::swap(H[parent(i)], H[i]);
        i = parent(i);
    }
}
```

Kode di atas merupakan fungsi shiftUp dimana selama node i lebih besar dari parentnya, maka node tersebut akan ditukar dan memperbaharui i sebagai parentnya.

#### Bagian 4

```C++
void shiftDown(int i) {
    int maxIndex = i;
    int l = leftChild(i);
    if (l <= heapSize && H[l] > H[maxIndex]) {
        maxIndex = l;
    }

    int r = rightChild(i);
    if (r <= heapSize && H[r] > H[maxIndex]) {
        maxIndex = r;
    }
    if (i != maxIndex) {
        std::swap(H[i], H[maxIndex]);
        shiftDown(maxIndex);
    }
}
```

Kode di atas merupakan kode untuk fungsi shiftDown. Dimana ketika child kiri maupun kanan lebih besar dari pada node yang ada maka tukar node child yang lebih besar dan perbaharui indeks ke-i.

#### Bagian 5

```C++
void insert(int p) {
    heapSize = heapSize + 1;
    H[heapSize] = p;
    shiftUp(heapSize);
}

int extractMax() {
    int result = H[0];
    H[0] = H[heapSize];
    heapSize = heapSize - 1;
    shiftDown(0);
    return result;
}

void changePriority(int i, int p) {
    int oldp = H[i];
    H[i] = p;
    if (p > oldp) {
        shiftUp(i);
    } else {
        shiftDown(i);
    }
}
```

Pada kode di atas terdapat 3 jenis fungsi. Pada fungsi pertama (insert) menambahkan elemen p ke dalam heap. Berikutnya, elemen baru akan ditempatkan pada akhir posisi lalu digunakanlah fungsi shiftUp.

Pada fungsi kedua (extractMax) mengambil elemen terbesar dari heap. Lalu menukar elemen terbesar dengan elemen terakhir dengan cara mengurangi ukuran heap (-1) digunakan juga fungsi shiftDown.

Pada fungsi ketiga (changePriority) mengubah prioritas elemen di indeks i menjadi p. Jika prioritas baru lebih besar, memanggil shiftUp, jika lebih kecil, memanggil shiftDown.

#### Bagian 6

```C++
int getMax() {
    return H[0];
}

void remove(int i) {
    H[i] = getMax() + 1;
    shiftUp(i);
    extractMax();
}
```

getMax() Mengembalikan elemen maksimum dari heap tanpa menghapusnya. remove(int i) Menghapus elemen di indeks i.Mengatur elemen tersebut ke nilai lebih besar dari elemen maksimum, memanggil shiftUp untuk menempatkannya di root, kemudian memanggil extractMax untuk menghapusnya.

#### Bagian 7

```C++
int main() {
    insert(45);
    insert(20);
    insert(14);
    insert(12);
    insert(31);
    insert(7);
    insert(11);
    insert(13);
    insert(7);
    std::cout << "Priority Queue : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";
    std::cout << "Node with maximum priority : " << extractMax() << "\n";
    std::cout << "Priority queue after extracting maximum : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";
    changePriority(2, 49);
    std::cout << "Priority queue after priority change : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";
    remove(3);
    std::cout << "Priority queue after removing the element : ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    return 0;
}
```

Fungsi main atau fungsi utama ini merupakan fungsi yang pertama kali akan dieksekusi dalam program. Pada main terdapat berbagai fungsi yang telah dibuat sebelumnya sehingga pada main hanya tinggal mengatur saja posisi fungsi yang telah di buat. Terdapat beberapa operasi pada heap sebagai berikut:

- insert: Menambahkan elemen ke heap.
- Mencetak antrian prioritas saat ini.
- extractMax: Mengambil dan mencetak elemen maksimum dari heap.
- changePriority: Mengubah prioritas elemen di indeks tertentu.
- remove: Menghapus elemen di indeks tertentu.
- Menampilkan heap setelah setiap operasi untuk memastikan fungsionalitas dan validitas.

#### Full Code Screenshot

<p align="center">
  <img src="https://github.com/rizaledc/Praktikum-Struktur-Data-Assigment-Modul-8/blob/main/Modul%208/SS/Guided1.png" alt="Alt Text">
</p>

#### Screenshot Output

<p align="center">
  <img src="https://github.com/rizaledc/Praktikum-Struktur-Data-Assigment-Modul-8/blob/main/Modul%208/SS/OutputGuided.png" alt="Alt Text">
</p>

#### Penjelasan

Pada output di atas, semuanya telah di atur dalam fungsi main. Terdapat priority queue yaitu 45 31 14 13 20 7 11 12 7. Node maksimum dikeluarkan (45), lalu diganti dengan 49, prioritas queue diurutkan kembali menjadi 49 20 31 12 7 7 11.

## Unguided 

### 1. Unguided 1

#### Modifikasi guided diatas yang mana heap dikonstruksi secara manual berdasarkan user

**Kode Program:**

```C++
#include <iostream>
#include <algorithm>
#include <vector>

// Mendeklarasikan vector untuk menyimpan elemen heap
std::vector<int> H;
// Mendeklarasikan variabel untuk menyimpan ukuran heap saat ini
int heapSize = -1;

// Fungsi untuk mendapatkan indeks parent dari node saat ini
int parent(int i) {
    return (i - 1) / 2;
}

// Fungsi untuk mendapatkan indeks anak kiri dari node saat ini
int leftChild(int i) {
    return (2 * i) + 1;
}

// Fungsi untuk mendapatkan indeks anak kanan dari node saat ini
int rightChild(int i) {
    return (2 * i) + 2;
}

// Fungsi untuk memperbaiki posisi node ke atas heap jika diperlukan
void shiftUp(int i) {
    while (i > 0 && H[parent(i)] < H[i]) {
        std::swap(H[parent(i)], H[i]);
        i = parent(i);
    }
}

// Fungsi untuk memperbaiki posisi node ke bawah heap jika diperlukan
void shiftDown(int i) {
    int maxIndex = i;
    int l = leftChild(i);
    if (l <= heapSize && H[l] > H[maxIndex]) {
        maxIndex = l;
    }

    int r = rightChild(i);
    if (r <= heapSize && H[r] > H[maxIndex]) {
        maxIndex = r;
    }
    if (i != maxIndex) {
        std::swap(H[i], H[maxIndex]);
        shiftDown(maxIndex);
    }
}

// Fungsi untuk memasukkan elemen baru ke dalam heap
void insert(int p) {
    if (static_cast<size_t>(heapSize + 1) >= H.size()) {
        H.push_back(p); // Memastikan ada ruang di vector sebelum menambahkan elemen
    } else {
        H[heapSize + 1] = p;
    }
    heapSize++;
    shiftUp(heapSize);
}

// Fungsi untuk mengambil elemen maksimum dari heap dan memperbaiki struktur heap
int extractMax() {
    int result = H[0];
    H[0] = H[heapSize];
    heapSize--;
    shiftDown(0);
    return result;
}

// Fungsi untuk mengubah prioritas elemen di heap dan memperbaiki struktur heap
void changePriority(int i, int p) {
    int oldp = H[i];
    H[i] = p;
    if (p > oldp) {
        shiftUp(i);
    } else {
        shiftDown(i);
    }
}

// Fungsi untuk mendapatkan elemen maksimum dari heap tanpa menghapusnya
int getMax() {
    return H[0];
}

// Fungsi untuk menghapus elemen dari heap
void remove(int i) {
    H[i] = getMax() + 1;
    shiftUp(i);
    extractMax();
}

// Fungsi utama untuk menjalankan operasi heap
int main() {
    int numElements, element;
    std::cout << "Masukkan jumlah elemen: ";
    std::cin >> numElements;
    std::cout << "Masukkan elemen-elemen heap: ";
    for (int i = 0; i < numElements; ++i) {
        std::cin >> element;
        insert(element);
    }

    std::cout << "Priority Queue: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    std::cout << "Node with maximum priority: " << extractMax() << "\n";
    std::cout << "Priority queue after extracting maximum: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    int indexToChange, newPriority;
    std::cout << "Enter index to change priority and new priority: ";
    std::cin >> indexToChange >> newPriority;
    changePriority(indexToChange, newPriority);
    std::cout << "Priority queue after priority change: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    int indexToRemove;
    std::cout << "Enter index to remove: ";
    std::cin >> indexToRemove;
    remove(indexToRemove);
    std::cout << "Priority queue after removing the element: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    return 0;
}
```

**Penjelasan:**

#### Bagian 1

```C++
#include <iostream>
#include <algorithm>
#include <vector>

// Mendeklarasikan vector untuk menyimpan elemen heap
std::vector<int> H;
// Mendeklarasikan variabel untuk menyimpan ukuran heap saat ini
int heapSize = -1;
```

Library iostream digunakan untuk menjalankan operasi input dan output pada program. Library algorithm digunakan untuk menggunakan algoritma std::swap. Library vector digunakan untuk menyimpan heap nantinya.

std::vector<int> H digunakan mendeklarasikan vector H untuk menyimpan elemen heap. int heapSize = -1 menginisialisasi ukuran heap dengan -1 menunjukkan bahwa heap awalnya kosong.

#### Bagian 2

```C++
int parent(int i) {
    return (i - 1) / 2;
}

int leftChild(int i) {
    return (2 * i) + 1;
}

int rightChild(int i) {
    return (2 * i) + 2;
}
```

3 kode di atas digunakan untuk mengembalikan indeks parent, mengembalikan indeks kiri, dan mengembalikan indeks kanan dengan indeks i.

#### Bagian 3

```C++
void shiftUp(int i) {
    while (i > 0 && H[parent(i)] < H[i]) {
        std::swap(H[parent(i)], H[i]);
        i = parent(i);
    }
}

```

Kode di atas merupakan fungsi shiftUp dimana selama node i lebih besar dari parentnya, maka node tersebut akan ditukar dan memperbaharui i sebagai parentnya.

#### Bagian 4

```C++
void shiftDown(int i) {
    int maxIndex = i;
    int l = leftChild(i);
    if (l <= heapSize && H[l] > H[maxIndex]) {
        maxIndex = l;
    }

    int r = rightChild(i);
    if (r <= heapSize && H[r] > H[maxIndex]) {
        maxIndex = r;
    }
    if (i != maxIndex) {
        std::swap(H[i], H[maxIndex]);
        shiftDown(maxIndex);
    }
}
```

Kode di atas merupakan kode untuk fungsi shiftDown. Dimana ketika child kiri maupun kanan lebih besar dari pada node yang ada maka tukar node child yang lebih besar dan perbaharui indeks ke-i.

#### Bagian 5

```C++
void insert(int p) {
    if (static_cast<size_t>(heapSize + 1) >= H.size()) {
        H.push_back(p); // Memastikan ada ruang di vector sebelum menambahkan elemen
    } else {
        H[heapSize + 1] = p;
    }
    heapSize++;
    shiftUp(heapSize);
}
```

Kode di atas merupakan kode untuk membuat fungsi insert dimana telah ditambahkan elemen baru yaitu p ke dalam heap. Lalu vektor ukurannya di cek apakah sudah cukup atau belum. Jika belum cukup maka akan ditambahkan elemen push_back. Menambah heapSize dan menempatkan elemen baru pada posisi akhir, lalu memanggil shiftUp untuk memastikan heap tetap valid.

#### Bagian 6

```C++
int extractMax() {
    int result = H[0];
    H[0] = H[heapSize];
    heapSize--;
    shiftDown(0);
    return result;
}
```

Pada kode di atas akan menjalankan tugas dengan mengambil elemen maksimum dari heap. Menukar elemen maksimum dengan elemen terakhir dan mengurangi ukuran heap. Lalu memanggil shiftDown untuk memastikan heap tetap valid.

#### Bagian 7

```C++
void changePriority(int i, int p) {
    int oldp = H[i];
    H[i] = p;
    if (p > oldp) {
        shiftUp(i);
    } else {
        shiftDown(i);
    }
}
```

Pada kode di atas, elemen indeks i menjadi p. Dimana ketika prioritas baru lebih besar maka akan dipanggil shiftUp. Jika lebih kecil, maka memanggil shiftDown.

#### Bagian 8

```C++
int getMax() {
    return H[0];
}
void remove(int i) {
    H[i] = getMax() + 1;
    shiftUp(i);
    extractMax();
}
```

getMax() digunakan untuk mengembalikan elemen maksimum. remove(int i) digunakan untuk menghapus elemen indeks ke-i. Mengatur elemen tersebut ke nilai lebih besar dari elemen maksimum, memanggil shiftUp untuk menempatkannya di root, kemudian memanggil extractMax untuk menghapusnya.

#### Bagian 8

```C++
int main() {
    int numElements, element;
    std::cout << "Masukkan jumlah elemen: ";
    std::cin >> numElements;
    std::cout << "Masukkan elemen-elemen heap: ";
    for (int i = 0; i < numElements; ++i) {
        std::cin >> element;
        insert(element);
    }

    std::cout << "Priority Queue: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    std::cout << "Node with maximum priority: " << extractMax() << "\n";
    std::cout << "Priority queue after extracting maximum: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    int indexToChange, newPriority;
    std::cout << "Enter index to change priority and new priority: ";
    std::cin >> indexToChange >> newPriority;
    changePriority(indexToChange, newPriority);
    std::cout << "Priority queue after priority change: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    int indexToRemove;
    std::cout << "Enter index to remove: ";
    std::cin >> indexToRemove;
    remove(indexToRemove);
    std::cout << "Priority queue after removing the element: ";
    for (int i = 0; i <= heapSize; ++i) {
        std::cout << H[i] << " ";
    }
    std::cout << "\n";

    return 0;
}
```
Fungsi di atas merupakan fungsi main yang berupa fungsi utama di dalam program yang disebut dengan fungsi main. Pengguna akan diminta untuk memasukkan jumlah elemen yang akan dimasukkan ke dalam heap. Berikutnya terdapat beberapa operasi yang berjalan di heap sebagai berikut:

- insert: Menambahkan elemen-elemen ke heap. --> Mencetak antrian prioritas saat ini.
- extractMax: Mengambil dan mencetak elemen maksimum dari heap. --> Mencetak antrian prioritas setelah mengeluarkan elemen maksimum.
- changePriority: Mengubah prioritas elemen di indeks tertentu. --> Mencetak antrian prioritas setelah mengubah prioritas elemen.
- remove: Menghapus elemen di indeks tertentu. --> Mencetak antrian prioritas setelah menghapus elemen.
- 
#### Full code Screenshot:

<p align="center">
  <img src="https://github.com/rizaledc/Praktikum-Struktur-Data-Assigment-Modul-8/blob/main/Modul%208/SS/Unguided1.png" alt="Alt Text">
</p>

#### Screenshot Output

<p align="center">
  <img src="https://github.com/rizaledc/Praktikum-Struktur-Data-Assigment-Modul-8/blob/main/Modul%208/SS/OutputUnguided.png" alt="Alt Text">
</p>

#### Penjelasan

Dari output di atas, ketika pengguna diminta memasukkan jumlah elemen, dimasukkan jumlah elemen sebanyak 5 dengan elemennya : 1 2 3 4 5. Berikutnya prioritasnya diurutkan. Dimana nilai maksimum (5) akan dikeluarkan. Pengguna memasukkan prioritas baru di indeks ke 2 yaitu 6. Maka perubahan prioritas menjadi 6 3 4 1. Lalu pada indeks ke-1 elemennya di remove. Akhir elemen menjadi 6 1 4.

## Kesimpulan

Priority queue merupakan antrian dengan elemen-elemen diurutkan sesuai prioritas berdasarkan nilainya. Elemen nilai tertinggi akan diambil terlebih dahulu dibandingkan nilai yang lebih rendah. Priority queue digunakan dalam sistem real-time dalam algoritma untuk meningkatkan efisiensi. Terdapat beberapa cara implementasi priority queue seperti array, linked list, heap, dan binary search tree.

## Referensi

[1]	J. A. Aryandi, M. A. Nugraha, Y. A. A. Basith, M. F. Pratama, D. Pradeka, and D. Anggraini, “Implementasi Algoritma Queue untuk Menentukan Prioritas Pelayanan Umum di Rumah Sakit,” JIKO (Jurnal Inform. dan Komputer), vol. 7, no. 2, p. 218, 2023, doi: 10.26798/jiko.v7i2.806.

[2]	L. Kohútka, “Efficiency of Priority Queue Architectures in FPGA †,” J. Low Power Electron. Appl., vol. 12, no. 3, 2022, doi: 10.3390/jlpea12030039.
