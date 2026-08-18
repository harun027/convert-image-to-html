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

### Step 2 — TOKENIZE: `:root` + `@theme inline`

Hasil Step 1 ditulis dengan pola shadcn/Tailwind v4 — tiga blok, bukan satu
(boilerplate lengkap di `references/setup.md`):

```css
:root { --primary: oklch(0.205 0 0); --border: … }              /* nilai mentah, di elemen <html> */
@media (prefers-color-scheme: dark) { :root { --primary: … } }  /* timpa nilai mentahnya */
@theme inline { --color-primary: var(--primary); … }            /* petakan ke utility */
```

Tiga hal yang mengikat:

- **`inline` wajib.** Tanpa itu Tailwind membekukan nilai saat kompilasi, `bg-primary`
  keluar warna mati, dan override `:root` untuk dark mode diam-diam tidak jalan.
- **`@theme` tidak sah di dalam `@media`** — hanya di level teratas. Override dark mode
  selalu lewat `:root`, bukan `@theme`.
- **Radius tidak ditokenkan.** Pakai skala bawaan Tailwind langsung di markup
  (`rounded-lg`, `rounded-xl`, `rounded-2xl`, `rounded-full`).

Blok ini ada **hanya di `index.html`**. File section tidak mengulanginya —
mereka cuma memakai class hasilnya (`bg-primary`, `border-border`, …).

### Step 3 — BUILD per section

Untuk tiap section, urut dari `01`:

1. Tulis `sections/<nn>-<nama>/index.html` — **markup section murni, tidak lebih.**
   Baris pertama langsung tag akarnya, baris terakhir tag penutupnya.

   ```html
   <section class="bg-background text-foreground py-16 md:py-24 lg:py-32">
     <div class="mx-auto max-w-7xl px-6 lg:px-8">
       …
     </div>
   </section>
   ```

   Dilarang ada di file ini: `<!doctype>`, `<html>`, `<head>`, `<meta>`, `<title>`,
   `<body>`, `<script>`, `<style>`, `@theme`, dan komentar penanda apa pun.
   Semua itu milik `index.html`.

   Dua aturan yang bikin potongan ini berdiri sendiri secara visual:

   - **Warna pakai palet bawaan Tailwind, bukan token proyek.** `bg-white`,
     `text-neutral-900`, `text-neutral-500`, `border-neutral-200`, `bg-neutral-100`,
     `bg-neutral-900`. Token proyek (`bg-primary`, `bg-background`, `bg-brand-*`)
     hanya ada di `index.html` — kalau dipakai di file section, section itu tampil
     tanpa warna di mana pun token-nya tidak dimuat. Palet bawaan selalu ada.
     Warna aksen yang bukan abu-abu → padanan bawaan terdekat (`bg-blue-500`,
     `bg-amber-300`). Nol `var(--…)` di file section.
   - **Tag akar membawa warnanya sendiri:** `bg-white text-neutral-900`
     (atau `bg-neutral-100`/`bg-neutral-950` kalau section itu memang beda dari gambar).
     Section tidak boleh menggantungkan warna dasarnya pada `<body>`.
   - **Radius pakai skala bawaan Tailwind** — `rounded-lg`, `rounded-xl`,
     `rounded-2xl`, `rounded-full`. Jangan bikin token radius kustom
     (`rounded-card`, `rounded-btn`); itu satu lapis tak perlu antara gambar dan kode.

   Hasilnya: file section tampil hitam-putih-abu yang menyerupai `index.html`, di
   editor mana pun, tanpa satu baris CSS di dalamnya. Warna penuh hanya di `index.html`.

   **Wajib: tulis peta padanannya di `NOTES.md`** (§Peta warna) — dua kolom, bawaan ↔
   token. Tanpa peta itu, assemble jadi menebak-nebak.

2. Interaksi apa pun → pakai pola CSS-only dari `references/no-js-patterns.md`.
   Kalau tidak ada pola yang cocok, sederhanakan desainnya, **jangan tambah JS**.
