# **Running Time**

> waktu yang dibutuhkan oleh suatu algoritma untuk menyelesaikan tugasnya.

Contoh:
 Sebuah fungsi yang mengecek apakah angka 5 ada dalam array `[1, 2, 3, 4, 5]` mungkin butuh 5 langkah pengecekan (1 langkah per elemen).

Tapi karena waktu berjalan bisa berbeda di setiap komputer, kita gunakan **model matematis** untuk mengukurnya → yaitu: **Big O Notation**

------

# Big O Notation

> cara untuk menggambarkan bagaimana **running time** suatu algoritma **berkembang seiring bertambahnya ukuran input  atau banyaknya data (`n`)**.

------

## Notasi Big O Umum

| Notasi     | Nama         | Contoh Algoritma                | Waktu Perkiraan (n = 1.000.000) |
| ---------- | ------------ | ------------------------------- | ------------------------------- |
| O(1)       | Konstan      | Akses array: `arr[i]`           | Sangat cepat (~1 langkah)       |
| O(log n)   | Logaritmik   | Binary search                   | ~20 langkah                     |
| O(n)       | Linear       | Linear search, loop for         | 1.000.000 langkah               |
| O(n log n) | Linear-log   | Merge sort, quicksort avg       | ~20.000.000 langkah             |
| O(n²)      | Kuadratik    | Nested loop (`for` dalam `for`) | 1.000.000.000.000 langkah       |
| O(2ⁿ)      | Eksponensial | Rekursi bruteforce              | Meledak cepat                   |

------

## Implementasi dalam **JavaScript**

### O(1) – Konstan

```javascript
function getFirstElement(arr) {
  return arr[0];
}

const data = [10, 20, 30, 40, 50];
console.log("O(1):", data[2]);  // Output: 30
```

> [!NOTE]
>
> Tidak peduli panjang array-nya, hanya 1 langkah dilakukan.

------

### O(n) – Linear

```javascript
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) return i;
  }
  return -1;
}

const nums = [2, 4, 6, 8, 10];
console.log("O(n):", linearSearch(nums, 8));  // Output: 3
```

> [!NOTE]
>
> Jumlah langkah sama dengan jumlah data, semua dicek (**worst case**).

------

### O(log n) – Logaritmik (Binary Search)

```javascript
function binarySearch(arr, target) {
  let low = 0;
  let high = arr.length - 1;

  while (low <= high) {
    const mid = Math.floor((low + high) / 2);
    if (arr[mid] === target) return mid;
    else if (arr[mid] < target) low = mid + 1;
    else high = mid - 1;
  }

  return -1;
}

const sorted = [7, 3, 5, 11, 9, 1, 13].sort((a, b) => a - b);
console.log("O(log n):", binarySearch(sorted, 11));  // Output: 5
```

> [!NOTE]
>
> - Harus digunakan untuk **data yang terurut**.
> - Setiap langkah membagi data menjadi setengah.
>
> | Data   | Urutan Ascending         | Urutan Descending        |
> | ------ | ------------------------ | ------------------------ |
> | Angka  | `.sort((a, b) => a - b)` | `.sort((a, b) => b - a)` |
> | String | `.sort()`                | `.sort().reverse()`      |

------

### O(n²) – Kuadratik

```javascript
function hasDuplicateBruteForce(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j]) return true;
    }
  }
  return false;
}

const test = [1, 2, 3, 4, 2];
console.log("O(n²):", hasDuplicates(test));  // Output: true
```

> [!NOTE]
>
> - 1 element memiliki pengecekan sebanyak jumlah data yang di terima
> - 5 data = 1 data 5 pengecekan, 5 data 25 pengecekan

------

### O(n log n) – Linear-Logarithmic Time

> Menggunakan `Array.prototype.sort()` (dalam V8 menggunakan TimSort)

```javascript
const unsorted = [42, 12, 35, 1, 23, 8];

const sortedArr = [...unsorted].sort((a, b) => a - b);
console.log("O(n log n):", sortedArr);  // Output: [1, 8, 12, 23, 35, 42]
```

> [!NOTE]
>
> Sorting optimal (merge sort / TimSort) dilakukan dalam `O(n log n)`.

---

### O(2ⁿ) – Eksponensial Time

> Hitung semua subset dari array (rekursif)

