# Penjelasan

Merupakan sebuah code reusable yang dapat digunakan cross project.

# Membuat module

Dalam Go, setiap proyek modern menggunakan Go Modules untuk mengelola dependency dan versi. Biasanya, jika proyek dihosting di GitHub, nama modul disesuaikan dengan URL repo, misalnya:

```bash
go mod init github.com/<username>/<repo-name>
```

## Terintegrasi Github

Karena Go Modules mendukung pengambilan kode langsung dari repositori Git (termasuk GitHub), kita bisa menggunakan GitHub sebagai sumber distribusi kode dan versi.

### Tagging

begitu pula untuk tagging juga bisa digunakan untuk repo github

```bash
git tag v1.0.0
git push origin v1.0.0
```

# Module sebagai tools CLI

Install module as tools(cli) globally

```bash
go install github.com/farid/package@latest
```

Perintah `go install` diatas akan melakukan:

- Mengambil module github.com/farid/package
- Mencari di dalamnya package utama (package main)
- Meng-compile file dari package main menjadi binary
- Menyimpannya ke dalam $GOBIN (biar bisa dijalankan dari mana saja)

## Mengupdate tools CLI

Cukup jalankan `go install` kembali, tapi sebutkan versi terbaru.

# Module sebagai Dependency/Library

Module juga bisa digunakan sebagai library yang akan digunakan pada sebuah projek

## Menambahkan Dependency

Untuk menambahkan dependency ke proyek:

```bash
go get github.com/some/module
```

Setelah itu, `go.mod` akan otomatis diperbarui dengan entri seperti:

```go
require github.com/some/module vX.Y.Z
```

Dependency juga akan dicatat di `go.sum`.

## Mengupdate Dependency

Jika ingin mengubah versi dependency:

1. Edit versi secara manual di file `go.mod`, atau gunakan perintah:
   ```bash
   go get github.com/some/module@vX.Y.Z
   ```
2. Jalankan:
   ```bash
   go mod tidy
   ```
   Untuk merapikan dan memastikan semua dependency digunakan dengan benar.

> Catatan: `go get` **tidak akan otomatis meng-upgrade** ke versi terbaru jika tidak diminta secara eksplisit.

## 🚨 Major Version Upgrade (v2 ke atas)

Dalam Go Modules, versi **v2 dan seterusnya** dianggap **breaking changes** (tidak backward-compatible). Oleh karena itu, perlu perlakuan khusus:

### Di dalam modul (repo asal):

1. **Ganti nama module di `go.mod`**, tambahkan `/v2` di akhir:

   ```go
   module github.com/<username>/<repo-name>/v2
   ```

2. **Buat Git tag versi mayor:**
   ```bash
   git tag v2.0.0
   git push origin v2.0.0
   ```

### Di dalam proyek yang menggunakan modul:

Untuk mengimpor modul versi 2, pastikan impor dan `go.mod` mengacu pada versi yang tepat:

```go
require github.com/<username>/<repo-name>/v2 v2.0.0
```

Dan dalam kode Go yang menggunakan module:

```go
import "github.com/<username>/<repo-name>/v2/..."
```
