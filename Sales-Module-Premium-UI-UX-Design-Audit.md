# ERP v1.0 — Sales Module
## Premium UI/UX Design Audit

**Peran Audit:** Principal Product Designer / Head of Design / Senior UX Researcher / SaaS Design Architect / Design System Lead
**Fokus:** Kualitas desain produk yang terlihat dan dirasakan pengguna — Sales Home, POS, Cart, Checkout, Payment, Marketplace, Settlement, Refund, Shift, Sales History, dialog, drawer, table, form, search, filter, empty state, loading, mobile/tablet/desktop layout, navigasi.
**Di luar cakupan (sesuai arahan):** implementasi teknis, kode, struktur file, refactor, arsitektur — audit ini murni menilai pengalaman & tampilan yang dilihat pengguna akhir.

---

## Executive Summary

Sales Module ini punya **kepribadian visual yang lebih hangat dan lebih "butik"** dibanding rata-rata ERP kasir — perpaduan huruf display serif untuk judul dengan sans-serif untuk isi, palet warna tint lembut, dan detail kecil seperti angka finansial yang selalu rapi sejajar, memberi kesan "dirancang", bukan "digenerate dari template admin". Sapaan personal di layar Beranda Sales, tombol CTA dengan gradasi dan shadow yang diperhalus, serta dekorasi halus di sidebar menunjukkan usaha nyata untuk membangun identitas visual sendiri.

Namun ketika dinilai dengan standar SaaS ERP premium 2026 (Stripe, Linear, Notion, Shopify Admin), ada jarak yang cukup terasa di tiga area: **konsistensi lintas layar** (setiap layar Sales terasa dirancang sendiri-sendiri, bukan dari satu sistem yang sama), **kematangan pengalaman tabel** (belum ada sortir kolom atau paginasi di manapun dalam modul ini, padahal ini standar minimum di produk seperti Linear/Stripe), dan **feedback loading** (tidak ada satu pun skeleton/placeholder loading yang teramati — semua data terasa "muncul begitu saja").

Skor keseluruhan UI: **6.6 / 10** — visual sudah punya karakter dan beberapa momen benar-benar premium, tapi belum konsisten dan belum "effortless" di titik-titik kerja berat (tabel, filter, transisi antar layar).

---

## Kelebihan UI

1. **Tipografi angka finansial yang rapi.** Seluruh angka uang dan kuantitas di POS, riwayat transaksi, dan struk selalu sejajar digit-nya (tidak "loncat-loncat" saat angka berganti) — detail kecil yang jarang diperhatikan tapi sangat terasa saat kasir bekerja cepat.
2. **Tombol checkout utama benar-benar menonjol.** Di layar pembayaran dan di panel detail pesanan, tombol aksi utama memakai gradasi warna primer dengan shadow yang diperhalus dan sedikit efek "tertekan" saat diklik — CTA ini tidak akan pernah tertukar dengan tombol sekunder di sekitarnya.
3. **Preset cepat yang berpikir seperti kasir sungguhan.** Chip nominal tunai cepat (Rp500 hingga Rp50.000) dan tombol "Uang Pas" langsung relevan dengan kebiasaan transaksi tunai di Indonesia — bukan preset generik ala template asing.
4. **Status pesanan diberi warna, ikon, dan penjelasan sekaligus** — bukan sekadar teks berwarna. Kombinasi titik warna + label singkat + latar tint lembut membuat status mudah dipindai sekilas.
5. **Panel detail yang tetap terlihat saat daftar di-scroll** (di layar Pusat Pesanan) — pola "list + detail yang menempel" ini menyamai pengalaman Linear/Gmail, jauh lebih efisien dibanding pola lama "klik → pindah halaman → klik kembali".
6. **Kartu KPI yang berfungsi ganda sebagai filter** di layar Pusat Pesanan — pengguna tidak perlu berpindah ke kontrol terpisah untuk melihat "Baru" atau "Refund", cukup klik kartunya. Ini pola interaksi yang efisien dan jarang ditemukan di ERP kelas menengah.
7. **Empty state yang membantu, bukan sekadar kosong** — saat filter tidak menghasilkan apa pun, pengguna diberi ikon, judul, dan saran konkret ("coba ubah tanggal atau reset filter") alih-alih layar putih polos.
8. **Navigasi sisi kiri terasa "dirawat"** — status aktif jelas, grup menu bisa dilipat, dan ada sentuhan dekoratif halus di bagian bawah sidebar yang memberi identitas tanpa mengganggu fungsi.
9. **Alur pembayaran tunai sangat cepat dipindai mata** — label "Kekurangan"/"Kembalian" berubah warna (merah/hijau) secara instan sesuai kondisi, memberi umpan balik visual seketika tanpa harus membaca angka dengan teliti.

