---
name: image-to-html-nojs
description: Use when turning a design image, screenshot, Figma export, or mockup into a static website that must visually match the reference, built with Tailwind CSS v4 and zero behavioral JavaScript, and delivered as a full index.html plus per-section HTML and per-section copy-paste prompts. Also use when the user says "buatkan promtingnya" / "buatkan prompting-nya" — that phrase means: generate the copy-paste prompt files in prompts/.
---

# Image → Static HTML (Tailwind v4, No JS)

## Overview

Satu gambar masuk, satu website statis keluar — mirip 100%, tanpa JavaScript perilaku,
dipecah jadi section supaya bisa di-generate ulang per bagian.

**Prinsip inti:** ukur dulu dari gambar, baru tulis kode. Kode yang ditulis sebelum
token diukur akan selalu meleset dan harus dibongkar.

## When to Use

- User mengirim gambar/screenshot/mockup dan minta dijadikan web.
- User minta "sama persis dengan gambar", "pixel perfect", "image to code".
- User minta HTML tanpa JavaScript / murni HTML+CSS.
- User minta output dipecah per section + prompt siap copy-paste.
- User mengetik **"buatkan promtingnya"** → lompat ke Step 3 poin 3 (mode prompt saja).

**Jangan pakai untuk:** aplikasi butuh state runtime nyata (form submit ke server,
data fetching, routing SPA). Itu butuh JS — bilang ke user, jangan dipaksakan.

---

## Workflow

### Step 0 — Terima gambar

Kalau user belum kirim gambar, minta dulu. Jangan mengarang layout.
Kalau user kirim beberapa gambar (desktop + mobile), catat mana yang jadi acuan utama.

### Step 1 — EXTRACT: bedah gambar jadi token

Baca gambar dan tulis hasilnya ke `output/<project>/NOTES.md` **sebelum** menulis HTML.
Wajib mengisi 7 hal ini:

| # | Yang diukur | Cara baca dari gambar |
|---|---|---|
| 1 | **Palette** | Sampel warna: background, surface/card, teks utama, teks sekunder, border, aksen/primary, warna tombol. Konversi ke `oklch()` atau hex. |
| 2 | **Typography** | Tebak font family (grotesk/serif/mono), lalu skala: hero, h2, h3, body, caption. Catat weight & letter-spacing (heading besar biasanya `tracking-tight`). |
| 3 | **Grid & container** | Lebar konten vs viewport → `max-w-6xl`/`max-w-7xl`. Jumlah kolom tiap section. Gutter kiri-kanan. |
| 4 | **Spacing rhythm** | Jarak vertikal antar section, jarak heading→body, gap antar card. Bulatkan ke skala 4px. |
| 5 | **Radius & border** | Radius card/tombol/input. Tebal & warna border. |
| 6 | **Shadow & depth** | Ada shadow? Halus atau tegas? Atau flat + border saja? |
| 7 | **Perilaku responsif** | Untuk tiap section, tentukan: berapa kolom di desktop, jadi berapa di 768px, jadi berapa di 375px. Tulis kelasnya (`grid-cols-1 md:grid-cols-2 lg:grid-cols-3`). Default lengkap di `references/responsive.md`. |
| 8 | **Section list** | Urutkan dari atas ke bawah, beri nomor: `01-hero`, `02-features`, `03-pricing`, … Lihat aturan pemecahan di bawah. |

Kalau sesuatu tidak terlihat (hover state, halaman kedua) → tulis di NOTES.md sebagai
asumsi. Jangan diam-diam mengarang.

#### Apa yang dihitung sebagai "section"

Section = **pita horizontal halaman** yang berdiri sendiri secara visual.
Komponen di dalamnya bukan section — komponen **melebur** ke file section induknya.

| Ini section (dapat folder) | Ini BUKAN section (lebur ke induk) |
|---|---|
| hero, features, pricing, testimonials, FAQ, CTA, footer, stats, gallery, blog list | navbar, menu mobile, tabs, dropdown, accordion item, breadcrumb, pagination, sidebar, search bar, modal, toast, card |

Aturan peleburan:

