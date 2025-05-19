# About

Go dibuat google menggunakan bahasa C, rilis publik 2009. Beberapa aplikasi yang dibuat dengan go: `docker, Kubernetes, Prometheus, cockroachDB`

# Kelebihan Go:

- Bahasa sederhana
- mendukung concurrency (multicore-processor)
- automatic garbage collector (tidak perlu manual)
- mendukung kompilasi cross-platform

# Beberapa konvensi

- Titik koma statement tidak wajib (seperti JS)
- Bukan OOP, melainkan procedural.

# Strongly typed

Go compiler sangat ketat dan akan menimbulkan compile-error jika tidak memenuhi:

- Semua variable harus digunakan `pengecualian dengan blank operator`.
- Assignment variable harus memiliki tipe yang sesuai.
- Fungsi dalam satu projek harus unik `meskipun berbeda file`
