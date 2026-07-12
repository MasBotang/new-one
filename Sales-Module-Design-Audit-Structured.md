# ERP v1.0 — Sales Module
## Premium UI/UX Design Audit (Structured — Evidence-Based)

**Fokus:** Kualitas pengalaman & tampilan yang dilihat/dirasakan pengguna akhir — bukan implementasi teknis.
**Metodologi:** Setiap temuan wajib menyebut Komponen, Lokasi, Masalah, dan Dampak, serta diberi level prioritas. Jika sebuah elemen/fitur tidak ditemukan pada implementasi yang diperiksa, laporan menyatakan **"Tidak ditemukan pada implementasi saat ini"** — tidak ada asumsi.

**Skala Prioritas:** 🔴 P0 Critical · 🟠 P1 High · 🟡 P2 Medium · 🔵 P3 Low

---

## Design System Inventory

Audit variasi komponen visual di seluruh Sales Module (Sales Home, POS, Checkout, Shift/Settlement, Refund, Sales History):

| Komponen | Jumlah Variasi yang Ditemukan | Catatan Konsistensi |
|---|---|---|
| **Button** | Sistem baku: 6 gaya (Primer, Destructive, Outline, Secondary, Ghost, Link) × 4 ukuran. Di luar sistem baku ini, ditemukan ≥5 kontrol bergaya tombol yang dirancang custom: chip kategori (pil gradasi), ubin metode pembayaran, toggle End User/Reseller, toggle Frozen/Matang, tombol stepper qty bulat. | Sistem dasar rapi dan terbatas, tetapi banyak titik interaksi penting justru keluar dari sistem tersebut. |
| **Card** | Ditemukan card dasar (shadow tipis, radius besar) dipakai konsisten sebagai wadah, tapi isi/gaya "kartu metrik" bervariasi luas — lihat baris **KPI Card** di bawah. | Wadah kartu konsisten; isi kartu metrik tidak. |
| **Badge / Status Pill** | 9 varian warna baku (Default, Secondary, Destructive, Outline, Success, Warning, Info, Gold, Muted) berbentuk kotak bersudut lembut, huruf kapital berjarak. Ditambah **1 bentuk kustom terpisah**: pil membulat penuh dengan titik warna kecil, dipakai khusus untuk status pesanan di Riwayat Transaksi. | Dua bahasa bentuk berbeda (kotak vs pil bulat) untuk konsep yang sama: menandai status. |
| **Dialog** | 4 profil ukuran berbeda ditemukan: (1) default tanpa lebar eksplisit — dipakai di dialog Refund; (2) lebar sedang-kecil — dialog Pembayaran & Transaksi Berhasil di POS; (3) lebar sedang — satu dialog di Shift; (4) lebar besar dengan scroll internal — dialog Detail Shift. | Tidak terlihat skala ukuran (kecil/sedang/besar) yang diterapkan secara sengaja berdasarkan kompleksitas konten. |
| **Drawer (Sheet)** | 1 penggunaan ditemukan: panel "Atur Packing" di POS. | Sampel terlalu sedikit untuk menilai konsistensi pola drawer di seluruh modul. |
| **Input** | Input teks dengan ikon kiri (Customer, Telepon, Catatan), input angka khusus untuk uang, dropdown Select — seluruhnya konsisten *di dalam* layar POS. | Konsisten di POS; belum diverifikasi setara di layar Shift/Refund. |
| **Dropdown (Select)** | 1 gaya baku, dipakai konsisten untuk Source Order (POS) dan filter Outlet/Marketplace/Kasir (Riwayat Transaksi). | Konsisten. |
| **Search** | 2 perlakuan berbeda: kolom pencarian besar berdiri sendiri dengan ikon dan penanda tombol pintasan (POS), vs kolom pencarian berukuran setara dengan filter lain dalam satu kartu (Riwayat Transaksi). | Search tidak diperlakukan sebagai elemen hierarki tertinggi secara merata. |
| **Filter** | 2 paradigma berbeda: filter tunggal berbasis pil horizontal (kategori produk di POS) vs kartu filter berisi 5 kontrol sekaligus (Riwayat Transaksi: cari, tanggal, outlet, marketplace, kasir). | Wajar karena konteks beda, tapi bahasa visualnya juga ikut berbeda total. |
| **KPI Card** | Sedikitnya **6 gaya visual berbeda** ditemukan: (1) kartu hero gradasi gelap dengan dekorasi garis SVG (Beranda), (2) kartu metrik mini transparan di dalam kartu hero tsb (Beranda), (3) kartu status beraksen warna kiri (Beranda), (4) kotak metrik sederhana bergaris (Beranda — panel Shift Aktif), (5) kartu KPI yang sekaligus berfungsi sebagai tombol filter (Riwayat Transaksi), (6) kartu metrik hero terpisah di layar Tutup Shift, ditambah kartu rekonsiliasi pembayaran bergaya lain lagi di layar yang sama. | Konsep "menampilkan satu angka penting" dirancang ulang di hampir setiap layar alih-alih dari satu pola yang dipakai berulang. |
| **Table** | 2 pola berbeda: tabel data baku dengan header/baris standar (dalam dialog Detail Shift), dan daftar bergaya kartu yang meniru tampilan tabel namun bukan tabel sesungguhnya (Riwayat Transaksi). | **Tidak ditemukan** kontrol sortir kolom atau paginasi pada implementasi manapun dalam modul ini. |
| **Empty State** | 2 kualitas berbeda: pola lengkap (ikon bulat + judul + deskripsi + saran tindakan) di Riwayat Transaksi dan grid produk POS, vs pola teks polos satu baris di dalam tabel dialog Detail Shift. | Kualitas empty state tidak merata di seluruh modul. |
| **Loading State** | **Tidak ditemukan** skeleton, shimmer, atau spinner loading pada implementasi manapun yang diperiksa dalam Sales Module. | Seluruh layar menampilkan data seolah selalu tersedia instan. |

