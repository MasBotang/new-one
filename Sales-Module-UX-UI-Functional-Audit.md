# ERP v1.0 — Sales Module
## Comprehensive UX/UI & Functional Audit

**Auditor:** Principal Product Designer / Senior ERP UX Architect / Lead QA Engineer
**Scope:** `src/features/sales/*`, `src/features/pos/*`, `src/services/sales*.ts`, `refund.service.ts`, `shift.service.ts`, `preorder.service.ts`, `hold.service.ts`, `marketplace-settlement.service.ts`, `queue.service.ts`, `packing.ts`, rute `_app.sales.*`, `_app.analytics.sales.tsx`
**Metodologi:** Static code + UI-tree audit langsung terhadap implementasi nyata (bukan asumsi). Setiap temuan disertai bukti file/baris.

> Catatan metodologi: audit ini murni membaca kode sumber (tidak menjalankan aplikasi secara visual), sehingga penilaian UI/UX didasarkan pada struktur JSX, className Tailwind, state machine, dan pola interaksi yang tertulis di kode — bukan tangkapan layar. Semua temuan "Missing" diverifikasi lewat pencarian menyeluruh (grep) di seluruh folder terkait, bukan pembacaan satu file saja.

> **Catatan revisi (klarifikasi Product Owner):** Setelah laporan awal disusun, Product Owner mengonfirmasi bahwa beberapa item berikut adalah **keputusan scope yang disengaja**, bukan kekurangan implementasi: (1) QRIS dinamis/integrasi payment gateway memang tidak ingin dibangun, (2) input referensi tambahan untuk Card/Transfer memang tidak ingin diadakan, (3) dukungan scanner barcode khusus memang tidak ingin diadakan, (4) CRM/Customer Management memang belum ingin dibangun saat ini. Item-item ini telah direklasifikasi dari "Missing/Partial" menjadi "Out of Scope (by design)" dan tidak lagi dihitung sebagai defisiensi dalam skor akhir. Field **Diskon** dikonfirmasi memang belum ada dan tetap dicatat sebagai temuan fungsional yang valid.

---

## 1. Executive Summary

Sales Module ini terdiri dari **4 halaman fitur utama** (`home-page.tsx`, `pesanan-page.tsx`, `preorder-page.tsx`, `shifts-page.tsx`) ditambah **POS Page** (`features/pos/pos-page.tsx`) sebagai jantung transaksi, didukung 8 service layer (`sales`, `shift`, `refund`, `preorder`, `hold`, `marketplace-settlement`, `queue`, `packing`).

Secara arsitektur bisnis, modul ini **jauh lebih matang dari rata-rata POS/ERP starter kit**: alur Cart → Stock Deduction → Packaging Deduction sudah menerapkan validasi pra-transaksi dan rollback atomik (lihat §3), refund memiliki approval-gate berbasis role, dan shift closing punya rekonsiliasi marketplace per-channel — fitur yang biasanya baru muncul di ERP tingkat menengah-atas.

Namun dari sisi **UI/UX dan kesiapan sebagai produk SaaS premium**, modul ini belum sejajar dengan Stripe/Linear/Shopify karena tiga masalah struktural:

1. **Tidak ada lapisan komponen bersama** untuk Sales (tidak ada `features/sales/components/`, tidak ada `SalesPageHeader`) — setiap halaman menulis ulang header, card, dan filter bar-nya sendiri, menghasilkan pola visual yang tidak seragam antar halaman dalam modul yang sama.
2. **File monolitik berukuran sangat besar** — `pos-page.tsx` (1.947 baris), `shifts-page.tsx` (1.824 baris), `pesanan-page.tsx` (1.169 baris) — seluruhnya berupa satu komponen fungsi tunggal tanpa dekomposisi, yang menjadi beban maintainability signifikan.
3. **Diskon dan Split Payment belum memiliki UI sama sekali** — untuk Diskon, field skema sudah tersedia tapi input-nya belum dibangun. (Item lain yang sempat ditemukan pada audit awal — QRIS dinamis, input referensi Card/Transfer, mode scanner barcode khusus, dan CRM — telah dikonfirmasi Product Owner sebagai **keputusan scope yang disengaja**, sehingga tidak lagi dihitung sebagai kekurangan dalam laporan ini.)

