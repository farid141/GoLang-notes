# Error

Golang memiliki interface `error` yang digunakan sebagai contract untuk membuat error.

```go
type error interface{
    Error() string
}
```

Untuk memenuhinya bisa buat interface/type yang memiliki method `Error()` dan var `Message`. Atau gunakan package errors

## Membuat error

Sudah disediakan package `errors` untuk membuat nilai bertipe `error`. Didalamnya ada fungsi `New()` yang mengembalikan `address ke instance struct` yang dibuat.

```go
error *errors.Error = errors.New("message")
```

## Custom error

1. buat type struct yang mengimplementasikan Error() dan Message

    ```go
    type validationError struct {
        Message string
    }
    
    func (v *validationError) Error() string{
        return v.Message 
    }

    //buat dan kembalikan error
    &validationError{Message: "asdasd"}

    // 2. cek error (type assertion)
    validationErr, ok := err.(*validationErr)
    ```
