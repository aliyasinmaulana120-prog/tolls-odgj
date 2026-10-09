# Tolls ODGJ — GitHub + Vercel + Supabase

Website prediksi TMA rerata 1 jam ke depan dengan **satu input TMA**, login akun, dan riwayat online yang bisa dihapus. Logo ODGJ sudah disertakan.

## Apa yang diperlukan
- Akun GitHub: https://github.com
- Akun Vercel: https://vercel.com
- Akun Supabase: https://supabase.com
- Semua mempunyai paket gratis dengan kuota dan ketentuan masing-masing. Tidak memerlukan Railway.

## 1. Siapkan Supabase
1. Buka Supabase Dashboard → **New project**. Tunggu database selesai dibuat.
2. Buka **SQL Editor**, tempel seluruh isi `schema.sql`, kemudian **Run**.
3. Pada **Authentication → Providers → Email**, aktifkan email/password. Disarankan mengaktifkan verifikasi email.
4. Salin **Project URL** dan **publishable key** (atau legacy anon public key) dari bagian Project Settings / API / Connect.
5. Edit `config.js`, ganti `YOUR_PROJECT` dan `YOUR_SUPABASE_PUBLISHABLE_OR_ANON_KEY` dengan nilai milik Anda. **Jangan pernah menggunakan service_role key atau secret key** di frontend.

## 2. Upload ke GitHub
1. Buat repository baru (boleh private), contoh `tolls-odgj`.
2. Upload berkas dari **isi folder ini** ke root repository: `index.html`, `config.js`, `logo-odgj.png`, `schema.sql`, `vercel.json`, dan `README.md`.
3. Commit perubahan. `schema.sql` hanya untuk dijalankan di Supabase, tidak dieksekusi oleh browser.

## 3. Deploy di Vercel
1. Login di https://vercel.com/new dengan akun GitHub.
2. **Import Git Repository**, pilih `tolls-odgj`.
3. Framework Preset: **Other**; Root Directory: `./`; tidak perlu framework atau build command.
4. Klik **Deploy**, lalu buka domain `*.vercel.app` yang diberikan Vercel.
5. Bila deploy awal gagal karena pengaturan build, kosongkan Build Command/Output Directory, lalu redeploy sebagai static site.

## 4. Set alamat website di Supabase
1. Di Supabase buka **Authentication → URL Configuration**.
2. Atur **Site URL** menjadi `https://<alamat-anda>.vercel.app`.
3. Tambahkan URL tersebut pada daftar **Redirect URLs** (gunakan alamat HTTPS yang persis).
4. Coba **Daftar akun** dan konfirmasi email jika aktif, lalu **Masuk**.

## 5. Penggunaan
- Setelah login, ketik TMA (contoh `66,75`) → Enter → prediksi 1 jam muncul.
- Pengecekan otomatis disimpan di `public.tma_checks` milik akun login.
- Riwayat bisa dihapus per baris atau semua. Row Level Security (RLS) membatasi tiap akun membaca/mengubah datanya sendiri.
- UI menampilkan maksimum 500 riwayat terbaru dalam sekali muat. **Data yang lebih lama masih disimpan**, selama tidak dihapus dan kuota database mencukupi.
- Riwayat versi HTML **lokal** lama tidak otomatis dipindah ke database baru.

## Keamanan dan batasan
- Hanya **publishable/anon key** yang boleh berada di `config.js` publik. RLS pada tabel harus tetap aktif.
- Jika aplikasi hanya untuk pribadi, setelah akun dibuat Anda bisa menonaktifkan public sign-up di konfigurasi Authentication Supabase.
- Data terbaru tidak dijamin tersimpan jika koneksi putus; periksa pesan status setelah prediksi. Backup database secara berkala.
- Mesin prediksi masih memakai median selisih dari **373 pasangan observasi sampel**, bukan model yang dilatih/diuji dari seluruh satu tahun TMA.
- Prediksi ini **belum tervalidasi** untuk pengambilan keputusan keselamatan atau operasi pintu bendungan.
- Supabase free dapat memiliki kondisi jeda otomatis atau batas kuota; Vercel Hobby juga memiliki ketentuan penggunaan pribadi nonkomersial.

## Struktur proyek
```
index.html       # frontend dan logika prediksi
config.js        # URL dan publishable/anon key Supabase
logo-odgj.png    # logo asli ODGJ
schema.sql       # tabel + RLS
vercel.json      # konfigurasi static hosting
README.md        # panduan ini
```