Skor keseluruhan modul ini: **6.6 / 10** — solid secara backend/business-logic, tertinggal di lapisan produk (UI system, komponetisasi, sebagian fitur checkout).

---

## 2. Functional Audit

| Fitur | Status | Bukti / Catatan |
|---|---|---|
| Product Grid + Search + Kategori | ✅ Complete | `pos-page.tsx:335-351` — filter kategori & pencarian nama/SKU/barcode/kode berjalan client-side, reaktif per keystroke |
| Cart (add/adjust/remove, validasi stok) | ✅ Complete | `pos-page.tsx:364-409` — validasi stok real-time saat `addToCart`/`adjust`, plus re-validasi final saat checkout (`pos-page.tsx:496-504`) |
| Checkout single-method payment (Cash/QRIS/Card/Transfer) | ✅ Complete | `pos-page.tsx:1742-1763` |
| **Diskon (Discount)** | ❌ **Missing** | Field `discount: number` ada di `types.ts:190` dan ditampilkan di riwayat/struk (`pesanan-page.tsx:983-988`, `invoice-receipt.ts:162`), tapi **tidak ada satupun input UI** untuk mengisinya. Pencarian menyeluruh `discount:` di seluruh `src/` hanya menemukan hard-code `discount: 0` (di `pos-page.tsx:561` dan `preorder-page.tsx:208`). Kasir tidak bisa memberi diskon apa pun. |
| **Split Payment** | ❌ **Missing** | `type PaymentMethod = "cash" \| "qris" \| "card" \| "transfer"` (`types.ts:32`) — satu sale hanya menyimpan satu metode. Tidak ada struktur data untuk multi-tender (mis. sebagian cash + sebagian QRIS). |
| QRIS | ✅ Complete (by design) | Berupa pilihan metode pembayaran (icon `QrCode` + toggle) untuk mencatat bahwa transaksi dibayar via QRIS statis di luar sistem (mis. stiker QRIS/EDC terpisah). **Konfirmasi produk:** QR code dinamis/integrasi payment gateway memang sengaja tidak dibangun di scope ini — bukan gap. |
| Cash (uang tunai, kembalian, quick-cash) | ✅ Complete | `pos-page.tsx:1766-1835` — quick denomination Rp500–Rp50.000, "Uang Pas", validasi kekurangan real-time, `Enter` untuk checkout cepat |
| Card / Transfer | ✅ Complete (by design) | Berupa pilihan metode pembayaran sederhana tanpa input referensi tambahan. **Konfirmasi produk:** input nomor referensi/bukti transfer memang sengaja tidak diikutkan di scope ini — bukan gap. |
| Hold Order (tunda transaksi) | ✅ Complete | `hold.service.ts` + state `holds`/`activeHoldId` di `pos-page.tsx:196-197` |
| Pre Order | ✅ Complete | `preorder.service.ts` (279 baris) — validasi jadwal pickup (`validatePickup`), kalender bulanan di `preorder-page.tsx`, terhubung dua arah ke Sale (`preorderId` ↔ `saleId`) |
| Marketplace order handling | ✅ Complete | `SourceOrder`/`MARKETPLACE_SOURCES`, price group otomatis per sumber (`priceGroupService.forSource`) |
| Shift (buka/tutup, cash reconciliation) | ✅ Complete | `shift.service.ts` (534 baris), guard "shift wajib aktif" sebelum transaksi (`pos-page.tsx:411-417`) |
| Rekonsiliasi Marketplace saat tutup shift | ✅ Complete | `shifts-page.tsx:126-331` — input settlement per marketplace, kalkulasi fee otomatis |
| Refund | ✅ Complete (dengan approval gate) | `refund.service.ts` + alur request→review→approve/reject berbasis role (`pesanan-page.tsx:374-435`); approval memicu pengembalian stok, antrean, dan packaging |
| History / Riwayat Transaksi | ✅ Complete | Terintegrasi sebagai satu halaman "Pesanan" dengan filter tanggal/outlet/marketplace/kasir + pencarian teks bebas |
| Search Product di POS | ✅ Complete | Multi-field (nama, SKU, barcode, kode) — `pos-page.tsx:340-351` |
| Barcode Scanner (hardware) | — Out of Scope (by design) | Pencarian teks manual di field search tersedia sebagai alur utama (berfungsi juga dengan scanner USB yang bertindak sebagai keyboard-wedge). **Konfirmasi produk:** mode scan/listener khusus dengan feedback suara memang sengaja tidak diadakan — bukan gap. |
| Customer Management (CRM) | — Out of Scope (by design, saat ini) | Tidak ditemukan `customer.service.ts` atau data store pelanggan; nama & telepon pelanggan hanya teks bebas per transaksi. **Konfirmasi produk:** CRM memang belum ingin dibangun di versi ini — dicatat sebagai keputusan scope, bukan defisiensi fungsional. |
| Notifikasi WhatsApp otomatis | ✅ Complete | `whatsapp.service.ts` — template per status antrean, void, dan refund (`pesanan-page.tsx:326-339, 408-417`) |
| Keyboard Shortcuts Kasir | ✅ Complete | F2 = fokus pencarian, F4 = buka pembayaran, Esc = tutup dialog, Ctrl/Cmd+P = cetak, Enter = checkout/selesaikan (`pos-page.tsx:435-464`) |
| Cetak Struk | ✅ Complete | `invoice-receipt.ts`, dipanggil dari POS dan dari halaman Pesanan (reprint) |

