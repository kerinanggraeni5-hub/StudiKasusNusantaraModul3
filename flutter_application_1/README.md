# Nusantara Cerdas Mobile

**Nusantara Cerdas Mobile** adalah sebuah aplikasi mobile yang dibangun menggunakan **Flutter** sebagai prototype penerapan konsep **Smart City**. Aplikasi ini dirancang untuk memberikan informasi seputar Smart City melalui enam pilar utama serta menyediakan beberapa layanan yang dapat digunakan untuk memenuhi kebutuhan masyarakat.

Pada tahap terbaru (Modul 3), aplikasi dilengkapi dengan **manajemen state** menggunakan **Provider** sehingga layanan favorit dan pengajuan layanan warga dapat tersimpan serta tampil secara reaktif di berbagai halaman.

## Fitur yang Tersedia

* **Halaman Beranda** — Menampilkan gambaran mengenai enam pilar yang menjadi bagian dari konsep Smart City.
* **Halaman Layanan** — Berisi kumpulan layanan yang dikelompokkan dalam tiga bidang (Perizinan, Kesehatan, dan Transportasi) dan dapat diakses oleh pengguna. Setiap layanan memiliki ikon bintang untuk menandainya sebagai favorit.
* **Detail Layanan** — Menampilkan penjelasan dan informasi yang lebih rinci dari layanan yang dipilih, serta tombol **Ajukan Permohonan** yang menampilkan indikator *sedang mengajukan* sebelum pengajuan tercatat.
* **Halaman Warga** — Menyediakan informasi serta fitur yang berhubungan dengan kebutuhan masyarakat, termasuk bagian **Layanan Favorit** yang tampil secara reaktif.
* **Riwayat Laporan** — Digunakan untuk melihat kembali laporan yang sebelumnya telah dibuat oleh pengguna.
* **Pengaturan Kota** — Menyediakan beberapa opsi pengaturan yang berkaitan dengan informasi dan kondisi kota.
* **Tentang Aplikasi** — Berisi keterangan umum mengenai aplikasi Nusantara Cerdas Mobile.
* **Route Tidak Ditemukan** — Digunakan sebagai halaman pemberitahuan apabila pengguna membuka alamat halaman yang tidak tersedia di dalam aplikasi.

### Fitur Manajemen State (Modul 3)

* **Layanan Favorit** — Warga dapat menandai layanan dengan ikon bintang pada halaman Layanan. Daftar favorit muncul pada halaman Warga tanpa memandang tab bidang asal layanan.
* **Badge Jumlah Pengajuan** — Jumlah layanan yang telah diajukan ditampilkan sebagai badge pada ikon tujuan **Warga** (NavigationBar, NavigationRail, dan NavigationDrawer), sehingga tetap terlihat saat warga membuka Beranda atau Layanan.
* **Indikator Sedang Mengajukan** — Tombol Ajukan Permohonan dinonaktifkan dan menampilkan indikator putar sesaat sebelum pengajuan tercatat, agar tombol tidak ditekan berulang kali.

## Teknologi dan Komponen

Dalam proses pembuatannya, aplikasi ini memanfaatkan beberapa teknologi utama, antara lain:

* **Flutter** sebagai framework untuk membangun aplikasi mobile.
* **Dart** sebagai bahasa pemrograman yang digunakan dalam pengembangan aplikasi.
* **Material Design 3** untuk membantu membangun tampilan antarmuka aplikasi.
* **Navigator dan Named Routes** pada Flutter untuk mengelola perpindahan dari satu halaman ke halaman lainnya.
* **Provider** dan **ChangeNotifier** untuk mengelola state yang dipakai bersama oleh beberapa halaman.

## Susunan Folder Project

```text
lib/
├── main.dart
├── models/
│   ├── favorit_model.dart
│   └── pengajuan_model.dart
├── navigation/
│   ├── app_routes.dart
│   └── kerangka_navigasi.dart
└── pages/
    ├── beranda_page.dart
    ├── layanan_page.dart
    ├── detail_layanan_page.dart
    ├── warga_page.dart
    ├── riwayat_laporan_page.dart
    ├── pengaturan_kota_page.dart
    ├── tentang_aplikasi_page.dart
    └── route_tidak_dikenal_page.dart
```

### Keterangan Folder

* **`main.dart`** → Menjadi file utama yang pertama kali digunakan saat aplikasi dijalankan. Pada file ini `MultiProvider` dipasang di atas `MaterialApp`.
* **`models/`** → Berisi class state berbasis `ChangeNotifier`.
  * `favorit_model.dart` → `FavoritModel`, menyimpan kumpulan nama layanan favorit melalui `tandai()` dan `batalTandai()`.
  * `pengajuan_model.dart` → `PengajuanModel`, menyimpan daftar layanan yang diajukan beserta getter `totalPengajuan`.
