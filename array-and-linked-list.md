# Array

> struktur data yang menyimpan sekumpulan elemen yang berurutan dalam memori, dengan tipe data yang sama. Elemen-elemen disimpan secara **kontigu** (bersebelahan) di memori.

## Cara Kerja di Memori

- Array dialokasikan dalam blok memori yang berkesinambungan (selalu berdampingan / bersebelahan).

- Setiap elemen memiliki alamat memori yang dapat dihitung dengan formula:

  ```javascript
  alamat_elemen = alamat_awal + (index * ukuran_elemen)
  ```

- Akses elemen dapat dilakukan secara **langsung (random access)** menggunakan indeks, yang membuat operasi baca/tulis sangat cepat, yaitu O(1).

## Ilustrasi Memori Array

> **contoh 5 elemen integer**

```javascript
Alamat Memori:
[1000] -> Elemen 0 (10)
[1004] -> Elemen 1 (20)
[1008] -> Elemen 2 (30)
[1012] -> Elemen 3 (40)
[1016] -> Elemen 4 (50)
```

Misalnya, ukuran tiap elemen adalah 4 byte.

------

## Contoh Kode JavaScript

```javascript
// Membuat array
const arr = [10, 20, 30, 40, 50];

// Mengakses elemen
console.log(arr[2]); // Output: 30

// Menambahkan elemen di akhir
arr.push(60);
console.log(arr); // Output: [10, 20, 30, 40, 50, 60]

// Menghapus elemen terakhir
arr.pop();
console.log(arr); // Output: [10, 20, 30, 40, 50]

// Mengubah elemen
arr[1] = 25;
console.log(arr); // Output: [10, 25, 30, 40, 50]
```

------

### memori

#### 1. Saat Membuat Array Awal

```
arr = [10, 20, 30, 40, 50];
```

- Array ini dialokasikan di memori secara **kontigu** (berurutan) dengan ruang cukup untuk 5 elemen.
- Misal alamat memori mulai dari `1000`, tiap elemen 4 byte (contoh), jadi:

```
[1000] 10
[1004] 20
[1008] 30
[1012] 40
[1016] 50
```

------

### 2. Saat Mengakses Elemen `arr[2]`

- Akses langsung ke alamat `1008` karena posisi indeks 2.
- Operasi ini sangat cepat karena tinggal ambil data di memori itu.

------

### 3. Saat `arr.push(60)`

- Jika di memori setelah alamat `1016` masih kosong (misal alamat `1020` masih bebas),
   **elemen `60` langsung disimpan di sana:**

```
[1020] 60
```

- Jadi sekarang array jadi 6 elemen dengan memori kontigu dari `1000` sampai `1020`.
- **Tapi**, jika alamat setelah `1016` sudah dipakai oleh data lain (atau kapasitas array sudah penuh), engine JavaScript akan:
  - **Buat blok memori baru yang lebih besar**, misal dua kali lipat kapasitas (10 elemen) mulai dari alamat `2000`.
  - **Copy semua elemen dari alamat lama ke yang baru**.
  - Tambahkan elemen `60` di alamat berikutnya (`2020`).
  - Bebaskan memori lama (`1000 - 1016`).

------

### 4. Saat `arr.pop()`

- Elemen terakhir (`60`) dihapus,
- Panjang array berkurang jadi 5,
- Tapi data di memori mungkin tetap ada sampai nanti di-overwrite.

------

### 5. Saat `arr[1] = 25`

- Nilai elemen pada indeks 1 di alamat `1004` diubah dari `20` jadi `25` secara langsung.
- Tidak perlu pindah memori atau copy data karena ukuran array tetap sama.

------

## Ilustrasi Singkat:

| Operasi       | Memori (Alamat & Data)                                       |
| ------------- | ------------------------------------------------------------ |
| Awal          | [1000] 10, [1004] 20, [1008] 30, [1012] 40, [1016] 50        |
| `push(60)`    | Jika cukup ruang: [1020] 60 ditambah  Jika penuh: resize, copy ke alamat baru dan tambah 60 |
| `pop()`       | Hapus elemen terakhir (misal [1020])                         |
| `arr[1] = 25` | Ubah langsung di alamat [1004] dari 20 → 25                  |

------

------

## 2. Linked List

### Apa itu Linked List?

Linked List adalah struktur data yang terdiri dari node-node yang masing-masing menyimpan data dan referensi (pointer) ke node berikutnya. Node-node ini tidak harus bersebelahan di memori.

### Cara Kerja di Memori

- Setiap node bisa berada di lokasi memori yang berbeda.
- Node terdiri dari dua bagian:
  - **Data**: menyimpan nilai
  - **Pointer (next)**: menyimpan alamat node berikutnya
- Traversal harus dilakukan dari node pertama (head), mengikuti pointer ke node selanjutnya.
- Operasi akses elemen bersifat **sekuensial**, yaitu O(n) untuk akses indeks tertentu.

------

### Ilustrasi Memori Linked List (contoh 3 node)

```
Node1: [Data: 10 | Next: 0x1020]
Node2: [Data: 20 | Next: 0x1080]
Node3: [Data: 30 | Next: null]

Alamat Memori:
0x1000 -> Node1
0x1020 -> Node2
0x1080 -> Node3
```

------

### Contoh Kode Linked List di JavaScript

```
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
  }

  // Menambahkan node di akhir linked list
  append(data) {
    const newNode = new Node(data);
    if (!this.head) {
      this.head = newNode;
      return;
    }
    let current = this.head;
    while (current.next) {
      current = current.next;
    }
    current.next = newNode;
  }

  // Menampilkan isi linked list
  printList() {
    let current = this.head;
    const elements = [];
    while (current) {
      elements.push(current.data);
      current = current.next;
    }
    console.log(elements.join(' -> '));
  }
}

// Contoh penggunaan
const list = new LinkedList();
list.append(10);
list.append(20);
list.append(30);

list.printList(); // Output: 10 -> 20 -> 30
```

------

### Ilustrasi Memori saat Membuat Linked List

| Node  | Data | Pointer (Next) | Alamat Memori (contoh) |
| ----- | ---- | -------------- | ---------------------- |
| Node1 | 10   | 0x1020         | 0x1000                 |
| Node2 | 20   | 0x1080         | 0x1020                 |
| Node3 | 30   | null           | 0x1080                 |

- `list.head` menunjuk ke `Node1` di alamat `0x1000`.
- `Node1.next` menunjuk ke `Node2` di alamat `0x1020`.
- `Node2.next` menunjuk ke `Node3` di alamat `0x1080`.
- `Node3.next` adalah `null`, menandakan akhir linked list.

------

## Perbandingan Singkat Array vs Linked List

| Fitur           | Array                                       | Linked List                                              |
| --------------- | ------------------------------------------- | -------------------------------------------------------- |
| Struktur Memori | Kontigu (bersebelahan)                      | Node tersebar di memori                                  |
| Akses Elemen    | O(1) (langsung lewat index)                 | O(n) (traversal)                                         |
| Ukuran Dinamis  | Tidak (ukuran tetap, kecuali array dinamis) | Dinamis, bisa bertambah                                  |
| Insert/Delete   | Mahal di tengah (O(n))                      | Murah di mana saja (O(1) jika sudah tahu posisi)         |
| Penggunaan      | Saat perlu akses cepat dan ukuran tetap     | Saat perlu fleksibilitas ukuran dan sering insert/delete |