---

## 3. Business Flow Audit

Alur yang diverifikasi di kode: **Product → Cart → Payment → Stock Deduction → Packaging Deduction → Queue → (Preorder link) → Analytics/Finance**.

**Temuan positif (flow utuh & aman):**
- `sales.service.ts:82-114`: sebelum stok dikurangi, seluruh baris cart divalidasi dulu terhadap stok terbaru (`fresh stock check`) — bila ada kekurangan, **seluruh transaksi dibatalkan dan entry sale yang sudah ditulis di-rollback** (bukan partial deduction). Ini adalah pola transaksi atomik yang jarang ditemukan di starter-kit POS.
- `sales.service.ts:116-160`: kebutuhan packaging (box, plastik, extra packaging) dihitung dan divalidasi terhadap stok inventory packaging **sebelum** benar-benar dikurangi; jika kurang, stok produk yang sudah didebit di langkah sebelumnya **di-rollback kembali** (`productService.addStock` dengan reason "Rollback stok — packaging kurang"). Ini mencegah kondisi stok "hangus" akibat transaksi gagal separuh jalan.
- Komentar developer di kode (`// FIX (Sprint 1.5): pra-validasi + shortage rollback WAJIB propagate ke caller...`) menunjukkan bug kelas ini **sudah pernah terjadi dan sudah diperbaiki secara terdokumentasi** — indikasi proses QA internal yang berjalan, bukan kode yang tidak pernah diuji.
- Refund approval (`pesanan-page.tsx:391-406`) memicu pengembalian stok, antrean, dan packaging secara terpusat lewat `refundService.approve` — flow tidak putus antara Sales ↔ Inventory ↔ Queue.
- Preorder dari kasir tercatat sebagai `PreOrder` **terpisah** dari `Sale` tapi saling terhubung via `preorderId`/`saleId` (`pos-page.tsx:517-538, 579-586`) — desain yang benar karena struk & pembayaran tetap tercatat di kasir sementara jadwal pickup dikelola di modul Pre Order tanpa masuk antrean produksi dini.

