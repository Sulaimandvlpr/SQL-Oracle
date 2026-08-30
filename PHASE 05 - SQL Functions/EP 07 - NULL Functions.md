`NULL` menunjukkan bahwa sebuah nilai **tidak tersedia atau tidak diketahui**.

Oracle memiliki beberapa function untuk menangani `NULL`.

```text
NVL()
NVL2()
NULLIF()
COALESCE()
```

---

## NVL()

Menggantikan `NULL` dengan nilai tertentu.

Syntax:

```text
NVL(expression, replacement)
```

Contoh:

```sql
SELECT NVL(commission_pct, 0)
FROM employees;
```

Jika:

```text
commission_pct = NULL
```

hasilnya:

```text
0
```

---

## NVL2()

Menghasilkan nilai berbeda berdasarkan apakah expression bernilai `NULL` atau tidak.

Syntax:

```text
NVL2(expression, value_if_not_null, value_if_null)
```

Contoh:

```sql
SELECT NVL2(
    commission_pct,
    'Has Commission',
    'No Commission'
)
FROM employees;
```

---

## NULLIF()

Menghasilkan `NULL` apabila dua expression memiliki nilai yang sama.

```sql
SELECT NULLIF(10, 10)
FROM dual;
```

Hasil:

```text
NULL
```

Jika berbeda:

```sql
SELECT NULLIF(10, 20)
FROM dual;
```

hasil:

```text
10
```

---

## COALESCE()

Mengembalikan expression pertama yang **tidak bernilai `NULL`**.

```sql
SELECT COALESCE(
    NULL,
    NULL,
    'Oracle',
    'SQL'
)
FROM dual;
```

Hasil:

```text
Oracle
```

Contoh penggunaan:

```sql
SELECT COALESCE(
    mobile_phone,
    home_phone,
    office_phone,
    'No Phone'
)
FROM employees;
```

Oracle akan mencari nilai pertama yang tidak `NULL`.

---
