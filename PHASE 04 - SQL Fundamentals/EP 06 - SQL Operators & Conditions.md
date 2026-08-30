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