---

## Halaman: Sales Home

**Kekuatan yang teramati:** kartu "Revenue Hero" (angka omset hari ini) memakai gradasi warna gelap kustom, dekorasi garis SVG halus, dan grafik garis tipis di bagian bawah kartu sebagai latar dekoratif — tingkat detail visual ini melampaui rata-rata dashboard admin dan terasa dirancang khusus, bukan hasil template generik. Sapaan personal ("Selamat datang kembali, {nama}") memberi nuansa hangat yang sejalan dengan gaya butik keseluruhan produk.

### UI Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟡 P2 | Header halaman | Bagian paling atas layar Beranda | Tidak ada breadcrumb/penanda modul, berbeda dari layar Pusat Pesanan yang menampilkan jejak "Sales › Pesanan" | Pengguna kehilangan penanda "sedang berada di modul apa" saat pertama mendarat di aplikasi |
| 🟡 P2 | Kartu "Revenue Hero", Kartu Status Pesanan, Kotak metrik Shift Aktif | Berurutan dari atas ke bawah pada satu halaman yang sama | Tiga gaya kartu visual berbeda (gradasi gelap dekoratif, kartu putih beraksen warna kiri, kotak sederhana bergaris) dipakai berdampingan untuk menampilkan data yang secara konsep sejenis (metrik/angka) | Halaman terasa seperti gabungan beberapa modul kecil yang berbeda perancang, bukan satu sistem visual tunggal |
| 🔵 P3 | Label "Data Hari Ini" dengan indikator titik hijau berdenyut | Pojok kanan atas header | Animasi denyut hijau permanen tanpa keterangan tambahan tentang makna status "live" tersebut | Elemen dekoratif berisiko disalahartikan sebagai indikator koneksi/sinkronisasi real-time yang sesungguhnya tidak dijelaskan ke pengguna |

