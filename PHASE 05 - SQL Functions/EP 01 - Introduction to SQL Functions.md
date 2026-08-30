## Definition

**SQL Function** adalah fungsi yang digunakan untuk melakukan operasi atau transformasi terhadap data dan menghasilkan suatu nilai.

Dalam Oracle SQL, function dapat digunakan dalam berbagai bagian query, terutama `SELECT`, `WHERE`, `ORDER BY`, dan `GROUP BY`.

Contoh:

```sql
SELECT UPPER(nama)
FROM mahasiswa;
```

Jika:

```text
nama
----------
Budi
Andi
Siti
```

hasilnya:

```text
UPPER(NAMA)
-----------
BUDI
ANDI
SITI
```

---

## Mengapa SQL Functions Penting?

Tanpa function, banyak proses pengolahan data harus dilakukan di aplikasi.

Dengan SQL function, database dapat melakukan transformasi data secara langsung.

Contoh kebutuhan nyata:

```text
Data mentah
     ↓
UPPER()
     ↓
Standarisasi nama
```

atau:

```text
Tanggal lahir
     ↓
MONTHS_BETWEEN()
     ↓
Menghitung usia
```

atau:

```text
Harga × Jumlah
     ↓
SUM()
     ↓
Total penjualan
```

---