**Flow yang perlu perhatian:**
- Tidak ditemukan pemicu eksplisit dari `sales.service.ts` ke modul Finance (mis. pencatatan otomatis ke ledger/cash flow). Integrasi Finance tampak terjadi lewat agregasi data terpisah (`finance-aggregation.ts`), bukan event langsung saat sale dibuat. Ini bukan cacat, tapi berarti **konsistensi Sales↔Finance bergantung pada proses agregasi berkala**, bukan write-through — perlu dipastikan agregasi ini selalu dijalankan sebelum laporan finance dibaca.
- Diskon yang tidak bisa diisi di UI (lihat §2) berarti kolom `discount` di alur Payment→Settlement secara praktik **selalu bernilai 0** — flow untuk skenario diskon (mis. promo, member, retur sebagian) tidak dapat diuji end-to-end karena input-nya tidak ada.

---

## 4. UI Audit

**Positif:**
- Sistem token visual konsisten dalam satu halaman: warna berbasis `oklch()`, radius `rounded-xl`/`rounded-2xl`, `tabular-nums` dipakai konsisten untuk seluruh angka uang/qty (mencegah "jitter" digit — detail yang sering diabaikan di admin template biasa).
- Status badge menggunakan pola tint lembut (`bg-sky-50 text-sky-700 border-sky-200`) alih-alih warna solid mencolok khas Bootstrap admin — ini **sudah sejalan** dengan bahasa visual Linear/Notion.
- Dialog "Transaksi Berhasil" (`pos-page.tsx:1853-1945`) memiliki hierarki visual yang baik: hero status → jumlah utama besar → ringkasan pembayaran → daftar item — pola ini setara struk digital pada Square/Shopify POS.

**Masalah (diurutkan berdasarkan prioritas):**

| # | Masalah | Prioritas | Bukti |
|---|---|---|---|
| 1 | Tidak ada komponen header Sales yang dibagikan antar halaman → lebar container (`max-w-[1200px]` di Home vs `max-w-[1440px]` di Pesanan vs `max-w-7xl` di Inventory) dan gaya header (hero sapaan vs breadcrumb formal) berbeda-beda dalam satu modul yang sama | Tinggi | `home-page.tsx:96`, `pesanan-page.tsx:492` |
| 2 | Segmented control status di list header Pesanan hanya menampilkan 3 dari 5 status (`Semua`, `Baru`, `Diproses`) — "Selesai" dan "Refund" hanya bisa dipilih lewat KPI card di atasnya, membuat dua kontrol filter yang tidak simetris untuk data yang sama | Sedang | `pesanan-page.tsx:671-683` vs `453-489` |
| 3 | Tidak ada pagination/virtualization pada daftar Pesanan — seluruh hasil filter dirender langsung ke DOM dalam container `overflow-y-auto` | Sedang | `pesanan-page.tsx:699-774` |
| 4 | Modul Finance memiliki komponen `FinancePageHeader` terpusat (`_shared.tsx`), sementara Sales tidak punya padanannya — inkonsistensi lintas modul yang akan makin terlihat begitu ada perubahan desain global | Sedang | `finance/_shared.tsx:18` vs tidak ada berkas serupa di `features/sales/` |
| 5 | Panel Packing (`Sheet`) adalah satu-satunya penggunaan komponen `Sheet` di POS — pola drawer tidak dipakai untuk hal lain yang berpotensi sama pentingnya (mis. riwayat cart tersimpan/hold order) | Rendah | `pos-page.tsx:1425` |

---

## 5. UX Audit

