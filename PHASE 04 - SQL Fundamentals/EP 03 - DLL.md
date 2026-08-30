## Data Definition Language

DDL digunakan untuk mendefinisikan dan mengubah **struktur objek database**.

Perintah utama:

```text
CREATE
ALTER
DROP
TRUNCATE
RENAME
COMMENT
```

Contoh objek yang dapat dikelola antara lain:

```text
TABLE
VIEW
INDEX
SEQUENCE
SYNONYM
```

### CREATE

Membuat objek database.

```sql
CREATE TABLE mahasiswa (
    nim NUMBER(10),
    nama VARCHAR2(100),
    umur NUMBER(3)
);
```

Oracle mendefinisikan table sebagai struktur dasar untuk menyimpan data user. `CREATE TABLE` digunakan untuk membuat tabel tersebut. ([Oracle Docs][2])

### ALTER

Mengubah struktur objek yang sudah ada.

```sql
ALTER TABLE mahasiswa
ADD email VARCHAR2(100);
```

### DROP

Menghapus objek database.

```sql
DROP TABLE mahasiswa;
```

### TRUNCATE

Menghapus seluruh data dari tabel tanpa menghapus struktur tabel.

```sql
TRUNCATE TABLE mahasiswa;
```

### RENAME

Mengubah nama objek.

```sql
RENAME mahasiswa TO mahasiswa_aktif;
```

### COMMENT

Memberikan deskripsi pada objek database.

```sql
COMMENT ON TABLE mahasiswa
IS 'Data mahasiswa aktif';
```

---
