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
 
