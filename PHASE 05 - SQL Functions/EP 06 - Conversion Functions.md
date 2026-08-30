Conversion functions digunakan untuk mengubah data dari satu tipe ke tipe lainnya.

Tiga function utama:

```text
TO_CHAR()
TO_DATE()
TO_NUMBER()
```

---

## TO_CHAR()

Mengubah nilai menjadi karakter.

### Number

```sql
SELECT TO_CHAR(
    1250000,
    '999G999G999'
)
FROM dual;
```

### Date

```sql
SELECT TO_CHAR(
    SYSDATE,
    'DD-MM-YYYY'
)
FROM dual;
```

Format yang sering digunakan:

```text
DD      → Day
MM      → Month
YYYY    → Year
HH24    → Hour
MI      → Minute
SS      → Second
```

Contoh:

```sql
SELECT TO_CHAR(
    SYSDATE,
    'DD MONTH YYYY'
)
FROM dual;
```

---

## TO_DATE()

Mengubah string menjadi tipe `DATE`.

```sql
SELECT TO_DATE(
    '30-08-2026',
    'DD-MM-YYYY'
)
FROM dual;
```

Format input harus sesuai dengan format yang diberikan.

---

## TO_NUMBER()

Mengubah string menjadi angka.

```sql
SELECT TO_NUMBER('1000')
FROM dual;
```

---
