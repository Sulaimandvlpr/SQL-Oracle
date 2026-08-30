Secara umum, function yang akan kita pelajari dapat dibagi menjadi dua kelompok utama:

```text
SQL FUNCTIONS
│
├── Single-Row Functions
│   ├── Character
│   ├── Number
│   ├── Date
│   ├── Conversion
│   └── NULL
│
└── Aggregate Functions
    ├── COUNT()
    ├── SUM()
    ├── AVG()
    ├── MIN()
    └── MAX()
```

---

# Single-Row Functions

**Single-row function** memproses setiap row secara individual dan menghasilkan satu nilai untuk setiap row.

Contoh:

```sql
SELECT nama,
       UPPER(nama) AS nama_upper
FROM mahasiswa;
```

Jika terdapat 3 row:

```text
Budi
Andi
Siti
```

function diproses terhadap masing-masing row:

```text
Budi → BUDI
Andi → ANDI
Siti → SITI
```

---

# Aggregate Functions

**Aggregate function** bekerja terhadap sekumpulan row dan menghasilkan satu nilai hasil agregasi.

Contoh:

```sql
SELECT AVG(umur)
FROM mahasiswa;
```

Jika umur:

```text
20
21
19
22
```

maka:

```text
AVG = 20.5
```

Aggregate functions akan menjadi lebih penting ketika kita masuk ke:

```text
GROUP BY
HAVING
```

pada phase berikutnya.

---