3. Tulis `prompts/<nn>-<nama>.md` — **brief desain berbentuk prosa**, bukan kerangka
   HTML. Isinya menjelaskan apa yang harus dibangun dengan nama class Tailwind yang
   konkret; generator markup di seberang yang menulis HTML-nya.

   Tiga aturan keras:

   - **Maksimum 5.000 karakter per file** — seluruh isi file, termasuk spasi, tab, dan
     baris baru. Itu batas kotak input tool generator; lewat satu karakter pun terpotong.
   - **Nol tab, nol indentasi.** Tiap baris mulai di kolom 1.
   - **Simpan LF, bukan CRLF.** CRLF menambah satu karakter per baris dan diam-diam
     menendang file melewati batas.

   Seluruh isi file adalah prompt — tanpa front-matter, tanpa judul markdown, tanpa
   penanda copy, tanpa blok kode. User blok seluruh file, salin, tempel.

   Enam blok, urutannya mengikat: kalimat pembuka · `LAYOUT —` · blok per bagian ·
   blok data · `STYLE REQUIREMENTS:` · `CONSTRAINTS:`. Judul blok KAPITAL tanpa `#`.
   Setiap butir menyebut elemen, teks literalnya dalam tanda kutip, lalu class-nya
   dalam kurung. Aturan lengkap + contoh di `references/prompt-template.md`.

**Trigger "buatkan promtingnya" (mode prompt saja).** Kalau user mengetik kalimat
itu — dengan atau tanpa nama section — kerjakan hanya poin 3 di atas:

- Sudah ada `sections/*/index.html` → prompt itu **transkrip file tersebut**, bukan
  brief desain baru. Salin daftar class tiap elemen apa adanya ke dalam kalimat, lalu
  **cek balik**: tiap nilai di prompt harus ada di file. Dilarang "memperbaiki" —
  `p-6` tetap `p-6`, bukan `p-6 lg:p-8`. Jangan ubah file section-nya.
- Belum ada section, hanya ada gambar → jalankan Step 1 + Step 2 dulu (butuh token),
  lalu tulis prompt-nya. Lewati penulisan HTML final.
- User menyebut satu section (`buatkan promtingnya untuk 03-pricing`) → satu file itu
  saja. Tanpa sebutan → semua section di `sections/`.

Selesai → sebutkan path file yang dibuat dan jumlah karakternya, dan ingatkan bahwa
seluruh isi file itu yang di-paste (tidak ada bagian yang perlu dipilah).

### Step 4 — ASSEMBLE

**Sebelum menulis `index.html`**, invoke skill `ui-ux-pro-max`, `emil-design-eng`,
`design-taste-frontend-v1`, dan `ponytail:ponytail` — dipakai bersamaan untuk
memandu keputusan desain (palet, tipografi, hierarki) sekaligus menjaga markup
tetap minimal, bukan sebagai pengganti Step 1–3.

`index.html` = satu `<head>` + satu blok token (boilerplate di `references/setup.md`),
lalu markup tiap section ditempel berurutan ke dalam `<body>` **apa adanya**.

- Urutan sesuai nomor section.
- Tempel utuh — jangan potong, jangan reformat, jangan ubah struktur, class layout,
  atau `id`.
- **Terjemahkan warnanya** lewat peta di `NOTES.md` §Peta warna: `bg-white` →
  `bg-background`, `bg-neutral-900` → `bg-primary`, dst. Ini satu-satunya perbedaan
  yang boleh ada antara file section dan potongan di `index.html` (selain komentar
  pemisah `<!-- ══ nn · NAMA ══ -->`). Kalau ada beda lain, salah satunya basi.
- `id` section dan anchor `href="#..."` ditulis di file section, bukan ditambahkan
  saat assemble. Kalau ditambahkan belakangan, kedua file langsung menyimpang.
- Cek jarak antar section konsisten setelah digabung.

### Step 5 — VERIFY sebelum bilang selesai

Buka gambar lagi, banding baris per baris:

- [ ] Semua section di gambar ada di output — tidak ada yang dilewat.
- [ ] Urutan elemen dalam tiap section sama.
- [ ] Warna, radius, shadow, dan berat font cocok.
- [ ] Grep hasil: tidak ada `onclick`, `addEventListener`, `<script>` selain CDN Tailwind.
- [ ] Grep `sections/`: nol `<!doctype`, `<html`, `<head`, `<meta`, `<title`, `<body`, `<script`, `<style`, `@theme`.
      Tiap file mulai dari tag akar section-nya.
