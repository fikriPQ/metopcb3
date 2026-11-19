# E-Maintenance Fasad, versi publik

Website pelaporan kerusakan fasad yang dapat dipakai sungguhan oleh publik.
Tampilannya (`index.html`) dipasang di GitHub Pages, sedangkan datanya disimpan di Google Sheets milik Anda melalui Google Apps Script (`Code.gs`). Keduanya gratis.

- Laporan, perintah kerja, riwayat status, dan akun petugas tersimpan di spreadsheet.
- Foto tersimpan di folder Google Drive **E-Maintenance Fasad - Foto**.
- Kata sandi disimpan dalam bentuk hash, dan pemeriksaan hak akses (pengelola atau teknisi) dilakukan di server.

## Langkah 1: Siapkan server di Google Sheets (sekitar 10 menit)

Pada langkah ini Anda membuat "server" website, yaitu tempat semua laporan disimpan. Server ini berupa sebuah Google Sheet yang diberi kode program (Apps Script). Anda tidak perlu menyewa hosting atau memasang apa pun di komputer. Cukup browser dan akun Google.

Siapkan dua hal:
- Akun Google (Gmail) yang akan menjadi pemilik data, misalnya akun pengelola gedung. Semua laporan dan foto tersimpan di akun ini, jadi sebaiknya jangan memakai akun pribadi yang jarang dibuka.
- Berkas `Code.gs`. Buka dengan Notepad agar isinya bisa disalin.

### 1.1 Buat spreadsheet baru

1. Masuk ke akun Google tersebut di browser (Chrome disarankan).
2. Buka alamat <https://sheets.new>. Spreadsheet kosong baru akan langsung terbuka.
3. Klik tulisan **Spreadsheet tanpa judul** di kiri atas, lalu ganti dengan **Data E-Maintenance Fasad**, dan tekan Enter.

Spreadsheet ini nantinya berisi semua data. Jangan menghapusnya, dan jangan mengubah nama lembar (tab) yang dibuat otomatis oleh skrip.

### 1.2 Buka editor Apps Script

1. Di spreadsheet tadi, klik menu **Ekstensi** (pada tampilan bahasa Inggris: **Extensions**), lalu pilih **Apps Script**.
2. Tab baru terbuka berisi editor kode dengan berkas bernama `Kode.gs` (atau `Code.gs`). Isinya kira-kira seperti ini:

   ```js
   function myFunction() {

   }
   ```

3. Klik **Proyek tanpa judul** di kiri atas, lalu ganti namanya menjadi **Server E-Maintenance Fasad**, dan klik **Ganti nama**.

### 1.3 Tempel kode server

1. Buka berkas `Code.gs` dengan Notepad, tekan **Ctrl+A** untuk memilih semua isinya, lalu **Ctrl+C** untuk menyalin.
2. Kembali ke editor Apps Script. Klik di area kode, tekan **Ctrl+A**, lalu **Delete** untuk menghapus isi bawaan.
3. Tekan **Ctrl+V** untuk menempel kode.
4. Klik ikon **Simpan** (gambar disket) atau tekan **Ctrl+S**.

Pastikan kode yang tertempel diawali tulisan `/**` dan `E-Maintenance Fasad — server Google Apps Script`, lalu diakhiri dengan fungsi `sandiAcak`. Kalau ada bagian yang terpotong, ulangi penyalinan.

### 1.4 Jalankan fungsi "pasang"

Fungsi ini menyiapkan isi spreadsheet: membuat lembar **Laporan**, **PerintahKerja**, **Riwayat**, dan **Pengguna**, membuat folder foto di Google Drive, dan membuat akun pengelola pertama.

1. Di bilah atas editor ada tombol **Jalankan** (ikon segitiga) dan, di sebelahnya, kotak pilihan nama fungsi.
2. Klik kotak pilihan itu, lalu pilih **pasang**.
3. Klik **Jalankan**.

Saat pertama kali dijalankan, Google akan meminta izin:

