# Toko Buku REST API

REST API untuk pengelolaan katalog buku dan kategori menggunakan **Laravel**. Proyek ini dibuat untuk memenuhi praktikum **Teknologi Web Service (Pertemuan 1 / Pekan 9)**, mencakup operasi CRUD penuh, relasi database, validasi input, penanganan error berformat JSON, serta dukungan kolom `isbn` unik pada data buku.

---

## Teknologi yang Digunakan

- **PHP** 8.3+
- **Laravel** 13
- **Database** MySQL (Laragon) / SQLite
- **Postman** (Pengujian & Dokumentasi Koleksi API)

---

## Cara Instalasi & Menjalankan Proyek

1. **Clone repository ini:**
   ```bash
   git clone [https://github.com/Milhamsaf/toko-buku-api.git](https://github.com/Milhamsaf/toko-buku-api.git)
   cd toko-buku-api
   ```

2. **Install dependensi Composer:**
   ```bash
   composer install
   ```

3. **Salin file konfigurasi `.env` dan generate application key:**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

4. **Konfigurasi Database di file `.env`:**
   Sesuaikan koneksi database (contoh menggunakan MySQL Laragon):
   ```env
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=toko_buku
   DB_USERNAME=root
   DB_PASSWORD=
   ```
   *(Pastikan database `toko_buku` sudah dibuat terlebih dahulu di MySQL).*

5. **Jalankan migrasi tabel:**
   ```bash
   php artisan migrate
   ```

6. **Jalankan server lokal:**
   ```bash
   php artisan serve
   ```
   API dapat diakses melalui `http://127.0.0.1:8000/api`.

---

## Aturan Umum API

- **Header Wajib:** Setiap request harus menyertakan header `Accept: application/json`.
- **Format Respon:** Data sukses dibungkus dalam kunci `"data"`, sedangkan pesan kesalahan dikembalikan dalam kunci `"message"` dan `"errors"` (untuk validasi).
- **HTTP Status Code:**
  - `200 OK` — Permintaan berhasil
  - `201 Created` — Data baru berhasil dibuat
  - `204 No Content` — Data berhasil dihapus
  - `404 Not Found` — Data atau endpoint tidak ditemukan
  - `409 Conflict` — Kategori tidak dapat dihapus karena masih digunakan oleh data buku
  - `422 Unprocessable Content` — Validasi input gagal

---

## Daftar Endpoint

### 1. Kategori (`/api/categories`)

| Method | Endpoint | Keterangan | Status Sukses |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/categories` | Menampilkan semua kategori beserta jumlah buku | `200` |
| `POST` | `/api/categories` | Menambahkan kategori baru (`name` unik) | `201` |
| `GET` | `/api/categories/{id}` | Menampilkan detail kategori beserta daftar bukunya | `200` |
| `PUT` | `/api/categories/{id}` | Memperbarui nama kategori | `200` |
| `DELETE` | `/api/categories/{id}` | Menghapus kategori (ditolak `409` jika masih memiliki buku) | `204` |

### 2. Buku (`/api/books`)

| Method | Endpoint | Keterangan | Status Sukses |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/books` | Menampilkan semua daftar buku beserta kategorinya | `200` |
| `POST` | `/api/books` | Menambahkan buku baru (`isbn` bersifat opsional dan unik) | `201` |
| `GET` | `/api/books/{id}` | Menampilkan detail satu buku | `200` |
| `PUT` | `/api/books/{id}` | Memperbarui sebagian atau seluruh data buku | `200` |
| `DELETE` | `/api/books/{id}` | Menghapus data buku | `204` |

---

## Contoh Request & Response

### Menambah Buku (`POST /api/books`)

**Request Body:**
```json
{
    "category_id": 1,
    "title": "Bumi",
    "author": "Tere Liye",
    "isbn": "978-602-03-0112-9",
    "price": 95000,
    "stock": 10,
    "description": "Novel petualangan dunia paralel"
}
```

**Response (`201 Created`):**
```json
{
    "data": {
        "id": 1,
        "title": "Bumi",
        "author": "Tere Liye",
        "isbn": "978-602-03-0112-9",
        "price": 95000,
        "stock": 10,
        "description": "Novel petualangan dunia paralel",
        "category": {
            "id": 1,
            "name": "Novel"
        },
        "created_at": "2026-11-13T04:30:00.000000Z"
    }
}
```