# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: Muhammad Haidh Hilmi Al Fikri 
**NIM**: 240202873 
**Modul yang Dikerjakan**:  
**Modul 3 – Manajemen Memori Tingkat Lanjut (Copy-on-Write dan Shared Memory)**

---

## 📌 Deskripsi Singkat Tugas

Modul ini membahas dua topik utama dalam manajemen memori:

1. **Copy-on-Write Fork (CoW):**  
   Meningkatkan efisiensi `fork()` dengan menunda penyalinan memori sampai proses menulis ke halaman tersebut (lazy copy). Halaman disalin hanya jika terjadi **page fault** akibat modifikasi. Refcount digunakan untuk memantau jumlah referensi ke setiap halaman fisik.

2. **Shared Memory (ala System V):**  
   Implementasi `shmget()` dan `shmrelease()` untuk berbagi satu halaman memori antar proses. Halaman diakses dari alamat tinggi (`USERTOP`) dan memiliki refcount.

---

## 🛠️ Rincian Implementasi

* Menambahkan array `ref_count[]` untuk mencatat referensi ke tiap halaman fisik
* Membuat fungsi `incref()` dan `decref()` di `vm.c`
* Menambahkan flag baru `PTE_COW` di `mmu.h`
* Membuat fungsi `cowuvm()` untuk menggantikan `copyuvm()` saat `fork()`
* Menangani `page fault` COW di `trap.c`
* Menambahkan struktur dan manajemen `shmtab[]` di `vm.c`
* Menambahkan syscall `shmget()` dan `shmrelease()` di `sysproc.c`
* Registrasi syscall baru ke `syscall.h`, `syscall.c`, `user.h`, `usys.S`

---

## ✅ Uji Fungsionalitas

### cowtest.c  
Menguji fork dengan copy-on-write. Proses anak mengubah isi memori sehingga terjadi page fault dan salinan dibuat.

### shmtest.c  
Menguji berbagi memori antar proses menggunakan `shmget()` dan `shmrelease()`.

---

## 📷 Hasil Uji

### 📍 Output `cowtest`:

```
Child sees: Y
Parent sees: X
```

### 📍 Output `shmtest`:

```
Child reads: A
Parent reads: B
```
### Screenshot

![hasil randomtest dan chmodtest ](./Screenshot/modul3.png)
---

## ⚠️ Kendala yang Dihadapi

* Salah dalam menentukan alamat saat page fault menyebabkan panic
* Tidak semua refcount diupdate → menyebabkan double free
* Salah pemetaan `mappages()` menyebabkan halaman tidak bisa diakses
* Harus hati-hati menangani mapping `USERTOP` agar tidak bentrok

---

## 📚 Referensi

* [MIT xv6 Book (x86)](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* xv6-public: `vm.c`, `proc.c`, `trap.c`, `syscall.c`
* Diskusi forum mahasiswa, debugging bersama asisten

---

## 📝 Kesimpulan

Dengan menyelesaikan modul ini, Anda telah:

* Mengimplementasikan **Copy-on-Write Fork** untuk meningkatkan efisiensi proses `fork()`
* Mengembangkan fitur **Shared Memory** antar proses yang efisien dan aman
* Memahami konsep **lazy memory allocation**, **page fault handler**, dan **reference counting**