1. Muncul jendela **Otorisasi diperlukan**. Klik **Tinjau izin**.
2. Pilih akun Google yang tadi dipakai.
3. Muncul peringatan **Google belum memverifikasi aplikasi ini**. Ini wajar untuk skrip buatan sendiri. Klik **Lanjutan** (Advanced) di kiri bawah, lalu klik **Buka Server E-Maintenance Fasad (tidak aman)**.
4. Muncul daftar izin: melihat dan mengelola spreadsheet, melihat dan mengelola berkas Google Drive, serta terhubung ke layanan eksternal. Izin ini dipakai untuk menyimpan laporan dan foto. Klik **Izinkan** (Allow).

Setelah izin diberikan, fungsi berjalan beberapa detik.

### 1.5 Catat kata sandi pengelola

1. Di bagian bawah editor muncul panel **Log eksekusi**. Kalau tidak terlihat, klik **Log eksekusi** di bilah atas.
2. Cari baris seperti ini:

   ```
   Akun pengelola dibuat. Nama pengguna: pengelola   Kata sandi awal: Xy7kQm2PaR9d
   ```

3. Catat kata sandi tersebut. Huruf besar dan kecil dibedakan. Kata sandi ini dipakai untuk masuk pertama kali, lalu segera diganti.

Untuk memastikan semuanya berhasil, kembali ke tab spreadsheet. Di bagian bawah sekarang ada lembar **Laporan**, **PerintahKerja**, **Riwayat**, dan **Pengguna**. Di Google Drive Anda juga ada folder baru **E-Maintenance Fasad - Foto**.

Kalau fungsi dijalankan lagi, data yang sudah ada tidak terhapus. Log hanya menulis bahwa pemasangan sudah pernah dilakukan.

### 1.6 Terbitkan sebagai aplikasi web

Langkah ini membuat alamat (URL) yang dipakai website untuk mengirim dan mengambil data.

1. Di kanan atas editor, klik tombol biru **Terapkan** (Deploy), lalu pilih **Deployment baru** (New deployment).
2. Di samping tulisan **Pilih jenis**, klik ikon roda gigi, lalu pilih **Aplikasi web** (Web app).
3. Isi pengaturannya:
   - **Deskripsi**: misalnya `Versi 1`.
   - **Jalankan sebagai** (Execute as): **Saya** (alamat email Anda). Dengan pilihan ini, data selalu tersimpan di akun Anda.
   - **Yang memiliki akses** (Who has access): **Siapa saja** (Anyone). Pilihan ini wajib supaya pelapor dapat mengirim laporan tanpa akun Google. Data tetap aman karena halaman petugas memakai login.
4. Klik **Terapkan**. Kalau diminta izin lagi, ulangi langkah izin seperti pada 1.4.
5. Muncul jendela **Deployment baru**. Di bagian **Aplikasi web**, salin **URL** yang bentuknya seperti:

   ```
   https://script.google.com/macros/s/AKfycbx...panjang.../exec
   ```

   Klik **Salin**, lalu simpan URL ini di Notepad. URL ini dipakai pada Langkah 2.
6. Klik **Selesai**.

### 1.7 Uji alamat server

Tempel URL tadi di tab browser baru, lalu tekan Enter. Kalau server berjalan, muncul tulisan:

```
{"ok":true,"pesan":"Server E-Maintenance Fasad aktif."}
```

Kalau tulisan itu muncul, Langkah 1 selesai.

### Bila ada masalah pada Langkah 1

