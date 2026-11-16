# <h1 align="center">Laporan Praktikum Modul 7 - Stack</h1>
<p align="center">Hafis Saputra - 109082500087</p>

## Dasar Teori

### A. Stack

Stack merupakan struktur data yang prinsip kerjanya seperti tumpukan. Elemen yang terakhir dimasukkan akan menjadi elemen pertama yang diambil. Prinsip ini disebut LIFO (Last In First Out) [1].

### B. Komponen Stack

Komponen stack pada dasarnya sama dengan komponen pada singly linked list, tetapi akses data hanya dilakukan pada satu ujung saja, yaitu elemen paling atas yang disebut `Top`. Pada stack kosong, nilai `Top` menunjukkan bahwa belum ada elemen yang tersimpan [1].

### C. Operasi pada Stack

Stack memiliki dua operasi utama, yaitu `push` dan `pop`. Operasi `push` menyisipkan elemen baru di posisi paling atas stack, sedangkan operasi `pop` mengambil elemen paling atas dan menggeser `Top` ke elemen sebelumnya. Selain itu terdapat primitif pendukung seperti `createStack()` untuk membuat stack kosong, `isEmpty()` untuk memeriksa stack kosong, dan `printInfo()` untuk menampilkan isi stack [1].

### D. Stack Representasi Tabel (Array)

Pada representasi tabel, stack dibuat menggunakan array berindeks dengan jumlah elemen yang terbatas. Operasi `push` dilakukan dengan menaikkan nilai `Top` lalu menyimpan data pada indeks `Top`, sedangkan operasi `pop` mengambil data pada indeks `Top` lalu menurunkan nilai `Top`. Berbeda dengan representasi pointer, representasi tabel tidak memerlukan alokasi dan dealokasi memori [1].

### E. Penerapan Stack pada C++

Pada praktikum ini, stack dibuat menggunakan array berukuran 20 dengan tipe data integer. Program dibagi menjadi file header `stack.h`, file implementasi `stack.cpp`, dan file utama `main.cpp` agar deklarasi, proses fungsi, dan jalannya program tersusun lebih rapi [1], [2].

## Guided

### 1. Struktur Data Stack pada stack.h

```cpp
#ifndef STACK_H_INCLUDED
#define STACK_H_INCLUDED

#include <iostream>
using namespace std;

const int MAX_STACK = 20;

typedef int infotype;

struct Stack {
    infotype info[MAX_STACK];
    int top;            // top = -1 berarti stack kosong
};

void createStack(Stack &S);
bool isEmpty(const Stack &S);
bool isFull(const Stack &S);
void push(Stack &S, infotype x);
infotype pop(Stack &S);
void printInfo(const Stack &S);
void balikStack(Stack &S);
void pushAscending(Stack &S, infotype x);
void getInputStream(Stack &S);

#endif // STACK_H_INCLUDED
```

File `stack.h` digunakan untuk mendefinisikan struktur stack dan prototype seluruh fungsi. Struktur `Stack` berisi array `info` berukuran 20 untuk menyimpan data integer dan variabel `top` sebagai penunjuk elemen paling atas. Nilai `top = -1` menandakan stack kosong.

### 2. Implementasi Fungsi pada stack.cpp

```cpp
#include <iostream>
#include "stack.h"
using namespace std;

void createStack(Stack &S) {
    S.top = -1;
}

bool isEmpty(const Stack &S) {
    return S.top == -1;
}

bool isFull(const Stack &S) {
    return S.top == MAX_STACK - 1;
}

void push(Stack &S, infotype x) {
    if (isFull(S)) {
        cout << "Stack penuh" << endl;
        return;
    }
    S.top++;
    S.info[S.top] = x;
}

infotype pop(Stack &S) {
    if (isEmpty(S)) {
        cout << "Stack kosong" << endl;
        return -1;
    }
    infotype x = S.info[S.top];
    S.top--;
    return x;
}

void printInfo(const Stack &S) {
    cout << "[TOP] ";
    for (int i = S.top; i >= 0; i--) {
        cout << S.info[i] << " ";
    }
    cout << endl;
}

void balikStack(Stack &S) {
    int i = 0;
    int j = S.top;
    while (i < j) {
        infotype temp = S.info[i];
        S.info[i] = S.info[j];
        S.info[j] = temp;
        i++;
        j--;
    }
}

void pushAscending(Stack &S, infotype x) {
    if (isFull(S)) {
        cout << "Stack penuh" << endl;
        return;
    }

    Stack temp;
    createStack(temp);

    // pindahkan elemen yang lebih besar dari x ke stack bantu
    while (!isEmpty(S) && S.info[S.top] > x) {
        push(temp, pop(S));
    }

    push(S, x);

    // kembalikan elemen dari stack bantu
    while (!isEmpty(temp)) {
        push(S, pop(temp));
    }
}

void getInputStream(Stack &S) {
    int c = cin.get();
    while (c != '\n' && c != EOF && !isFull(S)) {
        if (c >= '0' && c <= '9') {
            push(S, c - '0');
        }
        c = cin.get();
    }
}
```

File `stack.cpp` berisi proses dari fungsi-fungsi yang dideklarasikan pada header. `createStack()` mengatur `top` menjadi -1. Fungsi `push()` menaikkan `top` lalu menyimpan data baru, sedangkan `pop()` mengambil data pada `top` lalu menurunkan `top`. Fungsi `printInfo()` menampilkan isi stack dari `top` menuju elemen paling bawah, sedangkan `balikStack()` membalik urutan isi stack dengan menukar elemen dari kedua ujung array.

## Unguided