* **`navigation/`** → Berisi file yang menangani pengaturan navigasi serta daftar route yang digunakan.
* **`pages/`** → Menampung file-file yang digunakan untuk membangun setiap halaman aplikasi.

## Manajemen State

Pemilihan teknik pengelolaan state disesuaikan dengan jangkauan datanya.

| Bagian | Teknik | Alasan |
| --- | --- | --- |
| Warna ikon bintang pada kartu layanan | `context.watch<FavoritModel>()` | Tampilan harus berubah mengikuti status favorit. |
| Aksi menekan ikon bintang | `context.read<FavoritModel>()` | Hanya memanggil aksi, tidak perlu mendengarkan perubahan. |
| Layanan Favorit pada halaman Warga | `Consumer<FavoritModel>` | Hanya daftar favorit yang dibangun ulang saat model berubah. |
| Badge jumlah pengajuan | `context.select<PengajuanModel, int>()` | Hanya membutuhkan angka `totalPengajuan`. |
| Menambahkan pengajuan | `context.read<PengajuanModel>()` | Aksi satu kali setelah proses pengajuan selesai. |
| Status sedang mengajukan | `setState()` | State lokal milik halaman detail layanan saja. |

`MultiProvider` ditempatkan **di atas `MaterialApp`** agar seluruh route, termasuk halaman yang dibuka melalui `Navigator.pushNamed`, dapat menjangkau `FavoritModel` dan `PengajuanModel` sehingga tidak terjadi `ProviderNotFoundException` pada layar sempit maupun layar lebar.

## Langkah Menjalankan Project

Sebelum mencoba menjalankan aplikasi, pastikan Flutter sudah terinstal dan konfigurasi environment telah dilakukan dengan benar.

Buka terminal pada folder project, kemudian jalankan:

```bash
flutter pub get
flutter run
```

Perintah `flutter pub get` juga akan mengunduh package **provider** yang dibutuhkan. Jika package tersebut belum ada pada `pubspec.yaml`, tambahkan dengan:

```bash
flutter pub add provider
```

Aplikasi juga dapat dijalankan pada peramban untuk melihat tampilan layar lebar (NavigationRail):

```bash
flutter run -d chrome
```

Perintah `flutter doctor` juga dapat digunakan untuk mengecek apakah terdapat masalah pada instalasi maupun konfigurasi Flutter.

```bash
flutter doctor
```

## Gambaran Navigasi Aplikasi

Perpindahan antarhalaman dalam aplikasi memiliki alur utama seperti berikut:

```text
Beranda
├── Layanan
│   └── Detail Layanan  (Ajukan Permohonan → pesan hasil tampil sebagai SnackBar)
├── Warga
│   ├── Layanan Favorit
│   └── Riwayat Laporan
├── Pengaturan Kota
└── Tentang Aplikasi
```

Pada layar sempit (lebar kurang dari 600 piksel logis) navigasi utama berupa **NavigationBar** di bawah, sedangkan pada layar lebar berubah menjadi **NavigationRail** di sisi kiri. Menu pendukung tersedia melalui **NavigationDrawer**.

Dari halaman utama, pengguna dapat memilih berbagai menu yang tersedia dan berpindah menuju halaman sesuai dengan kebutuhan.

## Rencana Pengembangan

Karena aplikasi ini masih dalam tahap prototype, terdapat beberapa pengembangan yang dapat dilakukan agar sistem menjadi lebih lengkap. Beberapa di antaranya adalah:

* Menyimpan layanan favorit dan pengajuan secara **permanen** agar tidak hilang saat aplikasi ditutup (misalnya dengan penyimpanan lokal).
* Menghubungkan aplikasi dengan **backend melalui REST API**.
* Menambahkan **database** untuk menyimpan dan mengelola data.
* Membuat mekanisme **login serta registrasi pengguna**.
* Mengembangkan layanan **pelaporan warga** agar dapat digunakan secara lebih nyata.
* Menambahkan **fitur notifikasi** kepada pengguna.
* Menyediakan data dan informasi kota yang dapat diperbarui secara **real-time**.

## Status Aplikasi

Saat ini, **Nusantara Cerdas Mobile** masih berada pada tahap **prototype aplikasi Smart City berbasis Flutter**. Data favorit dan pengajuan masih disimpan di memori selama aplikasi berjalan. Project ini dibuat sebagai media pembelajaran sekaligus dasar untuk mengembangkan aplikasi layanan masyarakat yang lebih lengkap pada tahap berikutnya.