---

## Kekurangan UI

*(diurutkan berdasarkan dampak terhadap pengguna)*

| # | Masalah | Dampak |
|---|---|---|
| 1 | **Setiap layar dalam Sales module terasa punya "kepala" sendiri** — layar Beranda memakai sapaan personal besar dengan lebar halaman yang lebih sempit, sementara layar Pusat Pesanan memakai pola breadcrumb formal dengan halaman yang lebih lebar. Berpindah antar layar dalam satu modul yang sama terasa seperti berpindah produk. | Tinggi — merusak rasa "satu sistem yang koheren" yang menjadi ciri khas Stripe/Linear |
| 2 | **Tidak ada sortir kolom di tabel manapun** dalam modul ini (baik di riwayat transaksi maupun di tabel rekonsiliasi shift). Pengguna power (owner/admin) yang terbiasa dengan Linear/Stripe akan otomatis mencoba klik header kolom dan tidak terjadi apa-apa. | Tinggi — ekspektasi dasar pengguna SaaS modern tidak terpenuhi |
| 3 | **Tidak ada paginasi** pada daftar pesanan — daftar panjang hanya di-scroll di dalam kotak kecil. Untuk toko dengan volume transaksi tinggi, ini akan terasa seperti "menggulir tanpa akhir" dibanding navigasi halaman yang jelas ala Stripe Dashboard. | Sedang–Tinggi |
| 4 | **Bentuk badge status tidak seragam antar bagian aplikasi.** Di satu tempat, status ditampilkan sebagai pil membulat penuh dengan titik warna kecil di depannya; di sistem badge dasar aplikasi (dipakai untuk hal lain), bentuknya kotak bersudut lembut dengan teks huruf kapital berjarak. Dua bahasa visual untuk konsep yang sama ("status") membingungkan secara halus. | Sedang |
| 5 | **Ukuran dialog tidak mengikuti skala yang konsisten.** Beberapa dialog kecil (konfirmasi ringkas), beberapa medium, satu dialog rekonsiliasi shift melebar signifikan dengan scroll internal — tapi tidak terlihat aturan jelas kapan dialog memakai ukuran apa. Efeknya, transisi antar dialog dalam satu alur kerja (mis. tutup shift) terasa tidak proporsional. | Sedang |
| 6 | **Tidak ada indikator loading dalam bentuk apa pun** — bukan spinner, bukan skeleton, bukan shimmer — di seluruh modul. Saat ini terasa "instan" karena data lokal, tapi dari sisi bahasa desain, tidak ada bahasa visual yang disiapkan untuk momen tunggu. | Sedang (berisiko tinggi begitu ada jeda nyata) |
| 7 | **Kolom pencarian pada layar riwayat transaksi "tenggelam"** di antara empat kontrol filter lain dalam satu kartu — berbeda dengan pencarian di POS yang berdiri sendiri, besar, dan langsung di atas grid produk. Padahal secara fungsi, pencarian adalah alat utama di kedua layar tersebut. | Sedang |
| 8 | **Ikon kategori produk di POS ditentukan otomatis berdasarkan nama kategori** — bekerja baik untuk nama umum (daging, ikan, minuman) tapi berpotensi jatuh ke ikon generik untuk nama kategori yang tidak lazim, membuat sebagian kategori terasa "kurang personal" dibanding yang lain dalam grid yang sama. | Rendah–Sedang |
| 9 | **Beberapa pesan validasi (mis. alasan refund wajib diisi) muncul sebagai notifikasi sekilas di pojok layar, bukan tanda merah di sebelah kolom yang bermasalah.** Pengguna harus menyambungkan sendiri notifikasi tersebut ke field mana yang perlu diperbaiki. | Rendah–Sedang |

---

## UX Findings