### 1. Pembuatan ADT Stack dan Operasi Dasar

Soal pertama meminta pembuatan ADT Stack menggunakan array yang berisi `createStack`, `push`, `pop`, `printInfo`, dan `balikStack`. Program menjalankan urutan operasi push 3, 4, 8, lalu pop, push 2, push 3, lalu pop, dan push 9. Setelah operasi tersebut, isi stack dari atas ke bawah adalah 9, 2, 4, dan 3. Setelah stack dibalik dengan `balikStack()`, isinya menjadi 3, 4, 2, dan 9.

### 2. Prosedur pushAscending

Soal kedua meminta penambahan prosedur `pushAscending` agar data yang dimasukkan selalu terurut. Prosedur ini memindahkan elemen yang lebih besar dari data baru ke stack bantu, menyisipkan data baru, kemudian mengembalikan elemen dari stack bantu ke stack utama. Dengan cara ini, isi stack selalu terurut dari kecil di bagian bawah ke besar di bagian atas.

### 3. Prosedur getInputStream

Soal ketiga meminta penambahan prosedur `getInputStream` yang terus membaca masukan pengguna dan memasukkan setiap karakter angka ke dalam stack sampai pengguna menekan tombol enter. Pembacaan dilakukan menggunakan `cin.get()`, kemudian karakter angka diubah menjadi nilai integer sebelum di-push.

### 4. Program Utama main.cpp

```cpp
#include <iostream>
#include "stack.h"
using namespace std;

int main() {
    Stack S;

    
    cout << "=== Latihan 1 ===" << endl;
    cout << "Hello world!" << endl;
    createStack(S);
    push(S, 3);
    push(S, 4);
    push(S, 8);
    pop(S);
    push(S, 2);
    push(S, 3);
    pop(S);
    push(S, 9);
    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);

    // ===== Latihan 2: pushAscending =====
    cout << "\n=== Latihan 2 ===" << endl;
    cout << "Hello world!" << endl;
    createStack(S);
    pushAscending(S, 3);
    pushAscending(S, 4);
    pushAscending(S, 8);
    pushAscending(S, 2);
    pushAscending(S, 3);
    pushAscending(S, 9);
    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);

  
    cout << "\n=== Latihan 3 ===" << endl;
    cout << "Hello world!" << endl;
    createStack(S);
    getInputStream(S);
    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);

    return 0;
}
```

Kode pada `main.cpp` menjalankan ketiga latihan secara berurutan. Setiap latihan membuat stack kosong terlebih dahulu, melakukan operasi yang diminta, menampilkan isi stack, membalik stack, lalu menampilkan isinya kembali.

### 5. Hasil Output Program

**Output 1 — Operasi Push, Pop, dan Balik Stack. Link foto output: https://github.com/apinghapis29-code/modul-7/blob/8ed33348437c5d73688d387cda5dd9e47cc69ec2/Cuplikan%20layar%202026-11-16%20132304.png

```text
=== Latihan 1 ===
Hello world!
[TOP] 9 2 4 3 
balik stack
[TOP] 3 4 2 9 
```

Pada output pertama, stack setelah seluruh operasi berisi 9, 2, 4, dan 3 dari posisi `TOP`. Setelah `balikStack()` dijalankan, urutannya menjadi 3, 4, 2, dan 9 sesuai contoh pada soal latihan [2].

**Output 2 — Push Ascending. Link foto output: https://github.com/apinghapis29-code/modul-7/blob/8ed33348437c5d73688d387cda5dd9e47cc69ec2/Cuplikan%20layar%202026-11-16%20132304.png

```text
=== Latihan 2 ===
Hello world!
[TOP] 9 8 4 3 3 2 
balik stack
[TOP] 2 3 3 4 8 9 
```

Output kedua menunjukkan bahwa `pushAscending()` berhasil menjaga isi stack tetap terurut. Dari posisi `TOP` data terlihat menurun (9, 8, 4, 3, 3, 2), dan setelah stack dibalik data terlihat menaik [2].

**Output 3 — Input Stream. Link foto output: https://github.com/apinghapis29-code/modul-7/blob/8ed33348437c5d73688d387cda5dd9e47cc69ec2/Cuplikan%20layar%202026-11-16%20132304.png

```text
=== Latihan 3 ===
Hello world!
4729601
[TOP] 1 0 6 9 2 7 4 
balik stack
[TOP] 4 7 2 9 6 0 1 
```

Output ketiga menunjukkan bahwa `getInputStream()` membaca setiap digit dari masukan `4729601` lalu memasukkannya ke stack. Karena sifat LIFO, digit terakhir (1) berada di posisi `TOP`. Setelah dibalik, urutan stack sama dengan urutan masukan [2].

## Kesimpulan

1. Stack bekerja dengan prinsip LIFO sehingga elemen yang terakhir dimasukkan menjadi elemen yang pertama diambil, dan seluruh akses dilakukan melalui `Top`.
2. Stack dengan representasi array berhasil melakukan operasi `push`, `pop`, `printInfo`, dan `balikStack` sesuai contoh output pada soal.
3. Prosedur `pushAscending()` berhasil menjaga data tetap terurut menggunakan stack bantu, dan `getInputStream()` berhasil memasukkan masukan pengguna ke dalam stack hingga tombol enter ditekan.

## Referensi

[1] Tim Penyusun, *Modul 7: Stack*, Buku Ajar Praktikum Struktur Data, hlm. 59–65, t.t.

[2] Tim Penyusun, *Soal Latihan Modul 7 Praktikum Struktur Data*, hlm. 64–65, t.t.
