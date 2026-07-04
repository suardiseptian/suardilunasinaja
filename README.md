# LunasIn — Aplikasi Simulasi Bayar Tagihan & Isi Pulsa

Aplikasi web frontend (client-side only) yang mensimulasikan proses pembayaran
tagihan rutin (listrik, PDAM, internet, seminar/event), cicilan biaya kuliah
(SPP), serta isi pulsa & paket data. Dibangun murni dengan **HTML5, CSS3, dan
Vanilla JavaScript (ES6+)** — tanpa backend maupun database sungguhan. Seluruh
data transaksi & saldo disimpan di `localStorage` milik browser.

> ⚠️ **Catatan penting**: Ini adalah aplikasi simulasi untuk keperluan tugas
> kuliah. Tidak ada uang sungguhan yang diproses, dan tidak terhubung ke
> sistem PLN/PDAM/kampus/provider yang asli.

## 1. Cara Menjalankan

Tidak perlu instalasi atau build tool apa pun.

1. Ekstrak folder proyek ini.
2. Buka file `index.html` langsung di browser (Chrome/Edge/Firefox terbaru).
3. Selesai — aplikasi berjalan sepenuhnya di sisi klien.

*(Opsional)* Jika ingin menjalankan lewat local server (disarankan agar semua
fitur browser bekerja normal):

```bash
# Python 3
python -m http.server 8080
# lalu buka http://localhost:8080
```

## 2. Struktur Folder

```
bayarkita/
├── index.html          # Struktur & semua section/view (SPA)
├── css/
│   └── style.css        # Design system, komponen, responsive, print
├── js/
│   ├── data.js           # Data simulasi (PLN, PDAM, internet, SPP, provider pulsa, dst.)
│   └── app.js             # Seluruh logika aplikasi (state, validasi, render, localStorage)
└── README.md
```

## 3. Konsep Desain

- **Nama & identitas**: "LunasIn" — dari kata "lunas", karena setiap
  pembayaran yang selesai ditandai dengan **stempel "LUNAS"** bergaya cap
  kasir di struk digital (elemen ciri khas aplikasi ini).
- **Palet warna**: hijau teal (`#0F6F5C`) sebagai warna kepercayaan finansial,
  dipadukan tinta gelap (`#0B2B26`) dan aksen kuning keemasan (`#E8A33D`).
  Mendukung mode gelap penuh.
- **Tipografi**: *Space Grotesk* untuk judul, *Inter* untuk teks umum, dan
  *JetBrains Mono* untuk seluruh angka/kode (nomor pelanggan, nominal, nomor
  VA, kode tagihan) agar mudah dipindai mata seperti pada struk asli.
- **Layout**: sidebar navigasi di desktop, berubah menjadi top bar + bottom
  tab bar ala aplikasi mobile banking pada layar kecil — dirancang agar tetap
  nyaman digunakan di kedua ukuran layar.

## 4. Daftar Fitur yang Diimplementasikan

### Wajib
- [x] 7 section/view (SPA, tanpa reload): Beranda, Bayar Tagihan, Biaya
      Kuliah, Isi Pulsa, Riwayat, Profil, Bantuan/FAQ.
- [x] Dashboard: saldo simulasi (top up manual), akses cepat kategori, promo
      banner, transaksi terakhir.
- [x] Bayar Tagihan — 4 kategori (PLN, PDAM, Internet, Seminar/Event):
  - Validasi input (panjang karakter, numerik/alfanumerik sesuai kategori).
  - Hasil cek tagihan: nama pelanggan, info, tagihan pokok, denda, jatuh
    tempo, total.
  - 3 metode pembayaran: **Virtual Account** (generate nomor VA + pilihan 4
    bank), **QRIS** (QR code asli via `qrcode.js` + countdown 5 menit), dan
    **Bayar di Teller** (kode pembayaran + daftar lokasi).
  - Loading state saat cek tagihan & saat memproses pembayaran.
  - Modal konfirmasi & struk digital dengan stempel "LUNAS", bisa dicetak
    (`window.print()` + CSS `@media print`).
