# Cara membuat data sinkron PC dan HP, dan bisa di-install

Penyebab dua masalah kamu:

1. **Tidak sinkron**: di file lama, config Firebase masih tulisan `ISI_API_KEY`, jadi aplikasi jalan
   "mode lokal" (data hanya di browser itu). Sekarang config sudah diisi.
2. **Tidak bisa di-install**: install ke HP butuh halaman yang dibuka lewat **alamat https**
   (bukan file yang dibuka dari folder), plus `manifest.json`, `sw.js`, dan ikon. Semua sudah dibuat.

Yang masih harus kamu lakukan ada di Firebase Console dan saat hosting.

## A. Aktifkan di Firebase Console (proyek monitoring-75f1b)

1. **Authentication** → Sign-in method → aktifkan **Google** → pilih email dukungan → Simpan.
2. **Firestore Database** → Create database → pilih lokasi (mis. `asia-southeast2` Jakarta) → mulai
   di production mode.
3. Tab **Rules** di Firestore → tempel isi `firestore.rules` → Publish.
   (Aturan ini membuat setiap akun hanya bisa melihat datanya sendiri.)
4. **Authentication → Settings → Authorized domains**: tambahkan domain tempat aplikasi di-host
   (kalau pakai Firebase Hosting, `monitoring-75f1b.web.app` dan `monitoring-75f1b.firebaseapp.com`
   sudah otomatis ada).

Catatan: kode memakai **Firestore**, bukan Realtime Database. `databaseURL` di config kamu tidak dipakai.

## B. Taruh di hosting https (pilih salah satu)

### Opsi 1: Firebase Hosting (paling mudah, satu proyek)
Di PC, buka terminal di folder ini:

```
npm install -g firebase-tools
firebase login
firebase use monitoring-75f1b
firebase deploy --only hosting,firestore:rules
```

Alamatnya: `https://monitoring-75f1b.web.app`

### Opsi 2: Netlify Drop
Buka app.netlify.com/drop, seret folder ini. Lalu tambahkan domain yang dibuat Netlify ke
Authorized domains (langkah A4).

## C. Install di HP

Buka alamat https tadi di HP, lalu:

- **Android (Chrome)**: menu ⋮ → **Instal aplikasi** (atau Tambahkan ke layar utama)
- **iPhone (Safari)**: Bagikan → **Tambah ke Layar Utama**

Masuk dengan akun Google yang sama di PC dan HP. Data akan muncul sama di keduanya dan
berubah otomatis saat ada input baru.

## Kalau masih bermasalah

- Tulisan di atas aplikasi "Mode lokal": config belum terbaca, muat ulang paksa (Ctrl+Shift+R).
- "Gagal masuk: auth/unauthorized-domain": domain belum ada di Authorized domains.
- "Gagal menyimpan ke cloud": Firestore belum dibuat atau rules belum dipublish.
- Membuka `index.html` langsung dari folder (file://) tidak bisa login Google dan tidak bisa di-install.