### UX Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟠 P1 | Kartu Aksi Cepat "Buat Pesanan" | Bagian paling bawah layar Beranda | Ini adalah satu-satunya jalur cepat menuju POS dari Beranda, tetapi diletakkan setelah tiga blok informasi lain (Revenue Hero, Status Pesanan, panel Shift) | Pengguna harus melewati banyak konten sebelum menemukan aksi harian yang paling sering mereka butuhkan |
| 🟡 P2 | Progress bar Target Harian | Dalam kartu Revenue Hero | Bila target belum diset, kolom untuk mengisi target hanya muncul bagi peran admin/owner — pengguna kasir akan melihat progress bar kosong tanpa penjelasan dan tanpa aksi yang bisa dilakukan | Elemen visual yang terasa "buntu" bagi mayoritas pengguna harian (kasir) |
| 🔵 P3 | Metrik "Refund" pada grid mini-metrik Revenue Hero | Kuadran kanan-bawah grid 2×2 dalam kartu hero | Angka refund ditampilkan setara secara visual dengan metrik netral lain (Pesanan, Rata-rata, Selesai), padahal secara bisnis nilai refund > 0 adalah sinyal yang perlu perhatian | Owner/admin tidak diarahkan matanya secara visual ke potensi masalah operasional |

### Quick Wins
- Tambahkan breadcrumb "Sales" tepat di atas judul sapaan.
- Pindahkan kartu Aksi Cepat "Buat Pesanan" ke posisi lebih atas (langsung di bawah header), mengingat ini adalah aksi harian utama pengguna kasir.
- Beri aksen warna berbeda pada angka Refund di dalam Revenue Hero saat nilainya lebih dari 0.

---

## Halaman: POS — Product Grid & Cart

**Kekuatan yang teramati:** kolom pencarian besar dengan indikator pintasan keyboard yang terlihat langsung di dalam kolomnya sendiri; kartu produk dengan latar radial gradient, dekorasi botanikal halus, badge kuantitas di keranjang, dan badge stok yang hanya muncul saat relevan (habis/menipis) — ini adalah tingkat polish yang jarang ditemukan di POS kelas menengah. Baris item keranjang dengan stepper kuantitas bulat dan area subtotal yang sejajar rapi juga tergolong sangat baik secara eksekusi visual.

### UI Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟠 P1 | Panel Keranjang & tombol "Bayar" | Kolom kanan layar POS, pada lebar layar di bawah ukuran laptop/tablet lanskap | Tata letak berubah menjadi satu kolom vertikal — grid produk dan panel keranjang bertumpuk, tombol "Bayar" ikut turun ke bawah tanpa ringkasan total yang tetap terlihat (sticky) di layar | Pada layar sempit, kasir harus scroll melewati seluruh grid produk untuk mencapai tombol checkout |
| 🟡 P2 | Chip kategori, ubin metode pembayaran, toggle End User/Reseller, toggle Frozen/Matang | Tersebar di grid produk (atas) dan panel keranjang (kanan) | Empat bentuk visual berbeda dipakai untuk konsep interaksi yang sama — memilih satu dari beberapa opsi (pil penuh bergradasi, ubin kotak berikon, segmented rectangular, ikon-only square) | Pengguna baru membutuhkan waktu lebih lama mengenali bahwa keempat kontrol ini sejenis fungsinya |
| 🔵 P3 | Ikon kategori otomatis | Pil kategori di atas grid produk | Ikon ditentukan otomatis berdasarkan nama kategori; nama kategori yang tidak lazim akan jatuh ke ikon generik | Sebagian kategori terasa "kurang personal" dibanding kategori umum yang sudah punya ikon spesifik |

### UX Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟠 P1 | Panel pengaturan pesanan (Customer, No. Telepon, Catatan, Source Order, Frozen/Matang, Pre Order) | Bagian atas panel Keranjang, sebelum daftar item | Enam blok input/kontrol tampil sekaligus di setiap transaksi, meski mayoritas transaksi harian kemungkinan tidak memerlukan Pre Order atau perubahan Source Order | Beban visual & kognitif tinggi di titik yang paling sering dipakai kasir (setiap transaksi tunggal) |
| 🟡 P2 | Kolom pencarian produk | Atas grid produk | Tidak ada indikator tekstual jumlah hasil ("X produk ditemukan") — kepastian hasil pencarian sepenuhnya bergantung pada perubahan grid | Mengurangi kepastian instan bagi kasir saat mengetik query sangat spesifik dengan hasil sedikit |
| 🔵 P3 | Strip "Hold" (pesanan ditahan) | Baris horizontal di atas kolom pencarian, muncul saat ada hold tersimpan | Tidak ada keterangan durasi/waktu sejak pesanan ditahan, hanya nama pelanggan | Kasir tidak tahu apakah ada hold yang sudah lama menumpuk dan perlu ditindaklanjuti |

