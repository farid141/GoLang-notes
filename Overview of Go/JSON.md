# 📦 JSON di Go

## 🔹 Penulisan Struct dengan Tag JSON

Untuk menghubungkan field di struct Go dengan field dalam JSON, gunakan tag `json:"fieldname"` setelah nama field:

```go
type Person struct {
  Name      string `json:"name"`              // Wajib ada
  Age       int    `json:"age,omitempty"`     // Dihilangkan jika nilai nol
  IsMarried bool   `json:"isMarried,omitempty"`
}
```

**Catatan penting:**

* Gunakan tanda backtick (`` ` ``) untuk menuliskan tag.
* Tambahkan `omitempty` untuk menghilangkan field dari hasil JSON jika nilainya kosong atau default (0, "", false).

---

## 🔹 Parsing JSON (Unmarshal)

String JSON ke variable Go

### ✅ Jika struktur data **diketahui**

```go
var people []Person
err := json.Unmarshal([]byte(myJson), &people)
if err != nil {
    log.Fatal(err)
}
```

### ✅ Jika struktur data **tidak diketahui**

```go
var data map[string]interface{}
err := json.Unmarshal([]byte(myJson), &data)
if err != nil {
    log.Fatal(err)
}
```

Atau jika data berupa array campuran:

```go
var arr []interface{}
json.Unmarshal([]byte(myJson), &arr)
```

---

## 🔹 Serialisasi JSON (Marshal)

Mengubah data Go menjadi JSON string:

```go
person := Person{Name: "Alice", Age: 25}
jsonData, err := json.Marshal(person)
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(jsonData))
```

---

## 🔹 Tips Tambahan

* Gunakan `json.Decoder` jika membaca dari `io.Reader` (misalnya file atau HTTP response).
* Hindari typo di tag `json:"..."` karena tidak akan muncul error, tetapi field tidak akan diserialisasi/deserialisasi.
* Gunakan `encoding/json` dari standar library Go.

---

Kalau kamu ingin bagian tambahan seperti penggunaan `json.RawMessage`, nested struct, atau decoding streaming JSON, silakan beri tahu!
