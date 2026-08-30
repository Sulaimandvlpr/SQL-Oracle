## Data Manipulation Language

DML digunakan untuk **memanipulasi data yang terdapat di dalam tabel**.

Perintah utama:

```text
INSERT
UPDATE
DELETE
MERGE
```

### INSERT

Menambahkan baris baru.

```sql
INSERT INTO mahasiswa (
    nim,
    nama,
    umur
)
VALUES (
    2507411018,
    'Budi',
    20
);
```

### UPDATE

Mengubah data yang sudah ada.

```sql
UPDATE mahasiswa
SET umur = 21
WHERE nim = 2507411018;
```

**Perhatikan `WHERE`.**

Tanpa `WHERE`:

```sql
UPDATE mahasiswa
SET umur = 21;
```

seluruh baris dalam tabel dapat berubah.

---

### DELETE

Menghapus data.

```sql
DELETE FROM mahasiswa
WHERE nim = 2507411018;
```

Tanpa `WHERE`:

```sql
DELETE FROM mahasiswa;
```

seluruh row dapat dihapus.

---

### MERGE

`MERGE` digunakan ketika kita ingin melakukan operasi berdasarkan apakah data sudah tersedia atau belum.

Secara konsep:

```text
Data ditemukan
      ↓
   UPDATE

Data tidak ditemukan
      ↓
   INSERT
```

Oracle menyediakan `MERGE` sebagai SQL statement untuk menggabungkan operasi tersebut berdasarkan kondisi tertentu.

---