### Quick Wins
- Tambahkan bar ringkasan total + tombol bayar yang menempel (sticky) di bagian bawah layar sempit/tablet portrait.
- Sembunyikan Source Order, Pre Order, dan Frozen/Matang di balik satu titik masuk opsional ("Opsi Lanjutan"), dengan alur default hanya menampilkan Customer + Catatan.
- Satukan bentuk visual kontrol pemilihan menjadi maksimal dua bahasa: satu untuk grup pilihan besar (kategori, metode bayar), satu untuk toggle kecil (Frozen/Matang, End User/Reseller).

---

## Halaman: Checkout & Payment

**Kekuatan yang teramati:** label tombol checkout utama berubah secara dinamis untuk menjelaskan alasan ketika dinonaktifkan (mis. "Buka shift untuk bertransaksi", "Alokasi Packing kurang N") — ini adalah pola komunikasi kesalahan yang justru lebih baik daripada notifikasi sekilas biasa, karena melekat langsung pada aksi yang diblokir. Label "Kekurangan"/"Kembalian" yang berubah warna instan (merah/hijau) sesuai kondisi nominal juga sangat efektif untuk dipindai cepat.

### UI Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟡 P2 | Dialog Pembayaran | Muncul di tengah layar saat tombol "Bayar" ditekan | Ukuran dialog (sedang-kecil) berbeda dari dialog rekonsiliasi Shift (besar dengan scroll internal), meski keduanya sama-sama proses finansial dalam satu modul | Menegaskan tidak adanya skala ukuran dialog yang diterapkan secara sengaja berdasarkan kompleksitas konten |
| 🔵 P3 | Grid empat ubin metode pembayaran | Bagian atas Dialog Pembayaran | Keempat ubin (Tunai/QRIS/Kartu/Transfer) berukuran identik, padahal preset nominal cepat hanya tersedia khusus untuk Tunai — mengindikasikan Tunai adalah metode dominan | Hierarki visual ubin tidak mencerminkan hierarki frekuensi penggunaan riil |

### UX Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟡 P2 | Chip preset nominal cepat | Bawah kolom "Uang Tunai Diterima" | Preset yang tersedia hanya Rp500–Rp50.000, bersifat akumulatif; tidak ada preset untuk nominal besar umum seperti Rp100.000 | Transaksi bernilai lebih besar (di atas ratusan ribu) tetap harus diketik manual atau mengklik preset kecil berkali-kali |
| 🔵 P3 | Dialog "Transaksi Berhasil" | Muncul otomatis setelah checkout selesai | Tidak ada opsi gabungan "Cetak & Transaksi Baru" — kasir harus menutup dialog secara terpisah dari aksi cetak sebelum melayani pelanggan berikutnya | Satu langkah tambahan berulang pada alur kerja berkecepatan tinggi |

### Quick Wins
- Tambahkan preset nominal lebih besar (mis. Rp100.000) pada kolom uang tunai.
- Pertimbangkan tombol gabungan "Cetak & Transaksi Baru" pada dialog Transaksi Berhasil.

---

## Halaman: Shift & Settlement (termasuk Rekonsiliasi Marketplace)

**Kekuatan yang teramati:** alur Tutup Shift dirancang sebagai wizard bernomor 4 langkah (Pengeluaran Operasional → Audit Packaging → Rekonsiliasi Pembayaran → Rekonsiliasi Marketplace) dengan kartu-kartu yang memberi umpan balik warna instan (selisih hijau/merah) — ini adalah eksekusi setara alat rekonsiliasi keuangan kelas menengah-atas, jauh melampaui ekspektasi "fitur tutup kasir" pada POS umumnya.