- [x] Biaya Kuliah/SPP:
  - Input NIM → daftar cicilan semester (6–8 item per mahasiswa, 3 NIM
    contoh) dengan checkbox multi-pilih & total otomatis.
  - Tombol "Pilih Semua Belum Lunas".
  - Pencarian tagihan berdasarkan Kode Tagihan lintas NIM.
- [x] Isi Pulsa & Paket Data:
  - Grid 6 provider dengan **deteksi otomatis** dari 4 digit awal nomor HP.
  - Pilihan nominal preset + nominal custom, atau paket data per provider.
  - Alur pembayaran sama seperti tagihan umum.
- [x] Riwayat Transaksi: tabel dari `localStorage`, filter kategori & pencarian
  teks, tombol hapus semua riwayat (dengan konfirmasi).
- [x] Notifikasi toast (sukses/error/info), validasi form real-time & saat
      submit, modal custom, dark/light mode (tersimpan di `localStorage`).
- [x] Accessibility: semantic HTML, atribut ARIA (`role="tablist"`,
      `aria-selected`, `aria-live` pada toast), skip link, fokus keyboard
      terlihat jelas, kontras warna memadai pada kedua tema.

### Nilai Tambah
- [x] Halaman Profil (nama & email tersimpan lokal).
- [x] Halaman Bantuan/FAQ (accordion).
- [x] Dark/Light mode toggle.
- [x] Auto-deteksi provider dari nomor HP.
- [x] Animasi stempel "LUNAS" pada struk sebagai elemen ciri khas brand.

### Edge Case yang Ditangani
- NIM/nomor pelanggan tidak terdaftar → pesan error jelas, tidak crash.
- Membayar tagihan yang sudah berstatus "Lunas" → checkbox otomatis
  dinonaktifkan pada baris tersebut.
- Nomor HP tidak valid (bukan 08xx / panjang salah) → validasi real-time.
- Tombol "Bayar Sekarang" hanya aktif bila metode pembayaran sudah dipilih.
- Hapus riwayat transaksi → meminta konfirmasi sebelum menghapus.
- QRIS kedaluwarsa setelah 5 menit → status berubah otomatis.

## 5. Data Simulasi (untuk mencoba aplikasi)

**PLN** — coba salah satu nomor: `123456789012`, `129876543210`,
`115533779911`, `110022334455`, `119988776655`

**PDAM**: `PD001234`, `PD005678`, `PD009911`

**Internet**: `INT77001122`, `INT77003344`, `INT77005566`

**Seminar/Event**: `SEM2026WEBDEV`, `SEM2026UIUX01`, `SEM2026AICON`

**NIM (Biaya Kuliah)**: `202310001`, `202310002`, `202310003`

**Kode Tagihan contoh**: `986248962486438` (NIM 202310001)

**Nomor HP** (untuk uji deteksi provider): awali dengan `0812` (Telkomsel),
`0817` (XL), `0815` (Indosat), `0895` (Tri), `0881` (Smartfren), `0831`
(Axis).

## 6. Teknologi yang Digunakan

- HTML5 semantic + ARIA
- CSS3 (Custom Properties, Flexbox, Grid, `@media print`)
- Vanilla JavaScript ES6+ (tanpa framework)
- [Font Awesome 6](https://fontawesome.com/) via CDN — ikon
- [qrcode.js](https://github.com/davidshimjs/qrcodejs) via CDN — QR code QRIS
- [jsPDF](https://github.com/parallax/jsPDF) via CDN — tersedia untuk ekspor
  PDF (opsional/pengembangan lanjutan)
- Google Fonts: Space Grotesk, Inter, JetBrains Mono

## 7. Pengembangan Lanjutan (ide jika ingin dikembangkan)

- Statistik pengeluaran bulanan dengan Chart.js pada halaman Riwayat.
- Export struk ke PDF menggunakan jsPDF (library sudah dimuat di `index.html`).
- Fake login multi-user dengan beberapa akun contoh.

---
Dibuat sebagai proyek tugas kuliah — Aplikasi Web Pembayaran Tagihan
Multi-Layanan & Pengisian Pulsa.