**Positif:**
- Alur checkout kasir sangat cepat: `Enter` di field pencarian → tidak trigger apa pun (aman), tapi `Enter` di luar input men-trigger `openPayment()`/`completeSale()`, `F4` untuk lompat langsung ke pembayaran, `Ctrl/Cmd+P` cetak — kombinasi ini **hemat klik** dibanding rata-rata POS berbasis mouse.
- Guard "shift wajib aktif" muncul **sebelum** kasir sempat membuka dialog pembayaran (`openPayment()` di `pos-page.tsx:411-417`), bukan setelah — mencegah kasir kehilangan waktu mengisi form pembayaran lalu ditolak di akhir.
- Idempotency guard (`isCompletingRef`) mencegah double-submit dari double-click/double-Enter saat checkout — masalah UX klasik di POS yang di sini sudah ditangani secara eksplisit.
- Empty state pada daftar Pesanan (`pesanan-page.tsx:700-711`) memiliki ikon, judul, dan saran tindakan ("Coba ubah tanggal atau reset filter") — bukan sekadar teks kosong.

**Masalah (diurutkan berdasarkan prioritas):**

| # | Masalah | Prioritas | Dampak |
|---|---|---|---|
| 1 | Tidak bisa memberi diskon sama sekali di kasir (lihat §2) — untuk kasus nyata seperti bundling promo, kompensasi komplain, atau harga member khusus, kasir terpaksa mengubah harga produk secara manual atau mencatat di luar sistem | Tinggi | Blocker operasional harian |
| 2 | Tidak ada split payment — transaksi campuran (mis. sebagian cash sebagian QRIS) tidak bisa dicatat akurat, memaksa kasir memilih satu metode yang tidak mencerminkan kondisi sebenarnya | Tinggi | Data rekonsiliasi kas tidak akurat |
| 3 | Pada breakpoint di bawah `lg`, grid POS berubah jadi `grid-cols-1` (`pos-page.tsx:754`) — cart dan tombol checkout berpindah ke bawah daftar produk tanpa ringkasan total/tombol bayar yang tetap terlihat (sticky bar) — kasir harus scroll untuk membayar di layar sempit/tablet portrait | Sedang | Kecepatan kasir di perangkat non-desktop menurun |
| 4 | Tidak ada pencarian/autocomplete pelanggan berulang — nama pelanggan diketik ulang setiap transaksi (dicatat sebagai konsekuensi dari keputusan scope "CRM belum dibangun", bukan defisiensi independen) | Rendah | Data pelanggan terfragmentasi; dampaknya rendah selama memang belum menjadi prioritas produk |

---

## 6. SaaS Modern Audit (2026 Benchmark)

| Dimensi | Perbandingan | Penilaian |
|---|---|---|
| Density & whitespace | Kartu KPI, badge tint lembut, `tabular-nums` — selaras dengan Linear/Stripe | Sudah modern |
| Command-driven interaction | Keyboard shortcut kasir (F2/F4/Enter/Ctrl+P) mendekati filosofi Linear ("keyboard-first") | Sudah modern |
| Component reuse / design system maturity | **Belum** — Stripe/Linear/Vercel dashboard dibangun di atas design-system komponen kecil yang dipakai ulang; Sales module di sini menulis ulang layout di setiap halaman sebagai monolith 600–1900 baris | Belum sejajar |
| Progressive disclosure pada form kompleks (checkout) | Dialog pembayaran cukup ringkas, tapi form utama POS (customer, preorder, packing, sourceOrder, priceGroup) semuanya tampak sekaligus di satu panel tanpa disclosure bertahap seperti pola step Stripe Checkout | Sebagian |
| Empty/loading state | Empty state baik; loading skeleton **tidak ada di manapun** dalam Sales module (state lokal dari `localStorage`, bukan network) — wajar untuk arsitektur saat ini, tapi menjadi utang teknis begitu backend nyata (network-bound) ditambahkan | Perlu diantisipasi |

**Kesimpulan:** Sales Module terasa modern di **level micro-interaction** (warna, tipografi, shortcut), tapi belum modern di **level arsitektur produk** (reusable component system) yang menjadi pembeda utama Stripe/Linear/Vercel dari admin template lama.

---

## 7. ERP Audit

