# Penjelasan

Dalam project Go, semua file yang berakhiran \_test.go dianggap sebagai file testing

1. Buat file testing `[file_name]_test.go`
2. Buat fungsi testing di dalamnya

```go
func TestLoginHandler(t *testing.T) {
  form := strings.NewReader("username=budi")
  req := httptest.NewRequest("POST", "/login", form)
  req.Header.Set("Content-Type", "application/x-www-form-urlencoded")

  rr := httptest.NewRecorder()

  Login(rr, req) // panggil fungsi asli

  if rr.Code != http.StatusOK {
    t.Errorf("Expected status 200, got %v", rr.Code)
  }

  expected := "Selamat datang, budi"
  if rr.Body.String() != expected {
    t.Errorf("Expected body '%s', got '%s'", expected, rr.Body.String())
  }
}
```

3. Jalankan testing `go test -v`

## Menampilkan test pada file html

> go test -coverprofile=coverage.out && go tool cover -html=coverage.out

## Menjalankan fungsi test spesifik

> go test -v -run=FuncTest