| Komponen | Melebur ke |
|---|---|
| Navbar / header / menu mobile | section paling atas (biasanya `01-hero`) |
| Tabs bulanan/tahunan | `0n-pricing` |
| Accordion | `0n-faq` |
| Breadcrumb | section pertama halaman itu |
| Pagination | section daftar/list-nya |
| Modal `:target` | section tempat tombol pemicunya berada |

Jadi `01-hero/index.html` berisi `<header>` **dan** `<section>` hero dalam satu file.
Jangan pernah bikin `sections/01-navbar/` atau `sections/xx-tabs/`.

### Step 2 — TOKENIZE: satu `@theme` untuk semua

Hasil Step 1 dijadikan satu blok `@theme` (lihat `references/setup.md`).
Blok ini **identik di semua file** — `index.html` dan setiap `sections/*/index.html`.
Satu sumber kebenaran; tidak ada warna hardcode di markup.

### Step 3 — BUILD per section

Untuk tiap section, urut dari `01`:

1. Tulis `sections/<nn>-<nama>/index.html` — **potongan HTML saja**, bukan halaman
   utuh. Mulai langsung dari `<header>`/`<section>`, akhiri di tag penutupnya.
   Tanpa `<!doctype>`, `<html>`, `<head>`, `<meta>`, `<title>`, `<body>`, `<script>`,
   `<style>`, `@theme` — semua itu hanya ada di `index.html` induk.
   Baris pertama: komentar penanda, mis.
   `<!-- 02-features — fragment. Tanpa <head>/<body>/@theme: itu milik index.html induk. -->`

   Konsekuensinya file section tidak bisa dibuka langsung di browser. Itu memang
   trade-off-nya — sampaikan ke user, dan arahkan preview lewat `index.html`.
2. Interaksi apa pun → pakai pola CSS-only dari `references/no-js-patterns.md`.
   Kalau tidak ada pola yang cocok, sederhanakan desainnya, **jangan tambah JS**.
3. Tulis `prompts/<nn>-<nama>.md` — prompt siap copy-paste untuk regenerate section
   itu dari gambar. **Prompt harus tahan model murah**: ditulis supaya Gemini Flash
   kelas menghasilkan output identik dengan model mahal. Artinya nol penalaran
   diperlukan — kerangka HTML sudah jadi, class ditulis literal, teks ada di tabel
   COPY, ditutup checklist verifikasi. **Struktur 5 blok wajib** (KONTRAK ·
   KERANGKA · COPY · CEK · KUNCI): aturan dan contoh lengkap di
   `references/prompt-template.md`.

   Prompt pendek, bukan panjang. Kerangka HTML ~80% isi file; prosa sisanya.
   Jangan hidupkan lagi blok whitelist class dan tabel responsif dari format
   lama — keduanya sudah terwakili kerangka, dan menambah teks tanpa menambah
   batasan justru menurunkan kepatuhan model murah.

**Trigger "buatkan promtingnya" (mode prompt saja).** Kalau user mengetik kalimat
itu — dengan atau tanpa nama section — kerjakan hanya poin 3 di atas:

- Sudah ada `sections/*/index.html` → turunkan kerangka Blok 2 langsung dari file
  section itu (salin utuh, sisipkan tanda `<!-- ISI:n -->` di titik teks), lalu
  tulis `prompts/<nn>-<nama>.md`. Jangan ubah file section-nya.
- Belum ada section, hanya ada gambar → jalankan Step 1 + Step 2 dulu (butuh token),
  lalu tulis prompt-nya. Lewati penulisan HTML final.
- User menyebut satu section (`buatkan promtingnya untuk 03-pricing`) → satu file itu
  saja. Tanpa sebutan → semua section di `sections/`.

Selesai → sebutkan path file yang dibuat dan ingatkan penanda
`▼ COPY MULAI` / `▲ COPY SELESAI` sebagai batas copy-paste.

### Step 4 — ASSEMBLE

`index.html` = satu `<head>` + satu `@theme` (boilerplate di `references/setup.md`),
lalu isi tiap file section ditempel berurutan ke dalam `<body>` **apa adanya**.

- Urutan sesuai nomor section.
- Tempel utuh — jangan potong, jangan reformat, jangan ubah class atau `id`. Isi
  `sections/<nn>/index.html` dan potongan di `index.html` harus identik; satu-satunya
  beda yang boleh ada adalah komentar (penanda fragment di file section, pemisah
  `<!-- ══ nn · NAMA ══ -->` di `index.html`).