### UI Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟡 P2 | Tabel "Pengeluaran Operasional" & "Pemakaian Packaging" | Di dalam dialog Detail Shift (riwayat shift yang sudah ditutup) | Saat data kosong, tabel hanya menampilkan satu baris teks polos ("Tidak ada pengeluaran.") — berbeda dari pola empty state (ikon + judul + deskripsi) yang dipakai di Riwayat Transaksi dan grid produk POS | Kesan "belum selesai dipoles" saat dibandingkan dengan layar lain dalam modul yang sama |
| 🔵 P3 | Kartu metrik ringkas pada header wizard Tutup Shift | Bagian atas layar Tutup Shift | Gaya visual mirip namun tidak identik dengan kartu metrik mini pada Revenue Hero di Beranda (detail shadow dan warna latar berbeda) | Dua "gaya kartu metrik kecil" yang nyaris sama tapi tidak sepenuhnya konsisten saat pengguna berpindah dari Beranda ke Shift |

### UX Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟡 P2 | Kolom input "Settlement" pada Rekonsiliasi Marketplace | Step 4 wizard Tutup Shift, satu baris per marketplace | Kolom input angka tidak menampilkan pemisah ribuan secara real-time saat diketik | Nominal besar sulit dibaca sekilas saat sedang mengetik, meningkatkan risiko salah input |
| 🔵 P3 | Kartu "Audit Status" (Siap Ditutup/Perlu Ditinjau/Perlu Perbaikan) | Kartu metrik ke-4 pada header wizard | Label status tidak dapat diklik untuk langsung berpindah ke step yang bermasalah | Pengguna harus scroll manual mencari step penyebab status "Perlu Perbaikan" |

### Quick Wins
- Samakan pola empty state tabel di dalam dialog Detail Shift dengan pola ikon+judul+deskripsi yang sudah dipakai di Riwayat Transaksi.
- Tambahkan pemisah ribuan otomatis saat mengetik pada kolom Settlement Marketplace.
- Jadikan kartu status audit "Perlu Perbaikan" dapat diklik untuk auto-scroll ke step terkait.

---

## Halaman: Refund

**Kekuatan yang teramati:** pemisahan warna dan posisi yang tegas antara tombol "Tolak" (gaya destructive/merah) dan "Setujui Refund" (gaya primer) pada dialog peninjauan — pola ini secara efektif mencegah kesalahan klik pada aksi finansial yang konsekuensinya besar.

### UI Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🔵 P3 | Dialog "Ajukan Refund" & "Review Pengajuan Refund" | Muncul dari panel Detail Pesanan di layar Riwayat Transaksi | Kedua dialog memakai ukuran default aplikasi (tanpa lebar eksplisit), berbeda dari dialog lain di modul yang sama (Pembayaran, Detail Shift) yang lebarnya diatur secara sengaja | Konsisten dengan temuan skala ukuran dialog yang belum baku di seluruh modul |

### UX Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟡 P2 | Kolom "Alasan" | Isi Dialog Ajukan Refund | Satu-satunya input dan bersifat teks bebas tanpa kategori/alasan standar (mis. "Salah pesan", "Barang rusak", "Pembatalan pelanggan") | Data alasan refund akan sulit dianalisis/dikelompokkan oleh owner di kemudian hari karena formatnya tidak seragam antar kasir |
| 🔵 P3 | Validasi kolom "Alasan" | Dialog Ajukan Refund | Peringatan "wajib diisi" hanya muncul sebagai notifikasi sekilas di pojok layar, bukan sebagai teks bantuan di bawah kolom Textarea | Kurang tertaut langsung ke kolom yang bermasalah, meski dampaknya rendah karena hanya satu kolom dalam form |

### Quick Wins
- Ubah kolom Alasan menjadi kombinasi dropdown kategori + teks bebas opsional untuk detail tambahan.
- Tambahkan teks bantuan berwarna di bawah Textarea saat kosong, sebagai pelengkap notifikasi sekilas yang sudah ada.

---

## Halaman: Sales History / Pusat Pesanan (termasuk Pre Order)

**Kekuatan yang teramati:** panel Detail Pesanan tetap terlihat (sticky) saat daftar di sisi kiri di-scroll — pola "list + detail yang menempel" ini setara pengalaman pada Linear/Gmail dan jauh lebih efisien dibanding pola lama "klik → pindah halaman". Kartu KPI yang sekaligus berfungsi sebagai filter status adalah keputusan desain yang efisien dan jarang ditemukan di ERP kelas menengah. Timeline vertikal status pesanan (Baru → Diproses → Siap → Selesai) sangat jelas dan mudah dipindai.