- **Alur checkout POS sangat efisien untuk pengguna berulang (kasir).** Kombinasi shortcut keyboard (fokus cepat ke pencarian, lompat ke pembayaran, selesaikan dengan Enter, cetak dengan satu kombinasi tombol) membuat kasir berpengalaman bisa bertransaksi nyaris tanpa menyentuh mouse — pola yang setara filosofi "keyboard-first" ala Linear/Raycast, namun diterapkan pada konteks kasir fisik, bukan software produktivitas. Ini adalah salah satu aspek UX terkuat di seluruh modul.
- **Guard operasional muncul di waktu yang tepat.** Sistem memperingatkan kasir bahwa shift belum dibuka **sebelum** mereka sempat mengisi form pembayaran, bukan setelah — mencegah rasa frustrasi kehilangan input yang sudah diisi.
- **Beban kognitif di layar POS cukup padat untuk pengguna baru.** Dalam satu layar, kasir dihadapkan pada: grid produk, keranjang, pilihan sumber pesanan, tipe pelanggan, opsi pre-order, dan pengaturan kemasan — semuanya berpotensi aktif sekaligus. Dibanding pengalaman kasir minimalis ala Square/Shopify POS (yang sengaja menyembunyikan opsi lanjutan di balik satu tombol "lainnya"), layar ini terasa lebih "penuh" sejak pandangan pertama, meski secara visual sudah dikelompokkan dengan rapi.
- **Transisi status pesanan (Baru → Diproses → Siap → Selesai) divisualisasikan sebagai garis waktu vertikal yang jelas** — pengguna tidak perlu menebak di tahap mana sebuah pesanan berada; ini pola yang sangat baik dan sejajar dengan tracking order di aplikasi e-commerce modern.
- **Default filter tanggal = hari ini** pada riwayat transaksi adalah keputusan default yang tepat — mengurangi satu langkah interaksi untuk kasus penggunaan paling umum (melihat transaksi hari ini).
- **Notifikasi WhatsApp yang terpicu otomatis dari perubahan status** adalah sentuhan yang terasa modern dan kontekstual untuk pasar Indonesia — bukan fitur generik yang ditempel, tapi terasa dirancang untuk alur kerja nyata toko/resto lokal.
- **Layar Sales tidak memberi jejak arah (breadcrumb) secara konsisten.** Layar Pusat Pesanan menunjukkan jejak "Sales › Pesanan", tapi layar Beranda Sales tidak menunjukkan jejak serupa — pengguna kehilangan penanda "saya sedang di modul apa" begitu berpindah ke Beranda.

---

## Visual Consistency

| Elemen | Temuan |
|---|---|
| Lebar halaman (container) | Berbeda-beda antar layar Sales — Beranda terasa lebih sempit dan terpusat, Pusat Pesanan terasa lebih lebar dan penuh. Tidak ada satu lebar acuan yang konsisten dalam modul yang sama. |
| Pola header halaman | Beranda memakai sapaan besar bernuansa personal; Pusat Pesanan memakai breadcrumb + judul formal. Dua nada yang berbeda untuk modul yang sama. |
| Bentuk badge/status | Dua bahasa visual: pil membulat penuh dengan titik warna (di riwayat transaksi) vs badge kotak bersudut lembut huruf kapital (di sistem dasar aplikasi). |
| Ukuran dialog | Tidak mengikuti skala tetap (kecil/sedang/besar) — bervariasi dari sempit hingga sangat lebar tanpa pola yang bisa ditebak dari konteksnya. |
| Kontrol pemilihan (selection controls) | Metode pembayaran memakai ubin besar bergaya kartu; kategori produk memakai tab bulat kecil; status pesanan memakai tab bersegmen — tiga bahasa visual berbeda untuk konsep yang serupa (memilih satu dari beberapa opsi). |
| Palet warna semantik | Sistem warna dasar (sukses/peringatan/bahaya/info) sudah terdefinisi rapi dan dipakai konsisten *di dalam* satu komponen, tapi implementasi status pesanan di riwayat transaksi tidak sepenuhnya menarik dari palet yang sama persis — nuansa warnanya dekat tapi tidak identik. |

---

## Modern SaaS Assessment

**Apa yang sudah terasa premium:**
- Palet warna tint lembut (bukan warna solid mencolok) untuk status dan latar sekunder — bahasa visual yang sama dipakai Linear dan Notion untuk menghindari kesan "mencolok murahan".
- Perhatian pada detail kecil bernilai tinggi: angka rata digit, transisi halus saat kartu di-hover (sedikit terangkat dengan shadow membesar), warna yang berubah reaktif sesuai kondisi data (kembalian vs kekurangan).
- Tombol CTA utama memakai gradasi arah warna primer yang halus dengan shadow berwarna (bukan shadow abu-abu generik) — detail yang biasa ditemukan di produk fintech premium (Stripe, Ramp), bukan admin template gratis.
- Identitas visual yang berani berbeda dari "SaaS dashboard dingin" pada umumnya — pilihan huruf display bergaya serif untuk judul memberi kesan butik/kuliner premium yang cocok untuk konteks bisnis F&B, alih-alih meniru mentah-mentah estetika Linear yang serba grotesque sans-serif dingin.