| Masalah | Penyebab dan cara mengatasi |
|---|---|
| Menu **Ekstensi** tidak ada | Anda belum masuk ke akun Google, atau berkas dibuka sebagai Excel. Pastikan alamatnya `docs.google.com/spreadsheets`. |
| Pilihan fungsi **pasang** tidak muncul | Kode belum disimpan. Tekan Ctrl+S, tunggu sebentar, lalu buka lagi kotak pilihan fungsi. |
| Tombol **Lanjutan** tidak muncul pada peringatan izin | Akun kampus atau kantor (Google Workspace) kadang melarang skrip buatan sendiri. Pakai akun Gmail biasa, atau minta admin kampus mengizinkannya. |
| Log menulis `Exception: ...` berwarna merah | Biasanya kode tidak tertempel lengkap. Ulangi 1.3, lalu jalankan **pasang** lagi. |
| Kata sandi di Log tidak tercatat atau hilang | Pilih fungsi **aturUlangSandiPengelola**, klik **Jalankan**, dan kata sandi baru akan muncul di Log. |
| Membuka URL menampilkan halaman masuk Google atau "Anda memerlukan akses" | Pengaturan **Yang memiliki akses** belum **Siapa saja**. Buka **Terapkan → Kelola deployment → ikon pensil**, ubah aksesnya, lalu klik **Terapkan**. |
| URL tidak diakhiri `/exec` | Anda menyalin URL pengujian (`/dev`) atau alamat editor. Salin ulang dari **Terapkan → Kelola deployment**. |

## Langkah 2: Hubungkan index.html ke server (memakai VS Code)

Pada langkah ini Anda memberi tahu website alamat server dari Langkah 1. Caranya dengan mengubah beberapa baris di bagian **PENGATURAN** dalam `index.html`. Bagian lain berkas tidak perlu diubah.

Siapkan dua hal:
- URL aplikasi web dari Langkah 1.6, yang diakhiri `/exec`.
- Visual Studio Code (VS Code). Kalau belum ada, unduh gratis dari <https://code.visualstudio.com>, lalu pasang dengan pilihan bawaan. Saat pemasangan, centang **Add "Open with Code" action to Windows Explorer file context menu** agar berkas bisa dibuka lewat klik kanan.

### 2.1 Buka index.html di VS Code

Pilih salah satu cara:
- Di File Explorer, buka folder yang berisi `index.html` (misalnya `E:\laragon\www\emaint-fasad-publik`), klik kanan `index.html`, lalu pilih **Open with Code**.
- Atau buka VS Code, klik menu **File → Open File…** (Ctrl+O), pilih `index.html`, lalu klik **Open**.

Berkas ini panjang (sekitar 1.700 baris) karena tampilan, logo, dan katalog kerusakan ikut di dalamnya. Itu normal.

Kalau muncul jendela **Do you trust the authors of the files in this folder?**, klik **Yes, I trust the authors**.

### 2.2 Temukan bagian PENGATURAN

1. Tekan **Ctrl+F**. Kotak pencarian muncul di kanan atas.
2. Ketik `API_URL:` (dengan titik dua), lalu tekan Enter. VS Code melompat ke baris yang dicari dan menandainya.
3. Tekan **Esc** untuk menutup kotak pencarian.

Cara lain: tekan **Ctrl+G**, ketik `550`, lalu tekan Enter untuk langsung ke baris itu. Nomor baris terlihat di sisi kiri editor.

Bagian yang Anda temukan terlihat seperti ini:

```js
window.KONFIGURASI = {
  API_URL: '',
  INSTANSI: 'Politeknik Negeri Semarang',
  TELEPON: '+6289649460633',
  GEDUNG: [
    {id: 'G8', nama: 'Gedung Perkuliahan 8 Lantai', lantai: 8, tahun: 2023, sisi: ['Utara', 'Timur', 'Selatan', 'Barat']},
    {id: 'G3', nama: 'Gedung Perkuliahan 3 Lantai', lantai: 3, tahun: 2016, sisi: ['Utara', 'Timur', 'Selatan', 'Barat']},
  ],
};
```

### 2.3 Tempel URL server

1. Klik tepat **di antara dua tanda kutip** pada baris `API_URL: '',`, yaitu di antara `'` dan `'`.
2. Tekan **Ctrl+V** untuk menempel URL dari Langkah 1.6.
3. Hasilnya harus seperti ini:

   ```js
     API_URL: 'https://script.google.com/macros/s/AKfycbx...panjang.../exec',
   ```

