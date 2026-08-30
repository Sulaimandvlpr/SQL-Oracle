## Arithmetic Operators

```text
+
-
*
/
```

Contoh:

```sql
SELECT harga * jumlah AS total
FROM transaksi;
```

---

## Comparison Operators

```text
=
<>
!=
>
<
>=
<=
```

Contoh:

```sql
SELECT *
FROM mahasiswa
WHERE umur >= 20;
```

---

## Logical Operators

```text
AND
OR
NOT
```

Contoh:

```sql
SELECT *
FROM mahasiswa
WHERE umur >= 20
AND status = 'Aktif';
```

---

## BETWEEN

Mencari nilai dalam rentang tertentu.

```sql
SELECT *
FROM mahasiswa
WHERE umur BETWEEN 18 AND 22;
```

---

## IN

Mencocokkan nilai dengan beberapa kemungkinan.

```sql
SELECT *
FROM mahasiswa
WHERE jurusan IN ('TI', 'SI', 'TK');
```

---

## LIKE

Melakukan pencarian berdasarkan pola.

```sql
SELECT *
FROM mahasiswa
WHERE nama LIKE 'A%';
```

`%` berarti sejumlah karakter.

```text
'A%'      → dimulai dengan A
'%a'      → diakhiri dengan a
'%an%'    → mengandung "an"
```

---

## IS NULL

Mengecek apakah suatu nilai adalah `NULL`.

```sql
SELECT *
FROM mahasiswa
WHERE email IS NULL;
```

Jangan menggunakan:

```sql
WHERE email = NULL
```

karena `NULL` bukan nilai biasa yang dibandingkan menggunakan `=`.

---

# 04.07 — TCL

## Transaction Control Language

TCL digunakan untuk mengendalikan transaksi database.

```text
COMMIT
ROLLBACK
SAVEPOINT
```

### COMMIT

Menyimpan perubahan transaksi.

```sql
COMMIT;
```

### ROLLBACK

Membatalkan perubahan yang belum di-commit.

```sql
ROLLBACK;
```

### SAVEPOINT

Membuat titik tertentu dalam transaksi.

```sql
SAVEPOINT sebelum_update;
```

Kemudian:

```sql
ROLLBACK TO sebelum_update;
```

---