### UI Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟠 P1 | Daftar Pesanan | Kolom kiri (± 58% lebar) pada layar Pusat Pesanan | **Tidak ditemukan** kontrol sortir kolom (mis. urutkan berdasarkan total atau waktu) dan **tidak ditemukan** kontrol paginasi — seluruh hasil filter dimuat sekaligus dalam satu area scroll internal | Pengguna power (owner/admin) yang meninjau banyak transaksi tidak punya cara mengurutkan ulang selain urutan bawaan (terbaru dulu) |
| 🟡 P2 | Tab status pada header daftar Pesanan | Tepat di atas daftar, sisi kiri, di bawah kartu-kartu KPI | Hanya menampilkan 3 dari 5 status (Semua, Baru, Diproses) — status "Selesai" dan "Refund" tidak memiliki tab di kontrol ini meski keduanya punya kartu KPI terpisah tepat di atasnya yang juga berfungsi sebagai filter | Dua kontrol filter berbeda untuk konsep yang sama menimbulkan ambiguitas mana yang menjadi acuan utama |
| 🟡 P2 | Kolom pencarian | Kartu filter, posisi pertama dari 5 kontrol filter | Kolom pencarian berukuran visual setara dengan filter Tanggal/Outlet/Marketplace/Kasir di sebelahnya, padahal fungsinya jauh lebih sering dipakai | Pencarian tidak "menang" secara visual sebagai alat utama, berbeda dari pola pencarian besar berdiri sendiri di POS |

### UX Findings

| Prioritas | Komponen | Lokasi | Masalah | Dampak |
|---|---|---|---|---|
| 🟠 P1 | Daftar Pesanan (kiri) dan Panel Detail (kanan) | Area kerja utama layar Pusat Pesanan | Kedua kolom memiliki area scroll internal yang independen satu sama lain, berdampingan dalam satu layar | Berpotensi membingungkan pengguna yang men-scroll satu sisi namun mengira sisi lain ikut bergerak |
| 🔵 P3 | Tab "Pesanan" / "Pre Order" | Pojok kanan atas header | Berpindah antar dua tab ini mengganti seluruh area kerja (dari daftar+detail menjadi kalender bulanan) tanpa transisi/animasi halus | Perpindahan terasa sedikit "patah", meski dampaknya rendah karena frekuensi perpindahan tab ini tidak tinggi |

### Quick Wins
- Tambahkan status "Selesai" dan "Refund" ke tab segmented di header daftar agar sinkron dengan kartu KPI di atasnya.
- Tonjolkan kolom pencarian secara visual dibanding 4 filter lain (lebar lebih besar dan/atau posisi terpisah).
- Tambahkan kontrol urutkan sederhana (terbaru/terlama/total tertinggi) di atas daftar.

---

## Premium Experience Review

**Konteks penilaian:** produk ini diposisikan sebagai ERP SaaS dengan setup Rp15–25 juta atau langganan Rp500 ribu–2 juta/bulan — level harga yang sejajar dengan software B2B profesional, bukan tools gratis/starter.

**Apa yang MASIH membuatnya terasa seperti admin template / dashboard gratis / project internal:**
- **Tabel tanpa sortir dan paginasi.** Ini adalah kesenjangan paling mencolok relatif terhadap harga yang ditawarkan — pembeli enterprise di kisaran harga ini akan otomatis mencoba mengklik header kolom dan mengharapkan navigasi halaman, bukan scroll tanpa akhir. Fitur ini adalah standar minimum di hampir semua dashboard SaaS berbayar, termasuk yang jauh lebih murah dari kisaran harga produk ini.
- **Ketiadaan bahasa visual untuk kondisi loading.** Produk seharga puluhan juta rupiah setup diharapkan terasa "dipoles" di setiap momen transisi, termasuk saat menunggu data — kekosongan total pada aspek ini terasa seperti proyek yang belum melewati tahap polish akhir.
- **Enam gaya kartu metrik berbeda** dalam satu modul (lihat Design System Inventory) memberi kesan produk dirakit dari beberapa iterasi desain berbeda yang tidak pernah disatukan kembali — sebuah software enterprise yang matang biasanya punya satu "bahasa KPI" yang dipakai ulang di mana pun angka penting ditampilkan.

