Oracle memiliki beberapa tipe data untuk menyimpan informasi tanggal dan waktu, seperti `DATE` dan `TIMESTAMP`.

Function yang umum digunakan:

```text
SYSDATE
SYSTIMESTAMP
ADD_MONTHS()
MONTHS_BETWEEN()
NEXT_DAY()
LAST_DAY()
```

---

## SYSDATE

Menghasilkan tanggal dan waktu dari database server.

```sql
SELECT SYSDATE
FROM dual;
```

---

## SYSTIMESTAMP

Menghasilkan timestamp dengan informasi waktu yang lebih detail dibandingkan `DATE`.

```sql
SELECT SYSTIMESTAMP
FROM dual;
```

---

## ADD_MONTHS()

Menambahkan sejumlah bulan ke sebuah tanggal.

```sql
SELECT ADD_MONTHS(
    DATE '2026-01-15',
    3
)
FROM dual;
```

Hasil:

```text
15-APR-2026
```

---

## MONTHS_BETWEEN()

Menghitung jumlah bulan antara dua tanggal.

```sql
SELECT MONTHS_BETWEEN(
    DATE '2026-07-01',
    DATE '2026-01-01'
)
FROM dual;
```

---

## NEXT_DAY()

Mencari tanggal berikutnya berdasarkan nama hari tertentu.

```sql
SELECT NEXT_DAY(
    DATE '2026-08-30',
    'MONDAY'
)
FROM dual;
```

---

## LAST_DAY()

Menghasilkan tanggal terakhir pada bulan tertentu.

```sql
SELECT LAST_DAY(
    DATE '2026-08-15'
)
FROM dual;
```

---

## Date Arithmetic

Oracle memungkinkan operasi aritmatika terhadap `DATE`.

Contoh:

```sql
SELECT SYSDATE + 7
FROM dual;
```

Artinya menambahkan **7 hari**.

```sql
SELECT SYSDATE - 7
FROM dual;
```

Artinya mengurangi **7 hari**.

Perbedaan dua tanggal juga dapat menghasilkan jumlah hari.

```sql
SELECT DATE '2026-08-30' - DATE '2026-08-20'
FROM dual;
```

Hasil:

```text
10
```

---
