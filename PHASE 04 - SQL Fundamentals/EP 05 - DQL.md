# 04.05 — DQL / SELECT

Ini akan menjadi **bagian terbesar dari SQL Fundamentals**, karena kemampuan mengambil dan menganalisis data merupakan inti penggunaan SQL.

Struktur dasar:

```sql
SELECT column
FROM table;
```

Contoh:

```sql
SELECT nama
FROM mahasiswa;
```

Beberapa komponen penting:

```text
SELECT
FROM
WHERE
DISTINCT
ORDER BY
FETCH
OFFSET
```

Kemudian pada phase berikutnya kita akan memperluasnya dengan:

```text
GROUP BY
HAVING
JOIN
SUBQUERY
CTE
WINDOW FUNCTION
```

---

## SELECT

Mengambil data.

```sql
SELECT *
FROM mahasiswa;
```

Lebih baik dalam query production untuk menyebutkan kolom yang memang diperlukan:

```sql
SELECT nim, nama, umur
FROM mahasiswa;
```

---

## FROM

Menentukan sumber data.

```sql
SELECT nama
FROM mahasiswa;
```

`mahasiswa` adalah sumber data.

---

## WHERE

Memfilter row berdasarkan kondisi.

```sql
SELECT *
FROM mahasiswa
WHERE umur >= 20;
```

Oracle SQL conditions digunakan untuk membentuk kondisi yang menghasilkan `TRUE`, `FALSE`, atau kondisi unknown. ([Oracle Docs][3])

---

## DISTINCT

Menghilangkan duplikasi hasil.

```sql
SELECT DISTINCT umur
FROM mahasiswa;
```

Misalnya data:

```text
20
20
21
22
22
```

hasil:

```text
20
21
22
```

---

## ORDER BY

Mengurutkan hasil.

Ascending:

```sql
SELECT *
FROM mahasiswa
ORDER BY umur ASC;
```

Descending:

```sql
SELECT *
FROM mahasiswa
ORDER BY umur DESC;
```

`ASC` adalah default.

---

## FETCH

Membatasi jumlah row yang dikembalikan.

```sql
SELECT *
FROM mahasiswa
FETCH FIRST 5 ROWS ONLY;
```

---

## OFFSET

Melewati sejumlah row sebelum mengambil hasil.

```sql
SELECT *
FROM mahasiswa
OFFSET 5 ROWS
FETCH NEXT 5 ROWS ONLY;
```

Konsep ini nantinya sangat berguna ketika membahas **pagination** pada aplikasi.

---