```javascript
function generateSubsets(arr, i = 0, current = [], result = []) {
  if (i === arr.length) {
    result.push([...current]);
    return;
  }

  // Ambil elemen
  current.push(arr[i]);
  generateSubsets(arr, i + 1, current, result);

  // Jangan ambil elemen
  current.pop();
  generateSubsets(arr, i + 1, current, result);

  return result;
}

const subsets = generateSubsets([1, 2, 3]);
console.log("O(2ⁿ):", subsets);
/* Output:
[
  [ 1, 2, 3 ],
  [ 1, 2 ],
  [ 1, 3 ],
  [ 1 ],
  [ 2, 3 ],
  [ 2 ],
  [ 3 ],
  []
]
*/
```

> [!NOTE]
>
> ### **Ilustrasi Pemanggilan (Array: [1, 2, 3])**
>
> mulai dari `i=0`, `current=[]`, `result = []` 
>
> #### Langkah 1
>
> - `i = 0`, `current = []`, `result = []`
> - **Ambil elemen indeks ke-0 (1)** → `current = [1]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 1, [1], []);`
>
> ------
>
> #### Langkah 2
>
> - `i = 1`, `current = [1]`, `result = []`
> - **Ambil elemen indeks ke-1 (2)** → `current = [1, 2]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 2, [1, 2], []);`
>
> ------
>
> #### Langkah 3
>
> - `i = 2`, `current = [1, 2]`, `result = []`
> - **Ambil elemen indeks ke-2 (3)** → `current = [1, 2, 3]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [1, 2, 3], []);`
>
> ------
>
> #### Langkah 4
>
> - `i = 3`, `current = [1, 2, 3]`, `result = []`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([1, 2, 3])` → `result = [[1, 2, 3]]`
> - Kembali ke pemanggilan sebelumnya (Langkah 3)
>
> ------
>
> #### Kembali ke Langkah 3
>
> - `i = 2`, `current = [1, 2, 3]`, `result = [[1, 2, 3]]`
> - **Jangan ambil elemen indeks ke-2 (3)**
> - `current.pop()` → `current = [1, 2]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [1, 2], [[1, 2, 3]]);`
>
> ------
>
> #### Langkah 5
>
> - `i = 3`, `current = [1, 2]`, `result = [[1, 2, 3]]`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([1, 2])` → `result = [[1, 2, 3], [1, 2]]`
> - Kembali ke pemanggilan sebelumnya (Langkah 2)
>
> ------
>
> #### Kembali ke Langkah 2
>
> - `i = 1`, `current = [1, 2]`, `result = [[1, 2, 3], [1, 2]]`
> - **Jangan ambil elemen indeks ke-1 (2)**
> - `current.pop()` → `current = [1]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 2, [1], [[1, 2, 3], [1, 2]]);`
>
> ------
>
> #### Langkah 6
>
> - `i = 2`, `current = [1]`, `result = [[1, 2, 3], [1, 2]]`
> - **Ambil elemen indeks ke-2 (3)**
> - `current.push(3)` → `current = [1, 3]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [1, 3], [[1, 2, 3], [1, 2]]);`
>
> ------
>
> #### Langkah 7
>
> - `i = 3`, `current = [1, 3]`, `result = [[1, 2, 3], [1, 2]]`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([1, 3])` → `result = [[1, 2, 3], [1, 2], [1, 3]]`
> - Kembali ke pemanggilan sebelumnya (Langkah 6)
>
> ------
>
> #### Kembali ke Langkah 6
>
> - `i = 2`, `current = [1, 3]`, `result = [[1, 2, 3], [1, 2], [1, 3]]`
> - **Jangan ambil elemen indeks ke-2 (3)**
> - `current.pop()` → `current = [1]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [1], [[1, 2, 3], [1, 2], [1, 3]]);`
>
> ------
>
> #### Langkah 8
>
> - `i = 3`, `current = [1]`, `result = [[1, 2, 3], [1, 2], [1, 3]]`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([1])` → `result = [[1, 2, 3], [1, 2], [1, 3], [1]]`
> - Kembali ke pemanggilan sebelumnya (Langkah 1)
>
> ------
>
> #### Kembali ke Langkah 1
>
> - `i = 0`, `current = [1]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1]]`
> - **Jangan ambil elemen indeks ke-0 (1)**
> - `current.pop()` → `current = []`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 1, [], [[1, 2, 3], [1, 2], [1, 3], [1]]);`
>
> ------
>
> #### Langkah 9
>
> - `i = 1`, `current = []`, `result = [[1, 2, 3], [1, 2], [1, 3], [1]]`
> - **Ambil elemen indeks ke-1 (2)**
> - `current.push(2)` → `current = [2]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 2, [2], [[1, 2, 3], [1, 2], [1, 3], [1]]);`
>
> ------
>
> #### Langkah 10
>
> - `i = 2`, `current = [2]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1]]`
> - **Ambil elemen indeks ke-2 (3)**
> - `current.push(3)` → `current = [2, 3]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [2, 3], [[1, 2, 3], [1, 2], [1, 3], [1]]);`
>
> ------
>
> #### Langkah 11
>
> - `i = 3`, `current = [2, 3]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1]]`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([2, 3])` → `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3]]`
> - Kembali ke pemanggilan sebelumnya (Langkah 10)
>
> ------
>
> #### Kembali ke Langkah 10
>
> - `i = 2`, `current = [2, 3]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3]]`
> - **Jangan ambil elemen indeks ke-2 (3)**
> - `current.pop()` → `current = [2]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [2], [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3]]);`
>
> ------
>
> #### Langkah 12
>
> - `i = 3`, `current = [2]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3]]`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([2])` → `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2]]`
> - Kembali ke pemanggilan sebelumnya (Langkah 9)
>
> ------
>
> #### Kembali ke Langkah 9
>
> - `i = 1`, `current = [2]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2]]`
> - **Jangan ambil elemen indeks ke-1 (2)**
> - `current.pop()` → `current = []`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 2, [], [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2]]);`
>
> ------
>
> #### Langkah 13
>
> - `i = 2`, `current = []`, `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2]]`
> - **Ambil elemen indeks ke-2 (3)**
> - `current.push(3)` → `current = [3]`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [3], [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2]]);`
>
> ------
>
> #### Langkah 14
>
> - `i = 3`, `current = [3]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2]]`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([3])` → `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2], [3]]`
> - Kembali ke pemanggilan sebelumnya (Langkah 13)
>
> ------
>
> #### Kembali ke Langkah 13
>
> - `i = 2`, `current = [3]`, `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2], [3]]`
> - **Jangan ambil elemen indeks ke-2 (3)**
> - `current.pop()` → `current = []`
> - Panggil rekursif:
>    `generateSubsets([1, 2, 3], 3, [], [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2], [3]]);`
>
> ------
>
> #### Langkah 15
>
> - `i = 3`, `current = []`, `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2], [3]]`
> - Karena `i == arr.length`, simpan subset ke result:
>    `result.push([])` → `result = [[1, 2, 3], [1, 2], [1, 3], [1], [2, 3], [2], [3], []]`
>
> ------
>
> ### Intinya:
>
> - Setiap elemen punya 2 pilihan: **ambil** atau **tidak**.
> - Jadi total kombinasi = 2^n (n = jumlah elemen).
> - Fungsi rekursif berjalan **memecah masalah menjadi sub-masalah lebih kecil** sampai kondisi dasar (`i == arr.length`) terpenuhi.

