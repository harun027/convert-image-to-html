# Kontrak Responsif

Semua angka di bawah adalah default yang mengikat. Geser hanya kalau gambar
memang menuntut, dan catat alasannya di `NOTES.md`.

"Rapi", "nyaman", dan "enak dilihat" tidak bisa diverifikasi — jadi diterjemahkan
jadi angka: lebar uji, titik pindah kolom, skala tipografi, dan ambang jarak.

---

## 1. Lima Lebar Uji (wajib dicek semua)

| Lebar | Perangkat | Yang harus benar |
|---|---|---|
| **320px** | HP terkecil (iPhone SE lama) | Nol horizontal scroll. Judul tidak terpotong. Tombol tidak keluar layar. |
| **375px** | HP umum | Baris teks 45–60 karakter. Tombol penuh atau cukup lega. |
| **768px** | Tablet portrait | Grid sudah 2 kolom. Nav masih hamburger. |
| **1024px** | Laptop kecil | Nav desktop muncul. Layout desktop penuh. |
| **1440px** | Desktop | Container terkunci `max-w-7xl`, sisanya jadi margin. Teks tidak melebar. |

Cek di DevTools: `Ctrl+Shift+M`, ketik lebar manual. Bukan sekadar tarik-tarik jendela.

## 2. Breakpoint yang Dipakai

Hanya tiga. Lebih banyak = ritme pecah dan sulit dijaga konsisten.

| Prefix | Lebar | Perannya |
|---|---|---|
| *(base)* | 0–767 | Satu kolom. Semua menumpuk. |
| `md:` | ≥768 | Grid mulai membelah. Padding naik. |
| `lg:` | ≥1024 | Layout desktop. Nav desktop muncul, hamburger hilang. |

`sm:` dipakai hanya untuk satu hal: tombol `w-full sm:w-auto`.
`xl:` dan `2xl:` tidak dipakai — `max-w-7xl` sudah menahan lebar.

## 3. Container & Padding Tepi

```html
<div class="mx-auto max-w-7xl px-6 lg:px-8">
```

| Lebar layar | Padding tepi | Lebar konten |
|---|---|---|
| 320px | 24px | 272px |
| 375px | 24px | 327px |
| 768px | 24px | 720px |
| 1024px | 32px | 960px |
| 1440px | 32px | 1280px (terkunci) |

Jangan `w-screen` — bikin overflow horizontal saat ada scrollbar. Pakai `w-full`.
Jangan lebar px tetap (`w-[1200px]`). Selalu `max-w-*`.

## 4. Jarak Vertikal Section

| Slot | base | `md:` | `lg:` |
|---|---|---|---|
| Padding section | `py-16` (64px) | `py-24` (96px) | `py-32` (128px) |
| Gap antar kolom | `gap-10` (40px) | `gap-12` (48px) | `gap-16` (64px) |
| Gap antar card | `gap-6` (24px) | `gap-6` | `gap-8` (32px) |
| Padding dalam card | `p-6` (24px) | `p-6` | `p-8` (32px) |

Saat kolom menumpuk di mobile, `gap` menggantikan peran jarak horizontal —
karena itu `gap-10` di base, bukan `gap-6`. Terlalu rapat = dua blok terbaca menyatu.

## 5. Peta Runtuh Grid

| Desktop | Kelas Tailwind |
|---|---|
| 2 kolom setara | `grid-cols-1 md:grid-cols-2` |
| 3 kolom (fitur, blog) | `grid-cols-1 md:grid-cols-2 lg:grid-cols-3` |
| 4 kolom (logo, stat) | `grid-cols-2 lg:grid-cols-4` |
| Split hero tak setara | `lg:grid-cols-12` + `lg:col-span-6` / `lg:col-span-5` |
| Footer 4 kolom | `grid-cols-2 lg:grid-cols-4` |

Aturannya: **kartu kecil boleh 2 kolom sejak 375px** (logo, statistik, ikon).
**Kartu berisi paragraf jangan 2 kolom sebelum 768px** — baris jadi terlalu pendek.

Urutan di mobile: teks selalu di atas visual. Kalau gambar harus duluan,
pakai `order-first lg:order-last`, bukan mengubah urutan DOM.

