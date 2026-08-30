## Data Control Language

DCL berkaitan dengan **privilege dan hak akses**.

Perintah utama:

```text
GRANT
REVOKE
```

### GRANT

Memberikan privilege.

```sql
GRANT SELECT
ON mahasiswa
TO user1;
```

### REVOKE

Mencabut privilege.

```sql
REVOKE SELECT
ON mahasiswa
FROM user1;
```

Bagian DCL nantinya akan kita perdalam ketika masuk ke **Database Security**, karena hak akses database tidak cukup dipahami hanya dari syntax `GRANT` dan `REVOKE`.

---
