Berikut ini deskripsi lengkap dan terstruktur untuk proyek **Daily Habit Tracker**, ditulis dengan gaya akademik dan profesional — cocok untuk laporan tesis, dokumentasi proyek, atau portofolio yang SEO-friendly dan berbasis teori sistem informasi modern.

---

# 🧠 **Deskripsi Lengkap Proyek: Daily Habit Tracker**

## 1. **Latar Belakang**

Kebiasaan harian (*daily habits*) merupakan salah satu indikator penting dalam pembentukan perilaku produktif dan pengelolaan waktu individu. Namun, banyak pengguna mengalami kesulitan dalam memantau konsistensi kegiatan sehari-hari seperti membaca, berolahraga, atau meditasi. Kebutuhan ini melatarbelakangi pembangunan aplikasi **Daily Habit Tracker**, sebuah sistem berbasis **Flutter** dan **Firebase** yang memungkinkan pengguna untuk mencatat, melacak, dan memvisualisasikan perkembangan kebiasaan mereka secara digital dan real-time.

Proyek ini memanfaatkan pendekatan **Mobile Cloud Computing**, di mana aplikasi client (Flutter) berfungsi sebagai *frontend* dan layanan **Firebase** bertindak sebagai *Backend-as-a-Service (BaaS)*. Dengan demikian, aplikasi ini tidak hanya menyediakan pengalaman pengguna yang interaktif, tetapi juga menawarkan sinkronisasi data lintas perangkat yang efisien.

---

## 2. **Tujuan Proyek**

Tujuan utama dari pembangunan aplikasi **Daily Habit Tracker** ini adalah:

1. Membangun sistem pelacakan kebiasaan yang dapat diakses dari berbagai perangkat menggunakan infrastruktur cloud.
2. Menyediakan fitur autentikasi aman berbasis Firebase Authentication.
3. Memungkinkan pengguna untuk menambah, mengubah, dan menghapus daftar kebiasaan (CRUD Operation).
4. Mengimplementasikan sistem visualisasi progres harian dengan pendekatan heatmap.
5. Menyediakan antarmuka sederhana, efisien, dan mudah digunakan berbasis Flutter UI.

---

## 3. **Ruang Lingkup Sistem**

Aplikasi **Daily Habit Tracker** dikembangkan dengan fokus pada:

* Pengguna individu (personal use)
* Platform mobile dan web
* Basis data cloud (Firebase Firestore)
* Otentikasi email & password
* Sinkronisasi real-time dan *auto-update UI*

---

## 4. **Teknologi dan Arsitektur Sistem**

### 4.1. **Teknologi yang Digunakan**

| Komponen             | Teknologi                | Fungsi                                                           |
| -------------------- | ------------------------ | ---------------------------------------------------------------- |
| **Frontend**         | Flutter SDK              | Membangun antarmuka pengguna lintas platform (Android, iOS, Web) |
| **Backend (BaaS)**   | Firebase                 | Mengelola autentikasi, basis data, dan hosting aplikasi          |
| **Database**         | Cloud Firestore          | Menyimpan data kebiasaan pengguna secara terstruktur             |
| **State Management** | Provider                 | Mengatur status aplikasi dan komunikasi antar widget             |
| **Visualisasi Data** | flutter_heatmap_calendar | Menampilkan data progres kebiasaan secara visual                 |
| **Deployment**       | Firebase Hosting         | Mendistribusikan versi web aplikasi secara online                |

---

## 5. **Arsitektur Sistem**

Sistem menggunakan **arsitektur client-cloud**, di mana aplikasi Flutter bertindak sebagai client yang berinteraksi langsung dengan Firebase sebagai server-side service.

**Komponen utama arsitektur:**

* **Firebase Authentication** untuk login dan registrasi pengguna.
* **Firestore Database** untuk menyimpan kebiasaan pengguna dan tanggal penyelesaiannya.
* **Provider Pattern** untuk menjaga konsistensi state antar halaman.
* **Firebase Hosting** untuk deployment versi web aplikasi.

Diagram arsitektur konseptual:

```
+-------------------------+
|     User Interface      |
|  (Flutter Frontend)     |
+-----------+-------------+
            |
            ▼
+-------------------------+
| Firebase Authentication |
| (Email & Password Login)|
+-------------------------+
            |
            ▼
+-------------------------+
|  Cloud Firestore DB     |
|  - users/{uid}/habits   |
|  - completedDays[]      |
+-------------------------+
            |
            ▼
+-------------------------+
| Firebase Hosting (Web)  |
+-------------------------+
```

---

## 6. **Desain dan Struktur Data**

Setiap pengguna memiliki dokumen unik dalam koleksi `users`. Di dalam dokumen tersebut terdapat sub-koleksi `habits`, yang menyimpan daftar kebiasaan.

### Struktur Data:

```
users (collection)
 └── {uid} (document)
      └── habits (sub-collection)
           ├── {habit_id_1} (document)
           │    ├── name: "Olahraga"
           │    └── completedDays: [Timestamp(2025-10-15), ...]
           └── {habit_id_2} (document)
                ├── name: "Baca Buku"
                └── completedDays: [...]
```

### Atribut Data:

| Nama Field      | Tipe Data       | Keterangan                             |
| --------------- | --------------- | -------------------------------------- |
| `name`          | String          | Nama kebiasaan                         |
| `completedDays` | List<Timestamp> | Tanggal-tanggal kebiasaan diselesaikan |

---

## 7. **Fitur Utama**

### 7.1. **Autentikasi Pengguna**

* Sistem login dan registrasi menggunakan **Firebase Authentication**.
* Mekanisme login otomatis setelah registrasi berhasil.
* Proteksi akses halaman menggunakan *Auth Gate* berbasis `StreamBuilder`.

### 7.2. **CRUD Habit**

* Pengguna dapat membuat, membaca, memperbarui, dan menghapus data kebiasaan.
* Operasi dijalankan menggunakan fungsi asinkron dari kelas `FirestoreService`.
* Perubahan data akan otomatis diperbarui di UI berkat *real-time stream* Firestore.

### 7.3. **Tracking Harian**

* Setiap kebiasaan dapat ditandai selesai menggunakan *checkbox*.
* Tanggal penyelesaian disimpan dalam array `completedDays`.
* Fungsi logika `updateHabitStatus()` mengatur penambahan atau penghapusan tanggal.

### 7.4. **Visualisasi Heatmap**

* Menggunakan `flutter_heatmap_calendar` untuk memvisualisasikan konsistensi harian.
* Warna hijau menunjukkan hari-hari di mana kebiasaan diselesaikan.

### 7.5. **Keamanan Data**

* Firestore Rules memastikan hanya pengguna dengan UID yang sesuai dapat mengakses datanya sendiri.

```firestore
match /users/{userId}/{documents=**} {
  allow read, write: if request.auth != null && request.auth.uid == userId;
}
```

---

## 8. **Algoritma dan Logika Utama**

### 8.1. **Algoritma Autentikasi**

1. Ambil input email dan password.
2. Jalankan `FirebaseAuth.instance.signInWithEmailAndPassword()`.
3. Jika login berhasil → tampilkan `HomePage`.
4. Jika gagal → tampilkan pesan error melalui dialog.

### 8.2. **Algoritma CRUD**

* **Create:** Tambahkan habit baru dengan nama unik ke sub-koleksi `habits`.
* **Read:** Gunakan `StreamBuilder` untuk mendengarkan perubahan data Firestore.
* **Update:** Ubah status kebiasaan berdasarkan tanggal hari ini.
* **Delete:** Hapus dokumen habit berdasarkan `habitId`.

### 8.3. **Algoritma Heatmap**

1. Ambil daftar `completedDays` dari Firestore.
2. Konversi ke `Map<DateTime, int>`.
3. Tampilkan dataset tersebut menggunakan `HeatMap()` dengan warna hijau untuk tanggal sukses.

---

## 9. **Implementasi dan Deployment**

### 9.1. **Build Web App**

```bash
flutter build web
```

### 9.2. **Deploy ke Firebase Hosting**

```bash
firebase init hosting
firebase deploy --only hosting
```

Setelah proses selesai, aplikasi dapat diakses melalui domain:

```
https://nama-proyekmu.web.app
```

---

## 10. **Manfaat dan Relevansi**

1. **Empiris:** Aplikasi ini membantu pengguna membangun kebiasaan positif secara konsisten dan terukur.
2. **Logis:** Implementasi berbasis cloud memungkinkan akses lintas perangkat dan keamanan tinggi.
3. **Relevan:** Mendukung gaya hidup digital yang terintegrasi dan produktif.
4. **Teoritis:** Berdasarkan konsep *self-tracking behavior* dan *habit formation theory* (Lally & Gardner, 2013), sistem ini mendorong pembentukan perilaku otomatis melalui pemantauan rutin.

---

## 11. **Kesimpulan**

Proyek **Daily Habit Tracker** merupakan aplikasi pelacak kebiasaan yang modern, efisien, dan terdistribusi secara cloud-native. Integrasi **Flutter** dan **Firebase** membentuk ekosistem yang solid untuk aplikasi lintas platform dengan kemampuan *real-time synchronization*. Dengan desain berbasis *user-centric*, sistem ini tidak hanya mempermudah pengguna dalam memantau aktivitas sehari-hari, tetapi juga berkontribusi pada penelitian perilaku digital dan pembentukan habit dalam konteks teknologi modern.

---

Apakah kamu ingin aku bantu lanjutkan membuatkan versi **deskripsi teknis proyek (BAB III Metodologi Pengembangan Sistem)** untuk dijadikan bagian skripsi atau laporan penelitian juga, biar bisa lengkap satu paket dengan bagian ini?