Periksa empat hal:
- URL diapit **tanda kutip tunggal** `'` di depan dan di belakang.
- Tanda **koma** di akhir baris masih ada.
- Tidak ada spasi atau baris baru di dalam URL.
- URL diakhiri `/exec`, bukan `/dev` atau `/edit`.

Kalau URL sangat panjang dan baris memanjang ke kanan, tekan **Alt+Z** untuk membungkus tampilan baris. Ini hanya mengubah tampilan, bukan isi berkas.

### 2.4 Sesuaikan instansi, telepon, dan gedung

Masih di bagian yang sama, ubah teks di antara tanda kutip sesuai keadaan sebenarnya:

| Isian | Arti | Contoh |
|---|---|---|
| `INSTANSI` | Nama instansi di bawah logo | `'Politeknik Negeri Semarang'` |
| `TELEPON` | Nomor pengelola di bagian bawah halaman publik, dengan format +62 | `'+6289649460633'` |
| `GEDUNG` | Daftar gedung yang dapat dipilih pelapor | lihat penjelasan di bawah |

Setiap gedung ditulis dalam satu baris di antara `{` dan `},`:

```js
{id: 'G8', nama: 'Gedung Perkuliahan 8 Lantai', lantai: 8, tahun: 2023, sisi: ['Utara', 'Timur', 'Selatan', 'Barat']},
```

- `id`: kode singkat gedung, tanpa spasi, misalnya `'G8'` atau `'GKB'`. **Jangan mengubah id setelah website dipakai**, karena laporan lama mencatat gedung berdasarkan id ini.
- `nama`: nama gedung yang tampil di formulir.
- `lantai`: jumlah lantai, ditulis sebagai angka tanpa tanda kutip. Angka ini menentukan jumlah baris petak pada gambar tampak gedung.
- `tahun`: tahun gedung selesai dibangun, untuk menghitung umur gedung di dasbor.
- `sisi`: sisi gedung (elevasi) yang dapat dipilih pelapor. Contohnya `['Depan', 'Belakang', 'Kiri', 'Kanan']` atau `['Utara', 'Timur Laut', 'Selatan']`.

Untuk **menambah gedung**, salin satu baris gedung, tempel di bawahnya (masih di dalam `[` dan `]`), lalu ubah isinya. Untuk **menghapus gedung**, hapus satu baris penuh dari `{` sampai `},`.

Contoh tiga gedung:

```js
  GEDUNG: [
    {id: 'G8', nama: 'Gedung Perkuliahan 8 Lantai', lantai: 8, tahun: 2023, sisi: ['Utara', 'Timur', 'Selatan', 'Barat']},
    {id: 'G3', nama: 'Gedung Perkuliahan 3 Lantai', lantai: 3, tahun: 2016, sisi: ['Utara', 'Timur', 'Selatan', 'Barat']},
    {id: 'GKU', nama: 'Gedung Kuliah Umum', lantai: 4, tahun: 2010, sisi: ['Depan', 'Belakang', 'Kiri', 'Kanan']},
  ],
```

### 2.5 Simpan berkas

Tekan **Ctrl+S**. Tanda titik putih di judul tab `index.html` hilang, artinya perubahan sudah tersimpan.

Untuk berjaga-jaga, Anda bisa menyimpan salinan sebelum mengubah apa pun, lewat **File → Save As…**, dengan nama misalnya `index-asli.html`.

### 2.6 Coba di komputer sebelum diunggah