- **Apakah terlalu banyak dashboard/chart?** Tidak. Home page Sales hanya menampilkan ringkasan operasional harian (revenue, order count, avg ticket, target) — proporsional, tidak berlebihan seperti dashboard BI yang dipaksakan ke alur operasional harian.
- **Apakah data operasional mudah dibaca?** Ya — KPI card di halaman Pesanan sekaligus berfungsi sebagai filter status (klik KPI = filter), pola yang efisien secara ruang dan kognisi.
- **Apakah kasir bisa bekerja cepat?** Ya, berkat keyboard shortcut dan quick-cash denomination Indonesia (Rp500–Rp50.000) yang disesuaikan konteks lokal (`pos-page.tsx:127`).
- **Apakah owner mudah melihat transaksi?** Ya — filter multi-dimensi (tanggal, outlet, marketplace, kasir) tersedia di satu tempat pada halaman Pesanan.
- **Kesenjangan ERP:** Tidak adanya modul diskon/split payment adalah kesenjangan nyata dibanding standar minimum ERP ritel modern (Odoo POS, Loyverse, Moka), yang semuanya menyediakan kedua fitur ini sebagai fitur inti, bukan tambahan.

---

## 8. Mobile Audit

- Layout utama POS memakai `grid-cols-1 lg:grid-cols-[minmax(0,1fr)_420px]` (`pos-page.tsx:754`) — di bawah breakpoint `lg` (1024px), cart **tidak** menjadi drawer/bottom-sheet terpisah, melainkan stack vertikal biasa di bawah grid produk. Tidak ditemukan sticky summary bar atau FAB checkout untuk layar sempit.
- Dialog pembayaran dan dialog "Transaksi Berhasil" menggunakan `sm:max-w-md`, ukurannya wajar untuk mobile.
- Filter bar di halaman Pesanan menggunakan grid 12 kolom yang collapse ke 1 kolom di mobile (`md:grid-cols-12`) — ini sudah benar secara struktur, tapi dengan 5 filter + tombol reset yang semuanya full-width vertikal, halaman Pesanan di mobile akan sangat panjang sebelum sampai ke daftar order.
- **Rekomendasi FAB:** Ya diperlukan — untuk POS di layar sempit, tombol "Bayar (Rp xxx)" idealnya sticky di bagian bawah layar begitu cart terisi, bukan mengikuti alur scroll normal.

---

## 9. Accessibility Audit

| Aspek | Temuan |
|---|---|
| `aria-label` | `pos-page.tsx` memiliki 9 penggunaan; `home-page.tsx`, `pesanan-page.tsx`, `preorder-page.tsx`, `shifts-page.tsx` **masing-masing 0**. Tombol ikon-saja (mis. tombol filter reset, tombol aksi di daftar order) berisiko tidak terbaca screen reader tanpa label eksplisit. |
| Kontras warna | Badge status memakai tint lembut (`bg-sky-50 text-sky-700`) — secara umum rasio kontras teks-terhadap-background pada palet tint pastel perlu diverifikasi manual dengan alat kontras (tidak bisa dipastikan dari kode saja), tapi pola ini secara desain cenderung lebih rendah kontras dibanding warna solid. |
| Focus state | Komponen dasar (`button.tsx`, `input.tsx` dari shadcn/ui) sudah membawa `focus-visible` ring bawaan Radix/shadcn — baseline aman, tapi tombol kustom ikon di POS (mis. metode pembayaran, quick-cash) menggunakan `<button>` polos dengan className manual; perlu verifikasi visual bahwa ring fokus konsisten di semua tombol kustom ini. |
| Keyboard navigation | Sangat baik untuk kasir (§5), tapi shortcut global (`window.addEventListener("keydown", ...)`) tidak memeriksa apakah dialog lain (mis. Packing Sheet) sedang menerima fokus form-nya sendiri secara terisolasi — berpotensi konflik shortcut saat banyak dialog bertumpuk. |
| Touch target | Tombol metode pembayaran (`px-2 py-3`) dan quick-cash (`size="sm"`) berukuran cukup untuk tablet POS berukuran umum, tapi mendekati batas bawah rekomendasi 44×44px untuk touch target di sejumlah kombinasi padding. |

