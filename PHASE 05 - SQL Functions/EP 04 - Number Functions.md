Number functions digunakan untuk melakukan operasi terhadap **angka**.

---

## ROUND()

Membulatkan angka.

```sql
SELECT ROUND(125.678, 2)
FROM dual;
```

Hasil:

```text
125.68
```

Tanpa jumlah digit:

```sql
SELECT ROUND(125.678)
FROM dual;
```

Hasil:

```text
126
```

---

## TRUNC()

Memotong angka tanpa melakukan pembulatan.

```sql
SELECT TRUNC(125.678, 2)
FROM dual;
```

Hasil:

```text
125.67
```

Perbedaan penting:

```text
ROUND(125.678, 2) → 125.68
TRUNC(125.678, 2) → 125.67
```

---

## CEIL()

Menghasilkan bilangan bulat terkecil yang lebih besar atau sama dengan angka tersebut.

```sql
SELECT CEIL(10.3)
FROM dual;
```

Hasil:

```text
11
```

---

## FLOOR()

Menghasilkan bilangan bulat terbesar yang lebih kecil atau sama dengan angka tersebut.

```sql
SELECT FLOOR(10.9)
FROM dual;
```

Hasil:

```text
10
```

---

## MOD()

Menghasilkan sisa hasil pembagian.

```sql
SELECT MOD(10, 3)
FROM dual;
```

Hasil:

```text
1
```

Contoh penggunaan:

Menentukan bilangan genap:

```sql
SELECT *
FROM mahasiswa
WHERE MOD(nim, 2) = 0;
```

---

## ABS()

Menghasilkan nilai absolut.

```sql
SELECT ABS(-100)
FROM dual;
```

Hasil:

```text
100
```

---
