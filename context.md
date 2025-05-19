# Pengertian

`Context` adalah sebuah data yang membawa _value_ dan _sinyal_ seperti **cancel**, **timeout**, atau **deadline** antar proses. Dalam aplikasi web berbasis Go, biasanya context sudah otomatis dibuat saat menerima sebuah request.

### Interface `Context`

```go
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
    Value(key interface{}) interface{}
}
```

## Cara Membuat Context

Ada dua cara membuat context:

1. **Manual**, dengan mengimplementasikan interface `Context` sendiri (jarang digunakan).
2. **Menggunakan package `context` dari standard library**, ini yang umum digunakan.

Beberapa fungsi penting dari package `context`:

- `context.Background()`  
  Mengembalikan context kosong yang tidak memiliki cancel, timeout, maupun value. Biasanya digunakan sebagai root context.
- `context.TODO()`  
  Mengembalikan context kosong. Digunakan saat developer belum memutuskan context apa yang akan dipakai.

# Parent-Child Relationship

- Satu context bisa memiliki banyak child.
- Tapi, setiap context hanya bisa memiliki **satu parent**.

## Cascading Actions

Beberapa aksi pada parent context akan berdampak pada semua child-nya (termasuk grandchild):

- Cancel
- Menambahkan value (hanya berlaku untuk context yang baru dibuat dari parent)

### Immutable

Context bersifat **immutable**. Artinya, kita **tidak bisa mengubah data context yang sudah ada**. Jika ingin menambahkan value atau timeout, yang sebenarnya terjadi adalah membuat context baru (child context) berdasarkan context sebelumnya.

### Menambah value context

```go
contextA = context.Background()
contextB = context.withValue(contextA, "b", "B")

```

Untuk mengecek hirarki pewarisan dari sebuah context:

- `fmt.println(contextB)` output: `context.Background.WithValue(b, B)`
- `fmt.println(contextB.Value("a"))` . Mendapatkan nilai by key. Jika tidak ditemukan, bernilai `<Nil>`

## Penerapan context ke Go-routine

Biasanya context digunakan untuk membatalkan goroutine dengan sinyal cancel dari context.

```go

func CreateCounter(ctx context.Context) chan int {
	destination := make(chan int)

	go func() {
		defer close(destination)
		counter := 1
		for {
			select {
			case <-ctx.Done():
				return
			default:
				destination <- counter
				counter++
				time.Sleep(1 * time.Second) // simulasi slow
			}
		}
	}()

	return destination
}

func TestContextWithCancel(t *testing.T) {
	parent := context.Background()
	ctx, cancel := context.WithCancel(parent)

	destination := CreateCounter(ctx)

	fmt.Println("Total Goroutine", runtime.NumGoroutine()) //10

	for n := range destination {
		fmt.Println("Counter", n)
		if n == 10 {
			break
		}
	}

	cancel() // mengirim sinyal cancel ke context

	time.Sleep(2 * time.Second)

	fmt.Println("Total Goroutine", runtime.NumGoroutine())
}
```
