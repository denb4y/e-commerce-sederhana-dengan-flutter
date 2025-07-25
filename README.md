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

### 1. **VS Code (Visual Studio Code)**  
   Editor kode sumber modern yang ringan, kaya fitur, dan sangat cocok untuk pengembangan Flutter.

### 2. **Ekstensi Flutter & Dart**  
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

### 5. Struktur Folder dan File
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
### 6. Tahapan Coding
- `main.dart`
```
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:toko_denbaayyy/providers/cart_provider.dart';
import 'package:toko_denbaayyy/screens/auth/login_screen.dart';

void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => CartProvider(),
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Toko Denbaayyy',
      debugShowCheckedModeBanner: false, 
      theme: ThemeData(
        primarySwatch: Colors.orange,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: const LoginScreen(), 
    );
  }
}
```
#### Penjelasannya:
##### File main.dart merupakan titik awal dari aplikasi Flutter. Di dalamnya, kita menggunakan runApp() untuk menjalankan widget utama aplikasi yaitu MyApp. Namun sebelum itu, widget tersebut dibungkus oleh ChangeNotifierProvider yang berasal dari package provider, di mana CartProvider disediakan sebagai state global untuk mengelola data keranjang belanja. Ini sangat penting karena nanti banyak bagian aplikasi yang membutuhkan akses ke data keranjang. Kelas MyApp sendiri adalah turunan dari StatelessWidget, yang di dalamnya terdapat MaterialApp sebagai pembungkus utama UI aplikasi. Properti title memberi nama aplikasi, debugShowCheckedModeBanner: false menyembunyikan label debug, theme mengatur warna utama (oranye), dan home menunjuk ke halaman pertama yaitu LoginScreen.
- `login_screen.dart`
```
import 'package:flutter/material.dart';
import 'package:toko_denbaayyy/services/api_service.dart';
import 'package:toko_denbaayyy/screens/home/home_screen.dart'; 

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final TextEditingController _usernameController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();
  final ApiService _apiService = ApiService();
  bool _isLoading = false;

  void _login() async {
    setState(() {
      _isLoading = true;
    });

    try {
      final response = await _apiService.login(
        _usernameController.text,
        _passwordController.text,
      );
      // Jika login berhasil
      print('Login berhasil: ${response['token']}');
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Login berhasil!')),
      );

      // Navigasi ke HomeScreen setelah login berhasil
      Navigator.pushReplacement(
        context,
        MaterialPageRoute(builder: (context) => const HomeScreen()),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Login gagal: ${e.toString()}')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('b4y'),
        backgroundColor: Colors.orange,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'Selamat Datang di Toko Denbaayyy',
              style: TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
                color: Colors.deepOrange,
              ),
              textAlign: TextAlign.center,
            ),
            const SizedBox(height: 40),
            TextField(
              controller: _usernameController,
              decoration: InputDecoration(
                labelText: 'Username', 
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(10),
                ),
                prefixIcon: const Icon(Icons.person),
              ),
            ),
            const SizedBox(height: 20),
            TextField(
              controller: _passwordController,
              obscureText: true,
              decoration: InputDecoration(
                labelText: 'Password',
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(10),
                ),
                prefixIcon: const Icon(Icons.lock),
              ),
            ),
            const SizedBox(height: 30),
            _isLoading
                ? const CircularProgressIndicator()
                : ElevatedButton(
                    onPressed: _login,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.orange, 
                      foregroundColor: Colors.white, 
                      padding: const EdgeInsets.symmetric(horizontal: 50, vertical: 15),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(10),
                      ),
                    ),
                    child: const Text(
                      'Login',
                      style: TextStyle(fontSize: 18),
                    ),
                  ),
          ],
        ),
      ),
    );
  }
}
```
#### Penjelasannya:
##### Pada file login_screen.dart, kita membuat halaman login menggunakan StatefulWidget karena membutuhkan state perubahan saat proses login berlangsung (misalnya loading). Di bagian atas kelas _LoginScreenState, kita membuat dua buah controller untuk menangkap input username dan password. Kemudian, kita inisialisasi ApiService yang akan digunakan untuk memanggil API login. Variabel _isLoading digunakan untuk menandakan proses sedang berlangsung. Fungsi _login() berisi logika proses login: saat tombol ditekan, status loading berubah menjadi true, kemudian API dipanggil dengan data dari form. Jika berhasil, akan ditampilkan snackbar “Login Berhasil” dan pengguna diarahkan ke halaman HomeScreen. Jika gagal, akan muncul pesan error. Di dalam method build(), kita menampilkan AppBar, form input, dan tombol login. Tombol akan berubah menjadi CircularProgressIndicator saat loading. Form input menggunakan TextField dengan ikon dan border melengkung untuk tampilan menarik dan user-friendly.
- `home_screen.dart`
```
import 'package:flutter/material.dart';
import 'package:toko_denbaayyy/models/product.dart';
import 'package:toko_denbaayyy/services/api_service.dart';
import 'package:toko_denbaayyy/screens/detail/product_detail_screen.dart';
import 'package:provider/provider.dart';
import 'package:toko_denbaayyy/providers/cart_provider.dart';
import 'package:toko_denbaayyy/widgets/cart_dialog.dart'; 

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  final ApiService _apiService = ApiService();
  late Future<List<Product>> _foodProductsFuture;

  @override
  void initState() {
    super.initState();
    _foodProductsFuture = _apiService.getFoodProducts();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Beranda'),
        backgroundColor: Colors.orange,
        actions: [
          IconButton(
            icon: const Icon(Icons.shopping_cart),
            onPressed: () {
              showDialog(
                context: context,
                builder: (context) => const CartDialog(), 
              );
            },
          ),
        ],
      ),
      body: FutureBuilder<List<Product>>(
        future: _foodProductsFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('Error: ${snapshot.error}'));
          } else if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return const Center(child: Text('Tidak ada produk makanan ditemukan.'));
          } else {
            final List<Product> foodProducts = snapshot.data!;
            return Padding(
              padding: const EdgeInsets.all(8.0),
              child: GridView.builder(
                gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                  crossAxisCount: 3,
                  crossAxisSpacing: 8.0,
                  mainAxisSpacing: 8.0,
                  childAspectRatio: 0.65,
                ),
                itemCount: foodProducts.length,
                itemBuilder: (context, index) {
                  final product = foodProducts[index];
                  return GestureDetector(
                    onTap: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(
                          builder: (context) => ProductDetailScreen(productId: product.id),
                        ),
                      );
                    },
                    child: Card(
                      elevation: 2,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.stretch,
                        children: [
                          Expanded(
                            child: ClipRRect(
                              borderRadius: const BorderRadius.vertical(top: Radius.circular(8)),
                              child: Image.network(
                                product.thumbnail,
                                fit: BoxFit.cover,
                                errorBuilder: (context, error, stackTrace) => const Center(child: Icon(Icons.broken_image, size: 30, color: Colors.grey)),
                              ),
                            ),
                          ),
                          Padding(
                            padding: const EdgeInsets.all(6.0),
                            child: Column(
                              crossAxisAlignment: CrossAxisAlignment.start,
                              children: [
                                Text(
                                  product.title,
                                  style: const TextStyle(
                                    fontWeight: FontWeight.bold,
                                    fontSize: 12,
                                  ),
                                  maxLines: 2,
                                  overflow: TextOverflow.ellipsis,
                                ),
                                const SizedBox(height: 3),
                                Text(
                                  'Rp ${(product.price * 10000).toStringAsFixed(0)}',
                                  style: const TextStyle(
                                    color: Colors.green,
                                    fontWeight: FontWeight.bold,
                                    fontSize: 11,
                                  ),
                                ),
                                const SizedBox(height: 5),
                                Align(
                                  alignment: Alignment.bottomRight,
                                  child: Consumer<CartProvider>(
                                    builder: (context, cartProvider, child) {
                                      return IconButton(
                                        icon: Icon(
                                          cartProvider.isProductInCart(product) ? Icons.check_circle : Icons.add_shopping_cart,
                                          color: cartProvider.isProductInCart(product) ? Colors.green : Colors.orange,
                                          size: 18,
                                        ),
                                        onPressed: () {
                                          if (!cartProvider.isProductInCart(product)) {
                                            cartProvider.addItem(product);
                                            ScaffoldMessenger.of(context).showSnackBar(
                                              SnackBar(content: Text('${product.title} ditambahkan ke keranjang')),
                                            );
                                          } else {
                                            ScaffoldMessenger.of(context).showSnackBar(
                                              SnackBar(content: Text('${product.title} sudah ada di keranjang')),
                                            );
                                          }
                                        },
                                      );
                                    },
                                  ),
                                ),
                              ],
                            ),
                          ),
                        ],
                      ),
                    ),
                  );
                },
              ),
            );
          }
        },
      ),
    );
  }
}
```
#### Penjelasannya:
##### File home_screen.dart berfungsi sebagai halaman utama setelah login berhasil. Di dalam initState, kita mengambil daftar produk dari API menggunakan method getFoodProducts() dan menyimpannya dalam Future<List<Product>>. Kemudian, di dalam widget build, kita menggunakan FutureBuilder untuk menunggu data produk dimuat. Jika loading, akan tampil CircularProgressIndicator, jika error akan muncul pesan error, dan jika berhasil maka ditampilkan dalam bentuk GridView.builder. Setiap item produk ditampilkan dalam Card dengan gambar, nama produk, harga, dan tombol untuk menambahkan ke keranjang. Jika produk sudah ada di keranjang, tombol berubah menjadi ikon centang. Selain itu, terdapat ikon keranjang di AppBar yang jika ditekan akan membuka CartDialog.
- `product_detail_screen.dart`
```
import 'package:flutter/material.dart';
import 'package:toko_denbaayyy/models/product.dart';
import 'package:toko_denbaayyy/services/api_service.dart';
import 'package:provider/provider.dart';
import 'package:toko_denbaayyy/providers/cart_provider.dart';

class ProductDetailScreen extends StatefulWidget {
  final int productId;

  const ProductDetailScreen({super.key, required this.productId});

  @override
  State<ProductDetailScreen> createState() => _ProductDetailScreenState();
}

class _ProductDetailScreenState extends State<ProductDetailScreen> {
  final ApiService _apiService = ApiService();
  late Future<Product> _productDetailFuture;

  @override
  void initState() {
    super.initState();
    _productDetailFuture = _apiService.getProductDetail(widget.productId);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Detail Produk'),
        backgroundColor: Colors.orange,
      ),
      body: FutureBuilder<Product>(
        future: _productDetailFuture,
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return const Center(child: CircularProgressIndicator());
          } else if (snapshot.hasError) {
            return Center(child: Text('Error: ${snapshot.error}'));
          } else if (!snapshot.hasData) {
            return const Center(child: Text('Produk tidak ditemukan.'));
          } else {
            final product = snapshot.data!;
            return SingleChildScrollView(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Center(
                    child: ClipRRect(
                      borderRadius: BorderRadius.circular(15),
                      child: Image.network(
                        product.thumbnail,
                        height: 180, 
                        width: MediaQuery.of(context).size.width * 0.7, 
                        fit: BoxFit.contain, 
                        errorBuilder: (context, error, stackTrace) => const Icon(Icons.broken_image, size: 100, color: Colors.grey),
                      ),
                    ),
                  ),
                  const SizedBox(height: 20),
                  Text(
                    product.title,
                    style: const TextStyle(
                      fontSize: 24,
                      fontWeight: FontWeight.bold,
                      color: Colors.deepOrange,
                    ),
                  ),
                  const SizedBox(height: 10),
                  Row(
                    children: [
                      const Text(
                        'Harga: ',
                        style: TextStyle(
                          fontSize: 18,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      Text(
                        'Rp ${(product.price * 10000).toStringAsFixed(0)}', 
                        style: const TextStyle(
                          fontSize: 20,
                          fontWeight: FontWeight.bold,
                          color: Colors.green,
                        ),
                      ),
                    ],
                  ),
                  const SizedBox(height: 10),
                  Row(
                    children: [
                      const Icon(Icons.star, color: Colors.amber, size: 18),
                      const SizedBox(width: 5),
                      Text(
                        product.rating.toStringAsFixed(1),
                        style: const TextStyle(fontSize: 16),
                      ),
                      const SizedBox(width: 10),
                      const Icon(Icons.category, color: Colors.grey, size: 18),
                      const SizedBox(width: 5),
                      Text(
                        product.category,
                        style: const TextStyle(fontSize: 16),
                      ),
                    ],
                  ),
                  const SizedBox(height: 20),
                  const Text(
                    'Deskripsi Produk:',
                    style: TextStyle(
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 10),
                  Text(
                    product.description,
                    style: const TextStyle(fontSize: 16),
                  ),
                  const SizedBox(height: 30),
                  Center(
                    child: Consumer<CartProvider>(
                      builder: (context, cartProvider, child) {
                        return ElevatedButton(
                          onPressed: () {
                            if (!cartProvider.isProductInCart(product)) {
                              cartProvider.addItem(product);
                              ScaffoldMessenger.of(context).showSnackBar(
                                SnackBar(content: Text('${product.title} ditambahkan ke keranjang')),
                              );
                            } else {
                              ScaffoldMessenger.of(context).showSnackBar(
                                SnackBar(content: Text('${product.title} sudah ada di keranjang')),
                              );
                            }
                          },
                          style: ElevatedButton.styleFrom(
                            backgroundColor: cartProvider.isProductInCart(product) ? Colors.grey : Colors.orange,
                            foregroundColor: Colors.white,
                            padding: const EdgeInsets.symmetric(horizontal: 30, vertical: 15),
                            shape: RoundedRectangleBorder(
                              borderRadius: BorderRadius.circular(10),
                            ),
                          ),
                          child: Text(
                            cartProvider.isProductInCart(product) ? 'Sudah di Keranjang' : 'Masukan Keranjang',
                            style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                          ),
                        );
                      },
                    ),
                  ),
                ],
              ),
            );
          }
        },
      ),
    );
  }
}
```
#### Penjelasannya:
##### File product_detail_screen.dart menampilkan halaman detail dari satu produk. Halaman ini menerima productId dari halaman sebelumnya. Di dalam initState, kita memanggil getProductDetail() dari ApiService untuk mengambil data detail produk berdasarkan ID. Jika data berhasil diambil, maka ditampilkan gambar produk, nama, harga, rating, kategori, dan deskripsi. Di bagian bawah terdapat tombol “Masukan Keranjang” yang akan berubah menjadi “Sudah di Keranjang” jika produk sudah ditambahkan. Tombol ini juga memanggil addItem() dari CartProvider untuk menambahkan produk ke keranjang. Seluruh isi halaman dibungkus dalam SingleChildScrollView agar bisa di-scroll jika konten panjang.
- `payment_screen.dart`
```
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:toko_denbaayyy/providers/cart_provider.dart';
import 'package:toko_denbaayyy/screens/payment/payment_success_screen.dart';

class PaymentScreen extends StatelessWidget {
  const PaymentScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final cartProvider = Provider.of<CartProvider>(context);

    return Scaffold(
      appBar: AppBar(
        title: const Text('Pembayaran'),
        backgroundColor: Colors.orange,
      ),
      body: cartProvider.items.isEmpty
          ? const Center(
              child: Text(
                'Keranjang Anda kosong. Tidak ada yang perlu dibayar.',
                textAlign: TextAlign.center,
                style: TextStyle(fontSize: 18, color: Colors.grey),
              ),
            )
          : Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const Text(
                    'Detail Pesanan:',
                    style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold, color: Colors.deepOrange),
                  ),
                  const SizedBox(height: 15),
                  Expanded(
                    child: ListView.builder(
                      itemCount: cartProvider.items.length,
                      itemBuilder: (context, index) {
                        final item = cartProvider.items[index];
                        return Card(
                          margin: const EdgeInsets.symmetric(vertical: 6.0),
                          elevation: 2,
                          shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(10)),
                          child: Padding(
                            padding: const EdgeInsets.all(10.0),
                            child: Row(
                              children: [
                                ClipRRect(
                                  borderRadius: BorderRadius.circular(8),
                                  child: Image.network(
                                    item.product.thumbnail,
                                    width: 60,
                                    height: 60,
                                    fit: BoxFit.cover,
                                  ),
                                ),
                                const SizedBox(width: 10),
                                Expanded(
                                  child: Column(
                                    crossAxisAlignment: CrossAxisAlignment.start,
                                    children: [
                                      Text(
                                        item.product.title,
                                        style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 15),
                                        maxLines: 1,
                                        overflow: TextOverflow.ellipsis,
                                      ),
                                      Text(
                                        'Jumlah: ${item.quantity}',
                                        style: const TextStyle(fontSize: 13, color: Colors.grey),
                                      ),
                                      Text(
                                        'Harga Satuan: Rp ${(item.product.price * 10000).toStringAsFixed(0)}',
                                        style: const TextStyle(fontSize: 13, color: Colors.grey),
                                      ),
                                      Text(
                                        'Subtotal: Rp ${(item.product.price * item.quantity * 10000).toStringAsFixed(0)}',
                                        style: const TextStyle(fontWeight: FontWeight.bold, color: Colors.green, fontSize: 14),
                                      ),
                                    ],
                                  ),
                                ),
                              ],
                            ),
                          ),
                        );
                      },
                    ),
                  ),
                  const SizedBox(height: 20),
                  const Text(
                    'Metode Pembayaran:',
                    style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                  ),
                  const SizedBox(height: 10),
                  Card(
                    elevation: 2,
                    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(10)),
                    child: const Padding(
                      padding: EdgeInsets.all(12.0),
                      child: Row(
                        children: [
                          Icon(Icons.credit_card, color: Colors.blue, size: 30),
                          SizedBox(width: 15),
                          Text(
                            'Kartu Kredit / Debit (Simulasi)',
                            style: TextStyle(fontSize: 16),
                          ),
                        ],
                      ),
                    ),
                  ),
                  const SizedBox(height: 20),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      const Text(
                        'Total yang harus dibayar:',
                        style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                      ),
                      Text(
                        'Rp ${(cartProvider.totalPrice * 10000).toStringAsFixed(0)}',
                        style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold, color: Colors.red),
                      ),
                    ],
                  ),
                  const SizedBox(height: 30),
                  SizedBox(
                    width: double.infinity,
                    child: ElevatedButton(
                      onPressed: () {
                        Future.delayed(const Duration(seconds: 2), () {
                          cartProvider.clearCart(); 
                          Navigator.pushReplacement(
                            context,
                            MaterialPageRoute(builder: (context) => const PaymentSuccessScreen()),
                          );
                        });
                      },
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Colors.green,
                        foregroundColor: Colors.white,
                        padding: const EdgeInsets.symmetric(vertical: 15),
                        shape: RoundedRectangleBorder(
                          borderRadius: BorderRadius.circular(10),
                        ),
                      ),
                      child: const Text(
                        'Bayar Sekarang',
                        style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                      ),
                    ),
                  ),
                ],
              ),
            ),
    );
  }
}
```
#### Penjelasannya:
##### File payment_screen.dart adalah halaman pembayaran yang menampilkan ringkasan pesanan. Jika keranjang kosong, maka akan muncul pesan bahwa tidak ada yang perlu dibayar. Jika ada isi keranjang, maka ditampilkan dalam bentuk ListView, dengan setiap item ditampilkan bersama jumlah, harga satuan, dan subtotal. Di bagian bawah terdapat informasi metode pembayaran (simulasi) dan total yang harus dibayar. Ketika tombol “Bayar Sekarang” ditekan, data keranjang akan dikosongkan dengan clearCart(), lalu diarahkan ke halaman PaymentSuccessScreen.
- `payment_success_screen.dart`
```
import 'package:flutter/material.dart';
import 'package:toko_denbaayyy/screens/home/home_screen.dart';

class PaymentSuccessScreen extends StatelessWidget {
  const PaymentSuccessScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(24.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Icon(
                Icons.check_circle_outline,
                color: Colors.green,
                size: 150,
              ),
              const SizedBox(height: 30),
              const Text(
                'Pembayaran Berhasil!',
                style: TextStyle(
                  fontSize: 28,
                  fontWeight: FontWeight.bold,
                  color: Colors.green,
                ),
                textAlign: TextAlign.center,
              ),
              const SizedBox(height: 20),
              const Text(
                'Terima kasih telah berbelanja di Toko Denbaayyy. Pesanan Anda akan segera diproses.',
                style: TextStyle(
                  fontSize: 16,
                  color: Colors.black54,
                ),
                textAlign: TextAlign.center,
              ),
              const SizedBox(height: 50),
              ElevatedButton(
                onPressed: () {
                  Navigator.of(context).pushAndRemoveUntil(
                    MaterialPageRoute(builder: (context) => const HomeScreen()),
                    (Route<dynamic> route) => false,
                  );
                },
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.orange,
                  foregroundColor: Colors.white,
                  padding: const EdgeInsets.symmetric(horizontal: 40, vertical: 15),
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10),
                  ),
                ),
                child: const Text(
                  'Kembali ke Beranda',
                  style: TextStyle(fontSize: 18),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```
