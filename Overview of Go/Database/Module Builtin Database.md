# Database

Dalam berinteraksi dengan database, dibutuhkan modul built-in `database/sql` dan juga `driver SQL untuk Go`

## Download driver

`go get -u github.com/go-sql-driver/mysql`

1. Import driver untuk panggil init `import (_ "module name")`

## Database Pooling

Sudah diatur Go dibalik layar, merupakan sebuah kumpulan koneksi ke database. Bisa diset min dan max untuk koneksi yang aktif

Beberapa pengaturan yang bisa digunakan:

- setMaxIdleConns(number)
- setMaxOpenConns(number)
- setConnMaxIdleTime(duration)
- setConnMaxLifetime(duration)

## Eksekusi Perintah

```go
 // query non-select
_, err := db.ExecContext(context, sql, params)

// query select
rows, err := db.QueryContext(context, sql, params)
for rows.Next() {
    var id, name string
    err := rows.Scan(&id, &name)
    if(err != nil){
        panic(err)
    }
}
defer rows.Close() // rows harus ditutup
```

Dalam query select, untuk menyimpan hasil ke variable, sesuaikan dengan tipe data Go yang sesuai. untuk timestamp, masukkan ke time.Time

### Menangani data nullable

```go
var email sql.NullString
err := rows.Scan(&email)

// mengecek email berupa string atau null
if email.Valid{
    // proses string
}
```

### Parameter Query

Sebelumnya kita menggunakan `QueryContext` dan `ExecContext` dengan string secara langsung. Hal ini memiliki resiko kerentanan `SQL Injection`. Dimana string yang diproses ternyata mengandung karakter yang dapat mengubah query. Untuk mengatasi itu, kita dapat gunakan params yang berupa variadic daripada format string.

Gunakan karakter `?` dalam sql string, akan direplace oleh variadic yang dikirim setelahnya

```go
sqlString := "SELECT username FROM USER WHERE username = ? AND password = ?"
rows, err := db.QueryContext(context, sqlString, "farid", "secret")
```

### Mengambil id setelah insert

```go
result, err := db.QueryContext(context, sqlString, "farid", "secret")
insertId, err := result.LastInsertId()
```

### Prepare Statement

Cara efisien untuk batch operation dimana satu query digunakan berulang ulang untuk data yang berbeda, sehingga koneksi lebih efisien

```go
sqlString := "INSERT INTO comments (email, comment) VALUES (?, ?)"
stmt, err := db.PrepareContext(ctx, sqlString)
if err != nil
    panic

defer stmt.Close() // close statement

for i:=0; i<10; i++{
    email := "farid" + strconv.Itoa(i) + "@gmail.com"
    comment := "comment" + strconv.Itoa(i)
    result, err := stmt.ExecContext(ctx, email, comment) //gunakan stmt
}
```

### Transaction

Untuk membuat operasi SQL bisa dirollback kalau gagal

```go
tx, err := db.Begin()
if err != nil
    panic(err)

// do query

err := tx.Commit();
if err != nil
    panic(err)

```
