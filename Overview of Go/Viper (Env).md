# Viper

Ketika sebuah aplikasi golang dibuild untuk production, hasil production adalah code yang telah dicompile sehingga kita tidak bisa mengatur `environment variable`.

Mendukung file konfigurasi:

- json
- yaml
- env
- properties (Java)

```bash
go get github.com/spf13/viper
```

```go
var config *viper.Viper = viper.New()

config.SetConfigName("config")
config.SetConfigType("json")

// atau langsung file
config.SetConfigFile("config.json")

config.AddConfigPath(".")  //direktori config.json

config.ReadConfig() // dipanggil sekali
config.GetString("database.host")
config.GetInt("database.port")
```

## Environment Variable

Merupakan variable yang tersimpan dalam OS, bukan file env. Kita dapat membaca konfigurasi env variable menggunakan viper (tidak terbaca secara default)

```go
config.AutomaticEnv() //panggil ini
```