#### Penjelasannya:
##### File payment_success_screen.dart adalah halaman konfirmasi bahwa pembayaran berhasil. Ditampilkan ikon centang besar berwarna hijau dan pesan ucapan terima kasih. Di bagian bawah terdapat tombol “Kembali ke Beranda” yang akan mengarahkan pengguna kembali ke HomeScreen, serta menghapus riwayat sebelumnya agar tidak bisa kembali ke halaman sebelumnya menggunakan tombol back.
- `product.dart`
```
class Product {
  final int id;
  final String title;
  final String description;
  final double price;
  final double discountPercentage;
  final double rating;
  final int stock;
  final String brand;
  final String category;
  final String thumbnail;
  final List<String> images;

  Product({
    required this.id,
    required this.title,
    required this.description,
    required this.price,
    required this.discountPercentage,
    required this.rating,
    required this.stock,
    required this.brand,
    required this.category,
    required this.thumbnail,
    required this.images,
  });

  factory Product.fromJson(Map<String, dynamic> json) {
    return Product(
      id: json['id'] as int,
      title: json['title'] as String,
      description: json['description'] as String,
      price: (json['price'] as num?)?.toDouble() ?? 0.0,
      discountPercentage: (json['discountPercentage'] as num?)?.toDouble() ?? 0.0,
      rating: (json['rating'] as num?)?.toDouble() ?? 0.0,
      stock: json['stock'] as int? ?? 0,
      brand: json['brand'] as String? ?? 'No Brand',
      category: json['category'] as String? ?? 'Uncategorized',
      thumbnail: json['thumbnail'] as String? ?? 'https://via.placeholder.com/150',
      images: (json['images'] as List<dynamic>?)?.map((e) => e.toString()).toList() ?? ['https://via.placeholder.com/150'],
    );
  }
}
```
#### Penjelasannya:
##### File product.dart merupakan model dari data produk yang kita ambil dari API. Di dalamnya terdapat berbagai atribut seperti id, title, description, price, rating, category, dan lain-lain. Kita juga membuat factory method Product.fromJson yang berfungsi untuk mengubah data JSON yang diterima dari API menjadi objek Product di dalam aplikasi Flutter. Ini sangat penting agar data dapat diolah dan ditampilkan dengan mudah dalam widget.
- `cart_provider.dart`
```
import 'package:flutter/material.dart';
import 'package:toko_denbaayyy/models/product.dart';

class CartItem {
  final Product product;
  int quantity;

  CartItem({required this.product, this.quantity = 1});
}

class CartProvider with ChangeNotifier {
  final List<CartItem> _items = [];

  List<CartItem> get items => _items;

  double get totalPrice {
    double total = 0.0;
    for (var item in _items) {
      total += item.product.price * item.quantity;
    }
    return total;
  }

  bool isProductInCart(Product product) {
    return _items.any((item) => item.product.id == product.id);
  }

  void addItem(Product product) {
    int index = _items.indexWhere((item) => item.product.id == product.id);
    if (index != -1) {
      _items[index].quantity++;
    } else {
      _items.add(CartItem(product: product));
    }
    notifyListeners();
  }

  void removeItem(Product product) {
    _items.removeWhere((item) => item.product.id == product.id);
    notifyListeners();
  }

  void increaseQuantity(Product product) {
    int index = _items.indexWhere((item) => item.product.id == product.id);
    if (index != -1) {
      _items[index].quantity++;
      notifyListeners();
    }
  }

  void decreaseQuantity(Product product) {
    int index = _items.indexWhere((item) => item.product.id == product.id);
    if (index != -1) {
      if (_items[index].quantity > 1) {
        _items[index].quantity--;
      } else {
        _items.removeAt(index);
      }
      notifyListeners();
    }
  }

  void clearCart() {
    _items.clear();
    notifyListeners();
  }
}
```
#### Penjelasannya:
##### File cart_provider.dart berisi logika state management untuk keranjang belanja. Pertama, kita definisikan class CartItem yang menyimpan data produk dan jumlahnya. Kemudian kita buat class CartProvider dengan metode-metode utama seperti addItem(), removeItem(), increaseQuantity(), decreaseQuantity(), dan clearCart(). Kita juga menyediakan getter items untuk mendapatkan daftar item dalam keranjang, dan totalPrice untuk menghitung total harga semua barang. Setiap kali data berubah, kita memanggil notifyListeners() agar UI otomatis diperbarui.
- `cart_dialog.dart`
```
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:toko_denbaayyy/providers/cart_provider.dart';
import 'package:toko_denbaayyy/widgets/cart_item_card.dart';
import 'package:toko_denbaayyy/screens/payment/payment_screen.dart'; 

class CartDialog extends StatelessWidget {
  const CartDialog({super.key});

  @override
  Widget build(BuildContext context) {
    return Dialog(
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(20),
      ),
      elevation: 10,
      child: Consumer<CartProvider>(
        builder: (context, cartProvider, child) {
          return Container(
            padding: const EdgeInsets.all(16.0),
            width: MediaQuery.of(context).size.width * 0.85,
            constraints: BoxConstraints(
              maxHeight: MediaQuery.of(context).size.height * 0.7,
            ),
            child: Column(
              mainAxisSize: MainAxisSize.min,
              children: [
                Row(
                  mainAxisAlignment: MainAxisAlignment.spaceBetween,
                  children: [
                    const Text(
                      'Keranjang Belanja',
                      style: TextStyle(
                        fontSize: 20,
                        fontWeight: FontWeight.bold,
                        color: Colors.orange,
                      ),
                    ),
                    IconButton(
                      icon: const Icon(Icons.close),
                      onPressed: () => Navigator.pop(context),
                    ),
                  ],
                ),
                const Divider(),
                if (cartProvider.items.isEmpty)
                  const Expanded(
                    child: Center(
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.center,
                        children: [
                          Icon(Icons.shopping_cart_outlined, size: 70, color: Colors.grey),
                          SizedBox(height: 10),
                          Text(
                            'Keranjang Anda kosong!',
                            style: TextStyle(fontSize: 16, color: Colors.grey),
                          ),
                        ],
                      ),
                    ),
                  )
                else
                  Expanded(
                    child: ListView.builder(
                      shrinkWrap: true,
                      itemCount: cartProvider.items.length,
                      itemBuilder: (context, index) {
                        final cartItem = cartProvider.items[index];
                        return CartItemCard(cartItem: cartItem);
                      },
                    ),
                  ),
                Padding(
                  padding: const EdgeInsets.only(top: 10.0),
                  child: Column(
                    children: [
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          const Text(
                            'Total Harga:',
                            style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
                          ),
                          Text(
                            'Rp ${(cartProvider.totalPrice * 10000).toStringAsFixed(0)}',
                            style: const TextStyle(
                              fontSize: 20,
                              fontWeight: FontWeight.bold,
                              color: Colors.green,
                            ),
                          ),
                        ],
                      ),
                      const SizedBox(height: 15),
                      ElevatedButton(
                        onPressed: () {
                          // >>> Navigasi ke PaymentScreen
                          if (cartProvider.items.isNotEmpty) {
                            Navigator.pop(context); 
                            Navigator.push(
                              context,
                              MaterialPageRoute(builder: (context) => const PaymentScreen()),
                            );
                          } else {
                            ScaffoldMessenger.of(context).showSnackBar(
                              const SnackBar(content: Text('Keranjang kosong, tidak bisa checkout!')),
                            );
                          }
                        },
                        style: ElevatedButton.styleFrom(
                          backgroundColor: Colors.green,
                          foregroundColor: Colors.white,
                          minimumSize: const Size(double.infinity, 45),
                          shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.circular(10),
                          ),
                        ),
                        child: const Text(
                          'Checkout',
                          style: TextStyle(fontSize: 18),
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
          );
        },
      ),
    );
  }
}
```
#### Penjelasannya:
##### File cart_dialog.dart menampilkan dialog pop-up ketika ikon keranjang ditekan. Dialog ini menampilkan semua item dalam keranjang. Jika keranjang kosong, akan muncul ikon dan teks bahwa keranjang masih kosong. Jika tidak kosong, item ditampilkan dalam bentuk ListView.builder menggunakan widget CartItemCard. Di bagian bawah dialog terdapat total harga seluruh item dan tombol “Checkout” yang akan mengarahkan pengguna ke halaman pembayaran (PaymentScreen). Jika tombol ditekan saat keranjang kosong, maka akan muncul snackbar peringatan.
- `api_service.dart`
```
import 'dart:convert';
import 'package:http/http.dart' as http;
import 'package:toko_denbaayyy/models/product.dart';

class ApiService {
  static const String baseUrl = 'https://dummyjson.com';

  // Fungsi untuk Login
  Future<Map<String, dynamic>> login(String username, String password) async {
    final response = await http.post(
      Uri.parse('$baseUrl/auth/login'),
      headers: <String, String>{
        'Content-Type': 'application/json',
      },
      body: jsonEncode(<String, String>{
        'username': username, 
        'password': password,
      }),
    );

    if (response.statusCode == 200) {
      return jsonDecode(response.body);
    } else {
      throw Exception('Failed to login: ${response.statusCode} ${response.body}');
    }
  }

  // Fungsi untuk mendapatkan daftar produk 
  Future<List<Product>> getFoodProducts() async {
    final response = await http.get(Uri.parse('$baseUrl/products/category/groceries'));

    if (response.statusCode == 200) {
      final Map<String, dynamic> data = jsonDecode(response.body);
      final List<dynamic> productsJson = data['products'];
      return productsJson.map((json) => Product.fromJson(json)).toList();
    } else {
      throw Exception('Failed to load food products');
    }
  }

  // Fungsi untuk mendapatkan detail produk
  Future<Product> getProductDetail(int productId) async {
    final response = await http.get(Uri.parse('$baseUrl/products/$productId'));

    if (response.statusCode == 200) {
      return Product.fromJson(jsonDecode(response.body));
    } else {
      throw Exception('Failed to load product detail');
    }
  }
}
```
#### Penjelasannya:
##### File api_service.dart berisi fungsi-fungsi HTTP yang mengakses API. Terdapat tiga fungsi utama: login() untuk autentikasi, getFoodProducts() untuk mendapatkan daftar produk dari kategori groceries, dan getProductDetail() untuk mengambil detail sebuah produk berdasarkan ID. Semua request menggunakan package http dan data di-decode dari JSON ke objek dart menggunakan jsonDecode. Di sini kita juga mengatur header seperti Content-Type: application/json.
- `cart_item_card.dart`
```
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:toko_denbaayyy/providers/cart_provider.dart';

class CartItemCard extends StatelessWidget {
  final CartItem cartItem;

  const CartItemCard({super.key, required this.cartItem});

  @override
  Widget build(BuildContext context) {
    final cartProvider = Provider.of<CartProvider>(context, listen: false);

    return Card(
      margin: const EdgeInsets.symmetric(horizontal: 16.0, vertical: 8.0),
      elevation: 3,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(10),
      ),
      child: Padding(
        padding: const EdgeInsets.all(12.0),
        child: Row(
          children: [
            ClipRRect(
              borderRadius: BorderRadius.circular(8),
              child: Image.network(
                cartItem.product.thumbnail,
                width: 80,
                height: 80,
                fit: BoxFit.cover,
                errorBuilder: (context, error, stackTrace) => const Icon(Icons.broken_image, size: 60),
              ),
            ),
            const SizedBox(width: 12),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(
                    cartItem.product.title,
                    style: const TextStyle(
                      fontWeight: FontWeight.bold,
                      fontSize: 16,
                    ),
                    maxLines: 2,
                    overflow: TextOverflow.ellipsis,
                  ),
                  const SizedBox(height: 4),
                  Text(
                    'Rp ${(cartItem.product.price * 10000).toStringAsFixed(0)}', 
                    style: const TextStyle(
                      color: Colors.green,
                      fontWeight: FontWeight.bold,
                      fontSize: 14,
                    ),
                  ),
                  const SizedBox(height: 8),
                  Row(
                    children: [
                      IconButton(
                        icon: const Icon(Icons.remove_circle),
                        onPressed: () {
                          cartProvider.decreaseQuantity(cartItem.product);
                        },
                      ),
                      Text(
                        '${cartItem.quantity}',
                        style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                      IconButton(
                        icon: const Icon(Icons.add_circle),
                        onPressed: () {
                          cartProvider.increaseQuantity(cartItem.product);
                        },
                      ),
                      const Spacer(),
                      Text(
                        'Total: Rp ${(cartItem.product.price * cartItem.quantity * 10000).toStringAsFixed(0)}', 
                        style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
                      ),
                    ],
                  ),
                ],
              ),
            ),
            IconButton(
              icon: const Icon(Icons.delete, color: Colors.red),
              onPressed: () {
                cartProvider.removeItem(cartItem.product);
              },
            ),
          ],
        ),
      ),
    );
  }
}
```
#### Penjelasannya:
##### File cart_item_card.dart adalah widget yang menampilkan satu item dalam keranjang. Isinya berupa gambar produk, nama, harga, dan jumlah. Ada dua tombol untuk menambah dan mengurangi jumlah, serta tombol delete untuk menghapus item dari keranjang. Semua aksi ini terhubung dengan method yang ada di CartProvider. Setiap perubahan jumlah atau penghapusan akan otomatis memperbarui tampilan karena CartProvider menggunakan notifyListeners().
### 7. Testing (Menjalankan Aplikasi)
- Buka terminal lalu ketik perintah `flutter pub get`.
- Selanjutnya, ketik perintah `flutter run`.
- Selanjutnya, akan disuruh memilih menjalankan memakai apa, contoh ketik `2` untuk memilih memakai chrome.
- Selanjutnya, selesai.
---

## 📸 Hasil Simulasi

### Semua hasil UI dapat dilihat pada folder `SS_an/` dalam repo ini, meliputi:
✅ Login <br> 
✅ Home <br> 
✅ Detail Produk <br> 
✅ Keranjang <br> 
✅ Pembayaran <br> 
✅ Pembayaran Berhasil <br> 

---

## 🙏 Penutup

Demikian dokumentasi aplikasi **E-Commerce Sederhana Menggunakan Flutter**.  
Jika terdapat kesalahan atau kekurangan, saya mohon maaf.  
Terima kasih telah membaca dan semoga bermanfaat 🎉