---

## 10. Consistency Audit (vs Modul Lain)

| Elemen | Sales | Finance | Inventory | Konsisten? |
|---|---|---|---|---|
| Shared page header component | Tidak ada | `FinancePageHeader` (`_shared.tsx`) | Tidak diverifikasi eksplisit, tapi container width `max-w-7xl` seragam antar halaman | ❌ Tidak |
| Container width | `1200px` (Home) / `1440px` (Pesanan) | Mengikuti `_shared.tsx` (terpusat) | `max-w-7xl` (~1280px), konsisten antar halaman inventory | ❌ Tidak |
| Empty state pattern | Ikon + judul + saran (baik, tapi hanya diimplementasikan lokal di `pesanan-page.tsx`) | Tidak diaudit di sesi ini | Tidak diaudit di sesi ini | ⚠ Perlu verifikasi lanjutan |
| Sistem badge status (tint pastel) | Konsisten secara internal di Sales | — | — | ✅ Internal Sales konsisten |

**Kesimpulan konsistensi:** Precedent sudah ada di modul lain (Finance) bahwa investasi ke komponen header terpusat per-modul adalah pola yang dipakai tim ini — tapi Sales belum mengikuti pola tersebut, sehingga Sales menjadi modul yang paling "custom-built" dibanding modul lain yang sudah diperiksa.

---

## 11. Technical Debt

| Temuan | Lokasi | Catatan |
|---|---|---|
| File monolitik >1000 baris tanpa dekomposisi komponen | `pos-page.tsx` (1.947), `shifts-page.tsx` (1.824), `pesanan-page.tsx` (1.169) | Tidak ada folder `features/sales/components/`; seluruh dialog/sheet/table didefinisikan inline dalam satu return JSX raksasa |
| Tidak ada shared component folder untuk Sales | — | Berbeda dari Finance yang punya `_shared.tsx` & `_shared-v2.tsx` — indikasi **dua generasi pola arsitektur berbeda** hidup berdampingan di codebase (Finance sudah py `_shared-v2`, Sales belum py `_shared` sama sekali) |
| Field skema tak terpakai di UI (`discount`) | `types.ts:190` | Skema data sudah menyiapkan kapasitas untuk fitur yang belum dibangun UI-nya — bukan dead code, tapi "half-built feature" yang berisiko membingungkan developer baru yang membaca tipe data dan mengira fitur sudah ada |
| Duplikasi util kecil (`firstNameOf`/`firstName`, parsing email→nama) | `pos-page.tsx:163-170` dan `home-page.tsx:47-54` (logika identik, nama fungsi beda) | Kandidat ekstraksi ke `src/lib/format.ts` atau util bersama |
| Route redirect kompatibilitas untuk migrasi UX Pre Order | `routes/_app.sales.preorder.tsx` | **Bukan technical debt** — ini adalah praktik migrasi yang baik (redirect dipertahankan untuk deep link lama, dijelaskan lewat komentar). Dicatat di sini sebagai bukti proses engineering yang tertib, disebut secara eksplisit sesuai instruksi audit. |
| Tidak ada test file untuk Sales/POS logic yang ditemukan dalam scope audit | — | Business logic kritikal (rollback stok, refund approval) idealnya memiliki unit test mengingat kompleksitasnya |

---

## 12. Quick Wins