- `id` section dan anchor `href="#..."` ditulis di file section, bukan ditambahkan
  saat assemble. Kalau ditambahkan belakangan, kedua file langsung menyimpang.
- Cek jarak antar section konsisten setelah digabung.

### Step 5 — VERIFY sebelum bilang selesai

Buka gambar lagi, banding baris per baris:

- [ ] Semua section di gambar ada di output — tidak ada yang dilewat.
- [ ] Urutan elemen dalam tiap section sama.
- [ ] Warna, radius, shadow, dan berat font cocok.
- [ ] Grep hasil: tidak ada `onclick`, `addEventListener`, `<script>` selain CDN Tailwind di `index.html`.
- [ ] Grep `sections/`: nol `<!doctype`, `<html`, `<head`, `<meta`, `<title`, `<body`, `<script`, `<style`, `@theme`.
      Tiap file mulai dari tag section-nya (setelah komentar penanda).
- [ ] **Lulus checklist responsif 17 poin** di `references/responsive.md` §9, diuji di
      **320 / 375 / 768 / 1024 / 1440px**. Ini bukan opsional — layout yang pecah di
      320px atau melebar tanpa batas di 1440px dihitung gagal, sebagus apa pun di 1440.
- [ ] Padding section, container, dan gap konsisten lintas section.
- [ ] Kontras teks ≥ 4.5:1; `focus-visible` ring masih ada di semua elemen interaktif.
- [ ] Semua `<img>` punya `alt`; heading turun berurutan (h1 → h2 → h3).

Ada yang gagal → perbaiki. Jangan lapor "100% mirip" sebelum checklist ini lulus.

---

## Output Structure

```
output/<nama-project>/
├── index.html                    # SATU-SATUNYA halaman utuh: <head>, @theme, semua section
├── NOTES.md                      # token hasil Step 1 + daftar asumsi
├── sections/                     # POTONGAN saja — tanpa <head>/<body>/@theme
│   ├── 01-hero/index.html        # <header> + <section>, navbar MENYATU di satu file
│   ├── 02-features/index.html    # <section> … </section>
│   └── 03-pricing/index.html     # tabs harga menyatu di sini
└── prompts/
    ├── 01-hero.md                # prompt copy-paste untuk regenerate potongan ini
    ├── 02-features.md
    └── 03-pricing.md
```

Satu section = satu folder = satu `index.html` = satu file prompt. Tidak ada
folder untuk navbar, tabs, atau komponen lain — semuanya di dalam file section induknya.

Contoh nyata bentuk ini ada di `_template/`. Tiru strukturnya persis.

**Kenapa `.md` bukan `.txt` untuk prompt:** code fence menjaga snippet HTML/CSS tetap
utuh saat di-copy, front-matter menyimpan metadata section, dan file-nya ter-render
rapi di editor maupun GitHub. `.txt` tidak memberi satu pun dari itu.

---

## Quick Reference

| Kebutuhan | Pakai ini | Jangan |
|---|---|---|
| Setup Tailwind | `@tailwindcss/browser@4` + `<style type="text/tailwindcss">` | `tailwind.config.js`, `@tailwind base` |
| Token warna | `@theme { --color-primary: … }` | hex acak di class |
| Accordion / FAQ | `<details><summary>` | script toggle |
| Menu mobile | `<details>` atau checkbox + `peer` | script toggle |
| Tabs | radio + `peer-checked:` | script |
| Modal | `:target` + anchor `#id` | `dialog.showModal()` |
| Carousel | `overflow-x-auto` + `snap-x snap-mandatory` | slider library |
| Dark mode | `@media (prefers-color-scheme: dark)` | toggle button |
| Ikon | inline `<svg>` | emoji, icon-font, `<script>` sprite |
| Dropdown form | `<select>` native | custom listbox |

Detail tiap pola: `references/no-js-patterns.md`.

## Spacing & Responsive Discipline

Skala 4px, tiga breakpoint saja (`base` / `md:` ≥768 / `lg:` ≥1024).
Default yang aman, geser hanya kalau gambar memang beda:

| Slot | base | `md:` | `lg:` |
|---|---|---|---|
| Padding vertikal section | `py-16` | `py-24` | `py-32` |
| Container | `mx-auto max-w-7xl px-6` | — | `lg:px-8` |
| Gap antar kolom | `gap-10` | `gap-12` | `gap-16` |
| Gap antar card | `gap-6` | `gap-6` | `gap-8` |
| Padding dalam card | `p-6` | `p-6` | `p-8` |
| Hero `<h1>` | `text-4xl` | `text-5xl` | `text-6xl` |
| Paragraf utama | `text-base` | `text-lg` | `text-lg` |
| Heading → body | `mt-6` | | |
| Gap grup tombol | `gap-3` + `flex-wrap` | | |
| Ukuran sentuh minimum | `min-h-11` / `size-11` (44px) | | |

Tiga hal yang paling sering bikin terlihat berantakan:

1. **Baris teks tanpa batas lebar** → paragraf melar di 1440px. Selalu `max-w-lg`
   (hero) atau `max-w-2xl` (section).
2. **Gap terlalu rapat saat kolom menumpuk** → dua blok terbaca menyatu di mobile.
   Karena itu `gap-10` di base, bukan `gap-6`.
3. **Nav desktop muncul di `md:`** → 768px terlalu sempit, item nav berdesakan.
   Tahan sampai `lg:`.

Nilai boleh berubah — **konsistensinya tidak**. Satu section pakai `py-24`,
section lain tidak boleh tiba-tiba `py-14` tanpa alasan dari gambar.

Peta runtuh grid, titik rawan pecah, dan checklist 17 poin: `references/responsive.md`.

## Common Mistakes

| Kesalahan | Perbaikan |
|---|---|
| Hanya cek di 1440px | Uji lima lebar: 320 / 375 / 768 / 1024 / 1440. |
| Paragraf tanpa `max-w-*` | Melar tak terbaca di layar lebar. Beri `max-w-lg` / `max-w-2xl`. |
| Nav desktop muncul di `md:` | Terlalu sempit di 768px. Tahan sampai `lg:`. |
| Tombol `py-2` tanpa `min-h-11` | Tinggi ~36px, di bawah ambang sentuh 44px. |
| Kartu paragraf jadi 2 kolom di 375px | Baris terlalu pendek. Tunggu `md:`. |
| Pakai Tailwind v3 config | v4 tidak punya file config. Semua token di `@theme`. |
| Coba `import { Button } from "@/components/ui/button"` | shadcn itu React. Tulis tangan markup-nya + token-nya. |
| Tambah JS "cuma untuk mobile menu" | `<details>` sudah cukup. Nol JS artinya nol. |
| File section punya `<head>`/`<body>`/`@theme` | Itu potongan, bukan halaman. Mulai dari tag section-nya. `@theme` hanya di `index.html`. |
| Prompt menyuruh AI keluarkan `<!doctype html>` | Output prompt = potongan. Mulai `<header`/`<section`, akhiri tag penutupnya. |
| Bikin folder `01-navbar/` atau `xx-tabs/` | Bukan section. Lebur ke file section induknya. |
| Prompt ditambah whitelist class / tabel responsif | Sudah terwakili kerangka. 5 blok saja. |
| Prompt panjang dikira prompt kuat | Yang bekerja cuma kerangka + tabel COPY. Sisanya dilewati model murah. |
| Warna dikira-kira | Sampel dari gambar. Kalau ragu, tulis di NOTES.md. |
| Section terakhir/footer dilewat | Semua section di gambar dikerjakan. |
| Hapus focus ring biar "bersih" | Ganti stylenya, jangan dihapus. |
| Lapor selesai tanpa banding ke gambar | Jalankan checklist Step 5 dulu. |

## References

- `references/setup.md` — boilerplate `<head>`, blok `@theme`, token shadcn-style
- `references/no-js-patterns.md` — resep interaksi CSS-only (copy-paste siap pakai)
- `references/responsive.md` — kontrak responsif: 5 lebar uji, peta runtuh grid, skala tipografi, checklist 17 poin
- `references/prompt-template.md` — cara menulis prompt yang tahan model murah (struktur 5 blok)
