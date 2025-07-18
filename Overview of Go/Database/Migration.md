# Migration

Digunakan untuk menyimpan aktifitas perubahan database dalam file migrasi. Merupakan tools CLI yang bisa diakses secara global.

## Golang Migrate

github.com/golang-migrate/migrate. Mendukung banyak jenis database

### Membuat migration

```bash
migrate create -ext mysql -dir db/migrations nama_file_migrasi [n]
```

Dari perintah tersebut akan terbuat file sql migrasi. Bisa kita tambahkan DDL/DML untuk mengubah database, `n` (opsional) merupakan jumlah file terbaru yang akan dimigrasi.

Jika kita menjalankan up dan ada beberapa file, tiap file akan diberi urutan sendiri (bukan batch).

### Menjalankan migration

```bash
migrate -database db_url -path db/migrations up
```

state migration akan disimpan pada table `schema_migrations`. Sehingga apabila kita punya file migration baru, tidak akan menjalankan migrasi yang sudah dilakukan sebelumnya

### Rollback migration

```bash
migrate -database db_url -path db/migrations down [n]
```

state migration akan disimpan pada table `schema_migrations`. Sehingga apabila kita punya file migration baru, tidak akan menjalankan migrasi yang sudah dilakukan sebelumnya.