1. Di File Explorer, klik dua kali `index.html` agar terbuka di browser. Bisa juga klik kanan berkas di panel kiri VS Code, lalu pilih **Reveal in File Explorer**.
2. Halaman lapor kerusakan seharusnya tampil. Kalau masih tertulis **Server belum diatur**, berarti `API_URL` belum tersimpan atau masih kosong.
3. Klik **Masuk petugas**, lalu masuk dengan nama pengguna `pengelola` dan kata sandi dari Langkah 1.5. Kalau dasbor tampil, website sudah terhubung ke server.
4. Agar yakin, kirim satu laporan percobaan dari halaman depan, lengkap dengan foto. Laporan itu akan muncul sebagai baris baru di lembar **Laporan** di spreadsheet, dan fotonya masuk ke folder **E-Maintenance Fasad - Foto** di Google Drive. Laporan percobaan dapat dihapus lewat **Daftar laporan → Bersihkan laporan**.

### Bila ada masalah pada Langkah 2

| Masalah | Penyebab dan cara mengatasi |
|---|---|
| Halaman putih kosong atau hanya bertuliskan "Memuat E-Maintenance Fasad…" | Ada tanda kutip, koma, atau kurung yang terhapus di bagian PENGATURAN. Bandingkan dengan contoh di 2.2. VS Code biasanya memberi garis merah bergelombang di bawah bagian yang salah. Kalau perlu, buka lagi berkas asli dan ulangi. |
| Tetap muncul **Server belum diatur** | `API_URL` masih kosong atau belum disimpan. Ulangi 2.3 lalu tekan Ctrl+S. |
| Muncul **Tidak dapat terhubung ke server** | Periksa koneksi internet, lalu pastikan URL yang ditempel lengkap dan diakhiri `/exec`. |
| Muncul **Server tidak memberi jawaban yang dapat dibaca** | Deployment Apps Script belum diatur ke akses **Siapa saja**. Lihat tabel masalah pada Langkah 1. |
| Gedung baru tidak muncul | Barisnya ditulis di luar `[` dan `]`, atau koma di akhir baris sebelumnya terhapus. |

## Langkah 3: Pasang di GitHub Pages

1. Buat repositori baru di GitHub, lalu unggah `index.html`. `PANDUAN.md` boleh ikut diunggah.
   **Jangan unggah `Code.gs`** ke repositori publik. Berkas ini tidak berisi rahasia, tetapi juga tidak diperlukan di sana.
2. Buka **Settings → Pages**, pilih **Deploy from a branch**, branch `main`, folder `/ (root)`, lalu **Save**.
3. Sekitar satu sampai dua menit kemudian, website terbuka di `https://nama-akun.github.io/nama-repositori/`.

## Langkah 4: Mulai memakai

1. Buka website, klik **Masuk petugas**, lalu masuk dengan nama pengguna `pengelola` dan kata sandi dari Log eksekusi.
2. Segera buka **Ganti kata sandi** di menu samping.
3. Buka **Admin → Pengguna** untuk membuat akun teknisi. Sampaikan kata sandi awalnya kepada mereka dan minta mereka menggantinya.
4. Bagikan alamat website kepada penghuni gedung. Pelapor tidak perlu akun.

## Pertanyaan umum

**Lupa kata sandi pengelola?** Buka Apps Script, jalankan fungsi `aturUlangSandiPengelola`, lalu lihat kata sandi baru di Log eksekusi. Kata sandi teknisi dapat diatur ulang oleh pengelola dari menu Pengguna.

**Mengubah Code.gs setelah dipasang?** Simpan perubahan, lalu buka **Terapkan → Kelola deployment → ikon pensil → Versi: Versi baru → Terapkan**. URL-nya tetap sama.

**Muncul pesan "Tidak dapat terhubung ke server"?** Periksa bahwa URL di `API_URL` diakhiri `/exec` dan deployment diatur ke "Siapa saja".

**Batas penggunaan?** Akun Google gratis cukup untuk ratusan laporan per hari. Spreadsheet tetap cepat sampai beberapa ribu laporan. Bersihkan laporan lama yang sudah selesai dari menu Daftar laporan bila perlu.

**Mengolah data untuk penelitian?** Buka spreadsheet, lalu pilih **File → Download → Microsoft Excel**. Tautan ke spreadsheet juga ada di dasbor pengelola.