1. Tambahkan input Diskon (persentase/nominal) di dialog pembayaran POS — field backend sudah siap, tinggal UI + kalkulasi total.
2. Lengkapi segmented Tabs di header daftar Pesanan agar mencakup seluruh 5 status (tambahkan "Selesai" & "Refund"), menyamakan dengan KPI card di atasnya.
3. Tambahkan `aria-label` pada tombol-ikon di `home-page.tsx`, `pesanan-page.tsx`, `preorder-page.tsx`, `shifts-page.tsx` — pekerjaan mekanis, dampak accessibility langsung.
4. Tambahkan sticky bottom bar berisi total + tombol "Bayar" pada breakpoint mobile/tablet portrait di POS.
5. Satukan `firstNameOf`/`firstName` menjadi satu util di `src/lib/format.ts`.

## 13. Major Refactor

1. Ekstrak `pos-page.tsx`, `shifts-page.tsx`, `pesanan-page.tsx` menjadi komponen-komponen lebih kecil (mis. `ProductGrid`, `CartPanel`, `PaymentDialog`, `ShiftCloseWizard`, `OrderListPanel`, `OrderDetailPanel`) ke dalam `features/sales/components/`.
2. Bangun `SalesPageHeader` bersama (mengikuti pola `FinancePageHeader`) dan standarkan container width di seluruh halaman Sales.
3. Implementasikan **Split Payment** sebagai struktur data multi-tender (`payments: {method, amount}[]`) alih-alih single `PaymentMethod` — ini perubahan skema, bukan sekadar UI.
4. Bangun modul **Diskon** end-to-end: input di POS, validasi (persentase/nominal, batas maksimum, siapa yang berwenang memberi diskon), dan tampilkan di seluruh titik yang sudah menunggu data ini (struk, riwayat, analytics).

## 14. Future Improvement (Roadmap)

- Virtualisasi/pagination pada daftar Pesanan untuk menjaga performa saat volume transaksi bertumbuh.
- Unit test untuk `sales.service.ts` dan `refund.service.ts`, mengingat kompleksitas rollback dan approval gate di dalamnya.

> Catatan: Customer Management (CRM), QRIS dinamis, dan mode scanner barcode khusus **tidak dicantumkan** di roadmap ini sesuai konfirmasi Product Owner bahwa ketiganya memang bukan arah produk saat ini.

---

## 15. Scoring

| Dimensi | Skor (0–10) | Justifikasi Singkat |
|---|---|---|
| Visual Design | 7.0 | Token warna & tipografi modern, tapi konsistensi lintas halaman dalam modul sendiri belum solid |
| UX | 6.8 | Keyboard-first checkout sangat baik; setelah QRIS/scanner/CRM direklasifikasi sebagai keputusan scope, celah UX inti yang tersisa tinggal **Diskon & Split Payment** |
| ERP Readiness | 6.8 | Flow inti (stok/packaging/refund) matang; CRM dikeluarkan dari penilaian gap karena memang bukan target scope saat ini — Diskon tetap satu-satunya celah fungsional inti |
| Enterprise Readiness | 6.0 | Rollback atomik & approval gate ada, tapi belum ada test coverage yang teridentifikasi dan komponen belum reusable |
| Modern SaaS Quality | 6.5 | Modern di micro-interaction, belum modern di arsitektur design-system |
| Consistency | 6.0 | Sales adalah modul paling "custom" dibanding Finance yang sudah punya shared header |
| Maintainability | 5.5 | File monolitik 1000–1900 baris adalah risiko nyata untuk kecepatan development ke depan |
| **Overall** | **6.7** | Fondasi business-logic kuat; dengan QRIS dinamis, Card/Transfer reference, barcode scanner, dan CRM dikonfirmasi sebagai keputusan scope (bukan gap), investasi berikutnya yang paling berdampak murni ada di komponetisasi UI dan fitur **Diskon** (Split Payment tetap dicatat sebagai gap fungsional, menunggu keputusan produk lebih lanjut) |

---

*Audit ini disusun berdasarkan pembacaan langsung terhadap kode sumber yang diunggah (`product-hub-workspace-main.zip`). Tidak ada perubahan kode, refactor, atau file baru yang dibuat di dalam project sesuai batasan tugas.*
