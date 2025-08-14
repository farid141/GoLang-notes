# Gofiber

Merupakan framework web untuk membuat webserver

## Routing

### Context

Merupakan parameter handler dan representasi request yang masuk ke webserver
<https://docs.gofiber.io/api/ctx/>

```go
// Mengambil query param
c.Query("name")

// mengembalikan response
return c.SendString("Hello, World!")
```

### Handler

Merupakan fungsi callback router (param ke 2) yang menangani request dan mengembalikan response

```go
// Simple GET handler
app.Get("/api/list", func(c *fiber.Ctx) error {
  return c.SendString("I'm a GET request!")
})

// Simple POST handler
app.Post("/api/register", func(c *fiber.Ctx) error {
  return c.SendString("I'm a POST request!")
})
```

### Middleware

Digunakan untuk menyaring request (dapat/tidak) diproses. Path yang cocok akan masuk ke callback `Use` sebelum ke route handler `return c.Next()`.

```go
// Match any request
app.Use(func(c *fiber.Ctx) error {
    return c.Next()
})

// Match request starting with /api
app.Use("/api", func(c *fiber.Ctx) error {
    return c.Next()
})

// Match requests starting with /api or /home (multiple-prefix support)
app.Use([]string{"/api", "/home"}, func(c *fiber.Ctx) error {
    return c.Next()
})

// Attach multiple handlers 
app.Use("/api", func(c *fiber.Ctx) error {
  c.Set("X-Custom-Header", random.String(32))
    return c.Next()
}, func(c *fiber.Ctx) error {
    return c.Next()
})
```

## Validator

Berisi validasi ketika response masuk dan akan membuat response error ketika validasi gagal