- [ ] Tag akar tiap section membawa warna sendiri (`bg-*` + `text-*`), tidak menggantung ke `<body>`.
- [ ] Grep `sections/`: nol token proyek (`bg-primary`, `bg-background`, `bg-brand-*`, `border-border`, `text-muted-foreground`) dan nol `var(--`.
- [ ] `NOTES.md` punya §Peta warna, dan tiap baris di dalamnya benar-benar dipakai di kedua file.
- [ ] `index.html` memakai `@theme inline` (bukan `@theme` polos), dan nol `@theme` di dalam `@media`.
- [ ] Radius memakai skala bawaan (`rounded-lg`/`rounded-xl`/`rounded-2xl`/`rounded-full`), nol token radius kustom.
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
├── index.html                    # SATU-SATUNYA halaman utuh: <head>, token, semua section
├── NOTES.md                      # token hasil Step 1 + daftar asumsi
├── sections/                     # MARKUP MURNI — tanpa <head>/<body>/<script>/<style>
│   ├── 01-hero/index.html        # <header> + <section>, navbar MENYATU
│   ├── 02-features/index.html    # <section> … </section>
│   └── 03-pricing/index.html     # tabs harga menyatu di sini
└── prompts/
    ├── 01-hero.md                # brief prosa, ≤5.000 karakter, seluruh isinya di-paste
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
| Token warna | `:root { --primary: … }` + `@theme inline` di `index.html` | hex acak di class, `@theme` polos |
| Radius | `rounded-lg` / `rounded-xl` / `rounded-2xl` | token kustom `rounded-card` |
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
| Pakai Tailwind v3 config | v4 tidak punya file config. Token di `:root` + `@theme inline`. |
| Coba `import { Button } from "@/components/ui/button"` | shadcn itu React. Tulis tangan markup-nya + token-nya. |
| Tambah JS "cuma untuk mobile menu" | `<details>` sudah cukup. Nol JS artinya nol. |
| File section punya `<head>`/`<body>`/`<script>`/`<style>` | Itu markup section murni. Mulai dari tag akarnya. |
| Tag akar section tanpa `bg-*`/`text-*` | Warna menggantung ke `<body>`. Section harus bawa warnanya sendiri. |
| Token proyek (`bg-primary`, `var(--x)`) dipakai di file section | Tampil tanpa warna di luar `index.html`. Pakai palet bawaan. |
| Tempel section ke `index.html` tanpa terjemah warna | Halaman penuh jadi abu-abu. Jalankan peta di `NOTES.md`. |
| Bikin token radius kustom (`rounded-card`) | Pakai skala bawaan: `rounded-lg`, `rounded-xl`, `rounded-2xl`. |
| Pakai `@theme` polos, bukan `@theme inline` | Nilai beku saat kompilasi → dark mode diam-diam mati. `inline` wajib. |
| `@theme` ditaruh di dalam `@media` | Tidak sah. Override dark mode lewat `:root`. |
| Prompt menyuruh AI keluarkan `<!doctype html>` atau `<style>` | Output prompt = markup section murni. |
| Bikin folder `01-navbar/` atau `xx-tabs/` | Bukan section. Lebur ke file section induknya. |
| File prompt berisi kerangka HTML | Prompt itu brief prosa. Markup ditulis generator di seberang. |
| File prompt > 5.000 karakter | Terpotong di kotak input tool. Padatkan butirnya, jangan buang nama class. |
| File prompt disimpan CRLF | +1 karakter per baris. Simpan LF. |
| Prompt menulis nilai spacing sebagai rentang ("p-6 to p-8") | Model memilih beda-beda tiap kartu. Satu nilai per slot. |
| Kedua segmen tab/toggle menyala bersamaan | Prompt memakai placeholder (`peer-checked/X:`). Tulis per label, literal, plus kata `ONLY`. |
| Nama kelas di prompt memakai placeholder | Placeholder = penalaran = bug. Tulis tiap varian utuh per elemen. |
| Prompt mendeskripsikan bentuk ikon | Sebut nama Lucide-nya, dan `d="…"` kalau file section memakai path spesifik. |
| Nilai di prompt tidak ada di file section | Itu improvisasi. Prompt = transkrip file, cek balik satu per satu. |
| Prompt menambah `lg:` yang tidak ada di file | Hasilnya melenceng dari section. Salin apa adanya. |
| File prompt punya front-matter / judul / penanda copy | Seluruh isi file harus bisa di-paste apa adanya. |
| Prompt pakai tab atau baris ber-indentasi | Nol tab. Tiap baris mulai di kolom 1. |
| Warna dikira-kira | Sampel dari gambar. Kalau ragu, tulis di NOTES.md. |
| Section terakhir/footer dilewat | Semua section di gambar dikerjakan. |
| Hapus focus ring biar "bersih" | Ganti stylenya, jangan dihapus. |
| Lapor selesai tanpa banding ke gambar | Jalankan checklist Step 5 dulu. |

## References

- `references/setup.md` — boilerplate `<head>`, token `:root` + `@theme inline` (pola shadcn v4)
- `references/no-js-patterns.md` — resep interaksi CSS-only (copy-paste siap pakai)
- `references/responsive.md` — kontrak responsif: 5 lebar uji, peta runtuh grid, skala tipografi, checklist 17 poin
- `references/prompt-template.md` — cara menulis file prompt: brief prosa, ≤5.000 karakter, enam blok
