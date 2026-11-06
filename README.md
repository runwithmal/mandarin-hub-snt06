# Mandarin Hub V4.1 — Perbaikan Login & Registrasi

## Perbaikan pada versi ini
- Menambahkan Firebase Authentication compat SDK yang sebelumnya belum dimuat. Ini penyebab utama `firebase.auth()` tidak tersedia sehingga login/registrasi gagal di versi sebelumnya.
- Tombol Masuk dan Daftar akun ditandai sebagai `type="button"`.
- Pesan error konfigurasi Firebase dibuat lebih jelas.
- Pendaftaran tetap terbuka: pengguna dapat memilih Guru/Operator atau Siswa.

## Penting
Registrasi sungguhan dan login lintas perangkat membutuhkan Firebase milikmu. File HTML tidak dapat menyimpan akun lintas perangkat tanpa backend.

## Setup Firebase (wajib)
1. Buka https://console.firebase.google.com/ dan buat project.
2. Tambahkan aplikasi Web (`</>`), lalu salin Firebase config.
3. Di Authentication > Sign-in method, aktifkan **Email/Password**.
4. Buat Realtime Database.
5. Buka `index.html` dengan Notepad/VS Code. Cari `const firebaseConfig = { ... }` dan ganti placeholder dengan konfigurasi Firebase Web milikmu. Contoh:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "nama-project.firebaseapp.com",
  databaseURL: "https://nama-project-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "nama-project",
  appId: "1:...:web:..."
};
```

Gunakan `databaseURL` persis seperti yang ditampilkan di halaman Realtime Database Firebase karena region bisa berbeda.

6. Untuk uji coba internal setelah Authentication aktif, gunakan rules berikut:

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
```

**Jangan gunakan rules sederhana ini untuk publikasi dengan data siswa sungguhan.** Semua pengguna login dapat membaca/menulis semua data. Untuk produksi, batasi akses per UID, verifikasi peran guru, dan validasi poin/streak melalui backend.

7. Simpan file dan jalankan melalui server lokal atau hosting HTTPS (misalnya Firebase Hosting). Browser perlu internet untuk memuat Firebase SDK.
8. Pilih **Daftar akun**, isi nama, pilih Guru/Operator atau Siswa, email, dan password minimal 6 karakter. Klik **Buat akun**.

## Troubleshooting
- Pesan Firebase belum dikonfigurasi: config masih placeholder/salah.
- `auth/operation-not-allowed`: Email/Password belum diaktifkan.
- `auth/invalid-api-key`: apiKey salah.
- `PERMISSION_DENIED`: periksa Rules Realtime Database.
- Jika membuka langsung lewat `file://` bermasalah, jalankan lewat server lokal atau deploy Firebase Hosting.

V4.3: Memperbaiki urutan deklarasi helper `$`. Sebelumnya showAuth() dipanggil sebelum helper tersebut dideklarasikan, menyebabkan ReferenceError dan menghentikan eksekusi JavaScript sebelum tab Daftar Akun berfungsi.