**Apa yang SUDAH terlihat seperti software enterprise profesional:**
- **Wizard Tutup Shift 4 langkah dengan rekonsiliasi finansial real-time** (selisih kas, selisih settlement marketplace dengan indikator warna) — ini adalah kualitas fitur yang sejajar dengan software akuntansi/reconciliation kelas menengah-atas, bukan sekadar "form tutup kasir".
- **Detail kecil yang konsisten bernilai tinggi**: seluruh angka finansial rata digit (tabular), preset uang tunai yang relevan dengan pecahan mata uang lokal, label tombol checkout yang menjelaskan diri sendiri saat dinonaktifkan.
- **Identitas visual yang berani berbeda** — gradasi warna kustom, dekorasi garis SVG halus pada kartu hero dan kartu produk, animasi hover yang halus (lift + shadow membesar) — ini adalah usaha desain yang jauh melampaui "pasang komponen dari library lalu selesai".
- **Alur kerja kasir yang dipikirkan end-to-end**: dari pencarian produk, hold pesanan, checkout, hingga cetak struk — seluruhnya terasa dirancang berdasarkan pemahaman nyata terhadap alur kerja kasir F&B, bukan asumsi generik.

---

## Modern SaaS Assessment (Standar Kualitas — Bukan Peniruan Desain)

| Produk Acuan | Aspek yang Dijadikan Standar | Posisi Sales Module |
|---|---|---|
| **Stripe Dashboard** | Detail finansial presisi (rata digit, warna semantik konsisten) | Sudah setara pada level komponen individual (POS, dialog pembayaran); belum setara pada level konsistensi lintas halaman |
| **Linear** | Satu bahasa visual yang dipakai ulang di semua layar; tabel dengan sortir & keyboard-first | Keyboard-first sudah kuat di POS; sortir tabel **tidak ditemukan** di manapun |
| **Shopify Admin** | Tabel data dengan paginasi, badge status yang seragam bentuknya | Paginasi **tidak ditemukan**; badge status memiliki 2 bentuk berbeda (lihat Design System Inventory) |
| **Notion** | Empty state yang membantu dan konsisten di semua tempat | Kualitas empty state tidak merata — pola lengkap di sebagian tempat, teks polos di tempat lain |
| **Vercel Dashboard** | Loading state yang halus (skeleton) di setiap titik data | **Tidak ditemukan** loading state dalam bentuk apa pun pada implementasi yang diperiksa |

---

## Final Deliverable — Ringkasan Skor

| Area | Score | Priority |
|---|---|---|
| Visual Design | 7.5 | 🟡 P2 |
| Layout | 6.5 | 🟠 P1 |
| Typography | 7.5 | 🔵 P3 |
| Navigation | 6.5 | 🟡 P2 |
| Tables | 5.0 | 🟠 P1 |
| Forms | 7.0 | 🟡 P2 |
| Search | 6.0 | 🟡 P2 |
| Filter | 6.5 | 🟡 P2 |
| Dialog | 6.0 | 🟡 P2 |
| Empty State | 6.5 | 🔵 P3 |
| Loading | 4.5 | 🟡 P2 |
| Consistency | 5.5 | 🟠 P1 |
| Premium Feeling | 7.3 | 🟡 P2 |
| **Overall UI** | **6.5** | — |

**Catatan:** Tidak ditemukan temuan berlevel 🔴 P0 (Critical) pada audit ini — tidak ada elemen yang merusak kegunaan dasar produk atau menciptakan kesan tidak profesional secara drastis pada pandangan pertama. Prioritas tertinggi (🟠 P1) terkonsentrasi pada tiga area yang saling terkait: **Layout** (checkout mobile tanpa ringkasan menempel), **Tables** (tidak ada sortir/paginasi), dan **Consistency** (proliferasi gaya kartu KPI serta ukuran dialog yang tidak baku) — memperbaiki ketiganya akan memindahkan persepsi produk secara paling signifikan dari "dashboard yang indah di setiap layar individual" menjadi "sistem SaaS premium yang koheren secara menyeluruh".
