# 🔍 Apa Itu Binary Search?

**Binary Search (Pencarian Biner)** adalah sebuah **algoritma pencarian** yang digunakan untuk mencari sebuah elemen pada **daftar (array atau list) yang sudah terurut**.

Binary search bekerja dengan cara **membagi dua** daftar pencarian, lalu membandingkan elemen tengah dengan target. Jika belum ditemukan, algoritma akan memilih **setengah daftar** yang mungkin berisi target dan membuang sisanya. Proses ini diulang hingga ditemukan atau tidak ada elemen yang tersisa.

------

# ⚙️ Syarat Binary Search

- Data **harus terurut** (ascending atau descending).
- Harus bisa mengakses elemen berdasarkan indeks (misalnya list/array, bukan linked list).

------

# 🧠 Cara Kerja Binary Search

1. Tentukan **awal** dan **akhir** dari list (misalnya indeks `low` dan `high`).
2. Ambil elemen di **tengah**: `mid = (low + high) // 2`
3. Bandingkan elemen tengah dengan target:
   - Jika sama, **berhasil** ditemukan.
   - Jika target lebih kecil, cari di **kiri**: `high = mid - 1`
   - Jika target lebih besar, cari di **kanan**: `low = mid + 1`
4. Ulangi sampai ketemu atau daftar habis.

------

# 🔢 Contoh Ilustrasi

Misal kamu punya daftar angka:

```
[1, 3, 5, 7, 9, 11, 13, 15]
```

Dan kamu ingin mencari angka `11`.

1. Ambil tengah: elemen ke-3 (`7`)
2. `11 > 7`, jadi cari di kanan → `[9, 11, 13, 15]`
3. Ambil tengah: `11`
4. Ditemukan!

------

# ⌛ Kompleksitas Waktu

- **Best Case:** O(1) → Langsung ketemu
- **Average Case:** O(log n)
- **Worst Case:** O(log n)

| Kasus        | Notasi   | Artinya secara praktis                                 |
| ------------ | -------- | ------------------------------------------------------ |
| Best Case    | O(1)     | Ditemukan langsung di tengah (sekali cek)              |
| Average Case | O(log n) | Rata-rata pencarian memotong separuh data tiap langkah |
| Worst Case   | O(log n) | Pencarian terus dilakukan sampai sisa 1 data           |

## 📊 **Ilustrasi Sederhana**

Misalnya kita cari angka **22** dalam array `[3, 7, 15, 20, 22, 30, 42, 55]`:

Jumlah data: 8 elemen → log₂(8) = 3
 Jadi maksimal butuh **3 langkah** pencarian.

Langkah-langkah:

1. Cek elemen tengah → `20` → bukan
2. Ambil setengah yang lebih besar → `30` → bukan
3. Ambil setengah yang lebih kecil → `22` → cocok ✅

Itu **3 langkah saja**, bukan 8 seperti di pencarian linear.Binary search jauh lebih cepat dibandingkan pencarian linear O(n) untuk data besar.

## 🧠 Interpretasi 

**log₂(20) ≈ 4.32**

> Kamu perlu membagi `20` secara **berulang kali menjadi setengah**, sebanyak **sekitar 4.32 kali**, sampai tersisa 1.

------

### ✂️ Simulasi Pembagian Setengah

Misalnya kamu tidak pakai log langsung, tapi **bagi 20 jadi setengah terus**:

| Langkah | Nilai |             |
| ------- | ----- | ----------- |
| 0       | 20    | (awal)      |
| 1       | 10    | 1 pembagian |
| 2       | 5     | 2 pembagian |
| 3       | 2.5   | 3 pembagian |
| 4       | 1.25  | 4 pembagian |
| 5       | 0.625 | 5 pembagian |

Nah, di antara langkah ke-4 dan ke-5, nilainya lewat angka 1.

Artinya:

> **Antara pembagian ke-4 dan ke-5**, kamu sudah melewati nilai 1 → jadi jumlah pembagiannya adalah **antara 4 dan 5**, yaitu **sekitar 4.32**

------

### 📌 Kesimpulan

- `log₂(20) ≈ 4.32` artinya **2^4.32 ≈ 20**

- Dalam konteks binary search, ini artinya:

  > Butuh sekitar **4.32 langkah pembagian dua** untuk mencari dalam data berukuran 20