## 6. Skala Tipografi

| Peran | base | `md:` | `lg:` |
|---|---|---|---|
| Hero `<h1>` | `text-4xl` (36px) | `text-5xl` (48px) | `text-6xl` (60px) |
| Judul section `<h2>` | `text-3xl` (30px) | `text-4xl` (36px) | `text-4xl` |
| Judul card `<h3>` | `text-lg` (18px) | `text-lg` | `text-xl` (20px) |
| Paragraf utama | `text-base` (16px) | `text-lg` (18px) | `text-lg` |
| Paragraf card | `text-sm` (14px) | `text-sm` | `text-base` (16px) |
| Caption / eyebrow | `text-xs` (12px) | `text-xs` | `text-xs` |

Batas bawah: **tidak ada teks isi di bawah 14px**. Caption boleh 12px.

Judul besar wajib `tracking-tight leading-[1.1]`. Tanpa itu, `text-6xl` terlihat
renggang dan barisnya berjauhan.

**Panjang baris dibatasi**, kalau tidak teks melebar tidak enak dibaca di 1440px:

| Jenis | Kelas |
|---|---|
| Paragraf hero | `max-w-lg` (512px) |
| Paragraf section | `max-w-2xl` (672px) |
| Paragraf tengah | `mx-auto max-w-2xl text-center` |
| Paragraf dalam card | (dibatasi lebar card, tidak perlu) |

## 7. Target Sentuh

| Aturan | Kelas |
|---|---|
| Tinggi minimum elemen interaktif | `min-h-11` (44px) |
| Tombol ikon | `size-11` (44×44) |
| Jarak antar target sentuh | minimal `gap-2` (8px) |
| Link dalam daftar vertikal | `px-3 py-3` (area klik penuh) |
| CTA utama di mobile | `w-full sm:w-auto` |

Tombol `py-2` saja tingginya ~36px — terlalu kecil. Selalu tambahkan `min-h-11`.

## 8. Titik Rawan Pecah

| Penyebab | Perbaikan |
|---|---|
| Kata/URL panjang | `break-words` pada wadahnya |
| Tabel | Bungkus `<div class="overflow-x-auto">` |
| Blok kode | `overflow-x-auto` |
| Baris tombol | `flex-wrap` + `gap-3` |
| Baris logo/badge | `flex-wrap` + `gap-x-8 gap-y-4` |
| Gambar | `w-full h-auto` + atribut `width`/`height` (cegah layout shift) |
| Rasio media | `aspect-[16/9]` atau `aspect-[4/3]`, jangan tinggi px tetap |
| Nav desktop di 768px | Sembunyikan sampai `lg:` — 768px terlalu sempit untuk nav penuh |
| Elemen absolut/sticky | Cek di 320px; sering keluar layar |
| `100vw` | Ganti `w-full` |

## 9. Checklist Verifikasi Responsif

Jalankan di lima lebar (§1). Semua harus lulus sebelum lapor selesai.

**Di 320px & 375px**
1. `document.body.scrollWidth <= window.innerWidth` — nol scroll horizontal.
2. Tidak ada teks terpotong atau tertimpa.
3. Semua tombol dan link tinggi ≥44px.
4. Paragraf 45–60 karakter per baris.
5. Menu hamburger terbuka dan tertutup (klik `<summary>`).
6. Panel menu mobile tidak menutupi konten permanen.

**Di 768px**
7. Grid card sudah 2 kolom, bukan masih 1.
8. Nav masih hamburger, nav desktop belum muncul.
9. Padding section sudah naik ke `py-24`.

**Di 1024px**
10. Nav desktop muncul, hamburger hilang.
11. Split hero sudah bersebelahan, bukan menumpuk.

**Di 1440px**
12. Konten terkunci `max-w-7xl`, tidak menempel tepi layar.
13. Tidak ada paragraf melebihi `max-w-2xl`.
14. Gambar tidak buram karena diregangkan melebihi ukuran aslinya.

**Semua lebar**
15. Zoom tidak dimatikan (`user-scalable=no` / `maximum-scale=1` dilarang).
16. `<meta name="viewport" content="width=device-width, initial-scale=1">` ada.
17. Ritme spacing sama di semua section pada lebar yang sama.