**Apa yang masih membuatnya terasa seperti template/kerja setengah jalan:**
- **Belum ada "sistem" yang benar-benar mengikat seluruh layar Sales menjadi satu produk.** Stripe Dashboard terasa premium bukan karena satu layar indah, tapi karena *setiap* layar terasa berasal dari cetakan yang sama. Di sini, kesan itu pecah begitu berpindah dari satu layar Sales ke layar Sales lainnya.
- **Ekspektasi dasar pengguna SaaS modern di tabel (sortir, paginasi, kolom yang bisa disesuaikan) belum hadir** — ini adalah salah satu ciri paling membedakan "dashboard admin lama" dari "SaaS 2026", dan modul ini masih berada di sisi yang lama untuk aspek ini secara spesifik.
- **Tidak ada bahasa visual untuk kondisi "sedang memuat"** — produk SaaS premium modern selalu punya skeleton/shimmer yang terasa halus dan disengaja; ketiadaannya membuat transisi terasa kurang "dipoles" begitu nanti terhubung ke data yang butuh waktu.

---

## Quick Wins

1. Satukan bentuk badge status ke satu bahasa visual tunggal (pilih salah satu: pil bulat dengan titik, atau kotak kapital) dan terapkan ke seluruh layar Sales.
2. Tambahkan breadcrumb yang konsisten di layar Beranda Sales, menyamakan penanda lokasi dengan layar Pusat Pesanan.
3. Perbesar dan tonjolkan kolom pencarian di riwayat transaksi agar setara secara visual dengan pencarian di POS — pisahkan dari kelompok filter sekunder.
4. Tetapkan skala ukuran dialog (kecil/sedang/besar) dan terapkan konsisten ke semua dialog dalam modul.
5. Pindahkan pesan validasi kritis (mis. "alasan refund wajib diisi") menjadi teks bantuan berwarna di bawah kolom terkait, selain notifikasi sekilas yang sudah ada.

## High Impact Improvements

1. **Bangun satu pola header/kerangka halaman tunggal** untuk seluruh layar Sales (lebar konsisten, posisi judul konsisten, posisi aksi utama konsisten) — ini akan memberi dampak "terasa satu produk" paling besar dibanding perbaikan lainnya.
2. **Tambahkan sortir kolom dan paginasi/"muat lebih banyak"** pada tabel riwayat transaksi dan tabel rekonsiliasi shift — ini adalah kesenjangan paling mendasar antara modul ini dan standar tabel SaaS 2026.
3. **Rancang bahasa visual loading (skeleton) untuk seluruh titik data** — kartu KPI, daftar pesanan, panel detail — agar produk siap terasa premium begitu terhubung ke sumber data yang tidak instan.
4. **Sederhanakan layar POS untuk kasir baru** — pertimbangkan menyembunyikan opsi lanjutan (pre-order, sumber pesanan non-default, kemasan kustom) di balik satu titik masuk opsional, dan hanya menampilkan alur inti (cari → keranjang → bayar) secara default, mengikuti filosofi progressive disclosure yang dipakai Stripe Checkout.

---

## Final Score

| Dimensi | Skor (0–10) |
|---|---|
| Visual Design | 7.2 |
| UI Consistency | 6.0 |
| UX | 6.7 |
| Navigation | 7.0 |
| Forms | 7.0 |
| Tables | 5.5 |
| Search & Filter | 6.8 |
| Accessibility | 5.5 |
| SaaS Premium Feeling | 7.0 |
| **Overall UI** | **6.6** |

**Catatan skor:** Modul ini paling kuat di momen-momen mikro (POS, checkout, kartu KPI) dan paling lemah di dua area yang justru paling dilihat pengguna berpengalaman (owner/admin yang bekerja dengan tabel setiap hari): pengalaman tabel dan konsistensi lintas layar. Menutup dua kesenjangan ini akan memindahkan persepsi produk secara signifikan dari "dashboard yang cantik" menjadi "SaaS ERP yang benar-benar premium".