- Karena kamu **nggak bisa ambil 0.32 langkah**, maka:

  - ⏱ Untuk hitung waktu/kompleksitas maksimal → ambil `ceil(4.32) = 5`
  - 📍 Untuk akses indeks array → pakai `floor` atau pembulatan ke bawah

------

# 🧑‍💻 Implementasi Kode (Javascirpt)

```javascript
function binarySearch(data, target) {
    let low = 0;
    let high = data.length - 1;

    while (low <= high) {
        let mid = Math.floor((low + high) / 2);

        if (data[mid] === target) {
            return mid; // ditemukan
        } else if (data[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return -1; // tidak ditemukan
}

// Contoh penggunaan:
let data = [3, 7, 15, 20, 22, 30, 42, 55];
let target = 22;

let result = binarySearch(data, target);
console.log("Hasil pencarian:", result);  // Output: 4
```

## **Ilustrasi Langkah-langkah**

> Mencari `target = 22` dalam array:
>  `[3, 7, 15, 20, 22, 30, 42, 55]`

------

### 📦 **Langkah 1**

- `low = 0`, `high = 7`
- `mid = Math.floor((0 + 7) / 2) = 3`
- `data[3] = 20`
- ➡️ 20 < 22 → pindah ke kanan
- 🔁 `low = 4`, `high = 7`

------

### 📦 **Langkah 2**

- `mid = Math.floor((4 + 7) / 2) = 5`
- `data[5] = 30`
- ➡️ 30 > 22 → pindah ke kiri
- 🔁 `low = 4`, `high = 4`

------

### 📦 **Langkah 3**

- `mid = Math.floor((4 + 4) / 2) = 4`
- `data[4] = 22` → ✅ cocok!

------

## ✅ **Hasil akhir:**

- Nilai `22` ditemukan di indeks `4`

------

## 🔍 Ilustrasi Visual

| Langkah | low  | high | mid  | data[mid] | Aksi           |
| ------- | ---- | ---- | ---- | --------- | -------------- |
| 1       | 0    | 7    | 3    | 20        | Geser ke kanan |
| 2       | 4    | 7    | 5    | 30        | Geser ke kiri  |
| 3       | 4    | 4    | 4    | 22        | ✅ Ditemukan    |

------

# 🌍 Contoh Implementasi Binary Search di Dunia Nyata

## 1. **Mencari Nama dalam Buku telepon**

Buku telepon diurutkan berdasarkan abjad. Kalau kamu mencari "Rudi", kamu tidak perlu membaca dari awal, cukup buka bagian tengah dan sesuaikan: lebih awal atau lebih akhir? Itu adalah **binary search manual**.

## 2. **Pencarian di Kamus**

Mencari kata dalam kamus cetak menggunakan metode serupa dengan binary search. Kamu buka halaman tengah dan mempersempit pencarian berdasarkan alfabet.

## 3. **Pencarian Produk di Toko Online (yang Sudah Diurutkan)**

Jika produk diurutkan berdasarkan harga dan kamu ingin tahu apakah ada produk seharga Rp50.000, backend bisa pakai binary search untuk mencarinya.

## 4. **Debugging "Bug" dalam Kode**

Jika ada error setelah baris ke-1000 dari 2000, kamu bisa "bagi dua" untuk mencari di mana letak error — ini konsep binary search yang disebut **"git bisect"** dalam pemrograman.

## 5. **Game: Menebak Angka**

Dalam permainan "tebak angka dari 1–100", jika kamu langsung tebak 50, lalu 75, lalu 62 — itu kamu sedang **secara alami menggunakan binary search**.

------

# 📌 Kelebihan & Kekurangan Binary Search

## ✅ Kelebihan:

- Sangat cepat untuk data besar yang terurut
- Sederhana dan efisien
- Mudah diimplementasikan

## ❌ Kekurangan:

- Hanya bisa digunakan pada data **yang sudah terurut**
- Tidak efisien jika data tidak bisa diakses secara langsung (seperti linked list)

------

# 📚 Kesimpulan

- Binary search adalah algoritma pencarian cepat dengan logika "bagi dua".
- Wajib digunakan jika kamu sering bekerja dengan data yang besar dan **sudah terurut**.
- Digunakan secara luas di dunia nyata, baik secara digital maupun manual (buku, kamus, debugging, dll).