---

# Simulasi Waktu (Javascript)

Misalnya kamu ingin tahu berapa waktu real untuk O(n):

```javascript
const data = Array.from({ length: 1000000 }, (_, i) => i);

console.time("LinearSearch");
linearSearch(data, 999999); // terakhir
console.timeEnd("LinearSearch");
```

Hasilnya mungkin:

```javascript
LinearSearch: 10.5 ms
```

> [!NOTE]
>
> `console.time` adalah fitur debugging di JavaScript yang digunakan untuk **mengukur waktu eksekusi** dari suatu blok kode. Ini sangat berguna untuk melihat performa dan seberapa lama suatu proses berjalan.
>
> ### Cara Kerja:
>
> memulai timer dengan `console.time(label)` dan menghentikannya dengan `console.timeEnd(label)`. Label (biasanya string) berfungsi sebagai nama pengenal timer tersebut.
>
> ### Contoh Penggunaan:
>
> ```javascript
> console.time('loop');
> 
> for (let i = 0; i < 1000000; i++) {
>   // proses yang ingin diukur
> }
> 
> console.timeEnd('loop');
> ```
>
> ### Output:
>
> Misalnya:
>
> ```javascript
> loop: 12.345ms
> ```
>
> Artinya, proses dalam loop tersebut memakan waktu sekitar 12.345 milidetik untuk dijalankan.

------

# Kesimpulan

- Gunakan **Big O Notation** untuk menganalisis performa algoritma
- Pilih algoritma efisien sesuai skala data
- Untuk data besar, hindari algoritma O(n²) atau O(2ⁿ)