
# 📦 Tutorial Membuat E-Commerce Sederhana Menggunakan Flutter

---

## 👤 Identitas
- **Nama:** Moch. Akbar Ramdani  
- **NPM:** 20122010  
- **Mata Kuliah:** Mobile Programming Lanjut  
- **Keterangan:** Ditujukan untuk memenuhi tugas Evaluasi Akhir Semester (EAS)  

---

## 📖 Pengertian

### Apa itu E-Commerce?
**Electronic Commerce (E-Commerce)** adalah proses transaksi jual beli yang dilakukan secara online melalui media elektronik.  
Menurut Laudon & Laudon, e-commerce adalah transaksi business to business yang terjadi dengan perantara jaringan internet.  
🔗 Referensi: [developers.bri.co.id](https://developers.bri.co.id/id/news/ketahui-perkembangan-e-commerce-di-indonesia-pengertian-jenis-dan-manfaatnya)

### Apa itu Flutter?
**Flutter** adalah kerangka kerja sumber terbuka yang dikembangkan oleh Google. Flutter digunakan oleh developer frontend dan full-stack untuk membangun UI aplikasi lintas platform dengan satu basis kode (single codebase).  
🔗 Referensi: [himatro.ee.unila.ac.id](https://himatro.ee.unila.ac.id/apa-itu-flutter/)

---

## 🛠️ Bahan & Tools

1. **VS Code (Visual Studio Code)**  
   Editor kode sumber modern yang ringan, kaya fitur, dan sangat cocok untuk pengembangan Flutter.

2. **Ekstensi Flutter & Dart**  
   - **Flutter**: Memungkinkan Anda membuat, menjalankan, dan mengelola proyek Flutter.
   - **Dart**: Bahasa pemrograman utama untuk membuat aplikasi Flutter.

---

## 🧑‍💻 Langkah-Langkah Pembuatan

### 1. Membuat Folder & Menjalankan VS Code
- Buat folder baru misalnya `e-commerce-sederhana`.
- Buka folder tersebut di VS Code.

### 2. Install Ekstensi
- Buka menu Extensions (Ctrl+Shift+X).
- Install: `Flutter` dan `Dart`.

### 3. Membuat Project Flutter
```bash
flutter create toko_denbaayyy
cd toko_denbaayyy
code .
```

### 4. Menambahkan Dependencies pada `pubspec.yaml`
```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.2.1
  provider: ^6.1.2
```

---

## 5. Struktur Folder dan File

Berikut struktur folder dan file beserta penjelasannya:

```
toko_denbaayyy/
├── lib/
│   ├── main.dart
│   ├── models/
│   │   └── product.dart
│   ├── providers/
│   │   └── cart_provider.dart
│   ├── screens/
│   │   ├── auth/
│   │   │   └── login_screen.dart
│   │   ├── detail/
│   │   │   └── product_detail_screen.dart
│   │   ├── home/
│   │   │   └── home_screen.dart
│   │   └── payment/
│   │       ├── payment_screen.dart
│   │       └── payment_success_screen.dart
│   ├── services/
│   │   └── api_service.dart
│   ├── widgets/
│       ├── cart_dialog.dart
│       └── cart_item_card.dart
```
---

## 📸 Hasil Simulasi

Semua hasil UI dapat dilihat pada folder `screenshoot/` dalam repo ini, meliputi:
- Login
- Home
- Detail Produk
- Keranjang
- Pembayaran
- Pembayaran Berhasil

---

## 🙏 Penutup

Demikian dokumentasi aplikasi **E-Commerce Sederhana Menggunakan Flutter**.  
Jika terdapat kesalahan atau kekurangan, saya mohon maaf.  
Terima kasih telah membaca dan semoga bermanfaat 🎉
