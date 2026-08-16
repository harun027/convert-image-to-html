---
section: 01-hero
order: 1
file: sections/01-hero/index.html
image: refs/01-hero.png
includes: [navbar, menu-mobile]
---

# Prompt — Section 01: Hero (navbar menyatu)

Lampirkan `refs/01-hero.png`, lalu copy-paste seluruh blok di bawah.
Ditulis untuk model termurah (Gemini Flash kelas) — nol penalaran diperlukan.

## ▼ COPY-PASTE MULAI DARI SINI

### BLOK 1 — PERAN & KONTRAK OUTPUT

Kamu adalah generator HTML statis. Tugasmu mengisi kerangka HTML yang sudah disediakan.

Kontrak output:
1. Keluarkan HANYA kode HTML. Tanpa penjelasan, tanpa basa-basi, tanpa komentar penutup.
2. Mulai dari `<!doctype html>`, akhiri dengan `</html>`.
3. Satu file saja. Navbar dan hero berada di file yang sama.
4. Bungkus jawaban dalam satu blok kode.

---

### BLOK 2 — LARANGAN MUTLAK

Enam aturan. Setiap pelanggaran = output ditolak.

1. **Tanpa JavaScript.**
   SALAH: `<button onclick="toggleMenu()">` · `<script>document.querySelector(...)</script>`
   BENAR: `<details><summary>` untuk buka-tutup menu.

2. **Tanpa sintaks Tailwind v3.**
   SALAH: `tailwind.config = {...}` · `@tailwind base;` · `theme.extend.colors`
   BENAR: `<style type="text/tailwindcss">@theme { --color-primary: ... }</style>`

3. **Tanpa warna hardcode.**
   SALAH: `class="bg-[#0F172A]"` · `class="text-gray-500"` · `style="color:#333"`
   BENAR: `class="bg-primary"` · `class="text-muted-foreground"`

4. **Tanpa mengubah kerangka.**
   SALAH: menambah `<div>` pembungkus, mengganti `text-4xl` jadi `text-5xl`, menukar urutan elemen.
   BENAR: hanya mengganti setiap `<!-- ISI:n -->` dengan teks dari tabel COPY.

5. **Tanpa emoji atau icon-font sebagai ikon.**
   SALAH: `<span>🚀</span>` · `<i class="fa fa-arrow-right"></i>`
   BENAR: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">…</svg>`

6. **Tanpa elemen tambahan di luar kerangka.**
   SALAH: menambah badge, statistik, gradient, section kedua, atau footer.
   BENAR: keluarkan persis elemen yang ada di kerangka, tidak lebih.

---

### BLOK 3 — KERANGKA HTML SIAP ISI

Salin kerangka ini apa adanya. Ganti setiap `<!-- ISI:n -->` dengan teks dari tabel COPY di Blok 4.
**Jangan ubah satu pun class, tag, atribut, atau urutan di luar tanda ISI.**

```html
<!doctype html>
<html lang="id" class="scroll-smooth">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title><!-- ISI:1 --></title>
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style type="text/tailwindcss">
    @theme {
      --color-background:         oklch(1 0 0);
      --color-foreground:         oklch(0.145 0 0);
      --color-muted:              oklch(0.97 0 0);
      --color-muted-foreground:   oklch(0.556 0 0);
      --color-primary:            oklch(0.205 0 0);
      --color-primary-foreground: oklch(0.985 0 0);
      --color-border:             oklch(0.922 0 0);
      --color-ring:               oklch(0.708 0 0);
      --radius-card: 0.75rem;
      --radius-btn:  0.5rem;
      --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;
    }
    @media (prefers-color-scheme: dark) {
      @theme {
        --color-background:         oklch(0.145 0 0);
        --color-foreground:         oklch(0.985 0 0);
        --color-muted:              oklch(0.269 0 0);
        --color-muted-foreground:   oklch(0.708 0 0);
        --color-primary:            oklch(0.985 0 0);
        --color-primary-foreground: oklch(0.205 0 0);
        --color-border:             oklch(0.269 0 0);
      }
    }
  </style>
</head>
<body class="bg-background text-foreground font-sans antialiased">

  <header class="sticky top-0 z-40 border-b border-border bg-background/80 backdrop-blur">
    <div class="mx-auto flex max-w-7xl items-center justify-between gap-6 px-6 py-4 lg:px-8">

      <a href="#" class="flex items-center gap-2 text-base font-semibold tracking-tight">
        <span class="grid size-8 place-items-center rounded-btn bg-primary text-primary-foreground">◆</span>
        <!-- ISI:2 -->
      </a>

      <nav class="hidden lg:block">
        <ul class="flex items-center gap-8 text-sm text-muted-foreground">
          <li><a href="#" class="transition-colors hover:text-foreground"><!-- ISI:3 --></a></li>
          <li><a href="#" class="transition-colors hover:text-foreground"><!-- ISI:4 --></a></li>
          <li><a href="#" class="transition-colors hover:text-foreground"><!-- ISI:5 --></a></li>
        </ul>
      </nav>

      <div class="hidden items-center gap-3 lg:flex">
        <a href="#" class="inline-flex min-h-11 items-center px-3 text-sm font-medium text-muted-foreground transition-colors hover:text-foreground focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring"><!-- ISI:6 --></a>
        <a href="#" class="inline-flex min-h-11 items-center rounded-btn bg-primary px-5 text-sm font-medium text-primary-foreground transition-opacity hover:opacity-90 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring"><!-- ISI:7 --></a>
      </div>

      <details class="lg:hidden">
        <summary class="grid size-11 cursor-pointer list-none place-items-center rounded-btn marker:content-none hover:bg-muted focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring" aria-label="Buka menu">
          <svg class="size-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="M4 6h16M4 12h16M4 18h16"/></svg>
        </summary>
        <nav class="absolute inset-x-0 top-full border-b border-border bg-background px-6 py-4">
          <ul class="flex flex-col gap-1 text-sm">
            <li><a href="#" class="block rounded-btn px-3 py-3 hover:bg-muted"><!-- ISI:3 --></a></li>
            <li><a href="#" class="block rounded-btn px-3 py-3 hover:bg-muted"><!-- ISI:4 --></a></li>
            <li><a href="#" class="block rounded-btn px-3 py-3 hover:bg-muted"><!-- ISI:5 --></a></li>
          </ul>
          <a href="#" class="mt-4 inline-flex min-h-11 w-full items-center justify-center rounded-btn bg-primary px-5 text-sm font-medium text-primary-foreground"><!-- ISI:7 --></a>
        </nav>
      </details>

    </div>
  </header>

  <section class="py-16 md:py-24 lg:py-32">
    <div class="mx-auto grid max-w-7xl items-center gap-10 px-6 md:gap-12 lg:grid-cols-12 lg:gap-16 lg:px-8">

      <div class="lg:col-span-6">

        <span class="inline-flex items-center rounded-full border border-border px-3 py-1 text-xs font-medium uppercase tracking-widest text-muted-foreground">
          <!-- ISI:8 -->
        </span>

        <h1 class="mt-6 text-4xl font-semibold leading-[1.1] tracking-tight md:text-5xl lg:text-6xl">
          <!-- ISI:9 -->
        </h1>

        <p class="mt-6 max-w-lg text-base leading-relaxed text-muted-foreground md:text-lg">
          <!-- ISI:10 -->
        </p>

        <div class="mt-8 flex flex-wrap items-center gap-3">
          <a href="#" class="inline-flex min-h-11 w-full items-center justify-center rounded-btn bg-primary px-6 sm:w-auto text-sm font-medium text-primary-foreground transition-opacity hover:opacity-90 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring">
            <!-- ISI:11 -->
          </a>
          <a href="#" class="inline-flex min-h-11 w-full items-center justify-center gap-2 rounded-btn border border-border px-6 sm:w-auto text-sm font-medium transition-colors hover:bg-muted focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring">
            <!-- ISI:12 -->
            <svg class="size-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
          </a>
        </div>

        <div class="mt-12 flex flex-wrap items-center gap-x-8 gap-y-4 text-sm text-muted-foreground">
          <span><!-- ISI:13 --></span>
          <span class="font-medium text-foreground"><!-- ISI:14 --></span>
          <span class="font-medium text-foreground"><!-- ISI:15 --></span>
          <span class="font-medium text-foreground"><!-- ISI:16 --></span>
        </div>

      </div>

      <div class="lg:col-span-6">
        <div class="aspect-[4/3] w-full rounded-card border border-border bg-muted"></div>
      </div>

    </div>
  </section>

</body>
</html>
```

---

### BLOK 4 — TABEL COPY (teks literal, salin apa adanya)

| Tanda | Teks pengganti |
|---|---|
| `ISI:1` | Acme — Ubah gambar desain jadi web statis |
| `ISI:2` | Acme |
| `ISI:3` | Produk |
| `ISI:4` | Harga |
| `ISI:5` | Dokumentasi |
| `ISI:6` | Masuk |
| `ISI:7` | Mulai gratis |
| `ISI:8` | Versi 2.0 |
| `ISI:9` | `Ubah gambar desain<br>jadi web statis` |
| `ISI:10` | Tailwind v4, tanpa satu baris JavaScript perilaku. Satu halaman penuh plus file terpisah per section. |
| `ISI:11` | Mulai sekarang |
| `ISI:12` | Lihat contoh |
| `ISI:13` | Dipakai oleh |
| `ISI:14` | Northwind |
| `ISI:15` | Contoso |
| `ISI:16` | Fabrikam |

`ISI:9` berisi tag `<br>` — tulis apa adanya, jangan di-escape.
Jangan menambah, memotong, atau memparafrase teks mana pun di tabel ini.

---

### BLOK 5 — WHITELIST CLASS

Hanya class berikut yang boleh muncul di output. Class di luar daftar ini = output ditolak.

**Layout:** `mx-auto` `max-w-7xl` `grid` `grid-cols-12` `lg:grid-cols-12` `lg:col-span-6` `flex` `flex-col` `flex-wrap` `inline-flex` `items-center` `justify-between` `justify-center` `place-items-center` `hidden` `block` `relative` `absolute` `sticky` `inset-x-0` `top-0` `top-full` `z-40` `w-full` `aspect-[4/3]` `size-4` `size-6` `size-8` `size-11` `min-h-11` `shrink-0`

**Spacing:** `px-3` `px-5` `px-6` `py-1` `py-3` `py-4` `py-16` `md:py-24` `lg:py-32` `lg:px-8` `p-6` `mt-4` `mt-6` `mt-8` `mt-12` `gap-1` `gap-2` `gap-3` `gap-6` `gap-8` `gap-10` `md:gap-12` `lg:gap-16` `gap-x-8` `gap-y-4`

**Responsif:** `sm:w-auto` `md:py-24` `md:gap-12` `md:text-lg` `md:text-5xl` `lg:px-8` `lg:py-32` `lg:gap-16` `lg:grid-cols-12` `lg:col-span-6` `lg:text-6xl` `lg:hidden` `lg:flex` `lg:block`

**Tipografi:** `text-xs` `text-sm` `text-base` `text-lg` `text-4xl` `md:text-lg` `md:text-5xl` `lg:text-6xl` `font-medium` `font-semibold` `tracking-tight` `tracking-widest` `uppercase` `leading-relaxed` `leading-[1.1]` `antialiased` `max-w-lg`

**Warna (token saja):** `bg-background` `bg-background/80` `bg-primary` `bg-muted` `text-foreground` `text-primary-foreground` `text-muted-foreground` `border-border`

**Bentuk & efek:** `border` `border-b` `rounded-btn` `rounded-card` `rounded-full` `backdrop-blur` `transition-colors` `transition-opacity` `hover:bg-muted` `hover:text-foreground` `hover:opacity-90` `cursor-pointer` `list-none` `marker:content-none` `focus-visible:outline-2` `focus-visible:outline-offset-2` `focus-visible:outline-ring` `lg:hidden` `lg:flex` `lg:block` `scroll-smooth`

Class yang TIDAK boleh muncul (contoh halusinasi umum):
`text-gray-500` `bg-slate-900` `shadow-medium` `rounded-2.5xl` `text-[17px]` `font-bold` `p-[22px]` `bg-[#0F172A]`

---

### BLOK 6 — PERILAKU RESPONSIF (kunci verifikasi)

Semua kelas breakpoint sudah ada di kerangka Blok 3. Kamu tidak perlu menghitung
apa pun — cocokkan saja output-mu dengan tabel ini.

| Elemen | 320–767px | 768–1023px | 1024px ke atas |
|---|---|---|---|
| Nav desktop (`<nav class="hidden lg:block">`) | tersembunyi | tersembunyi | tampil |
| Tombol hamburger (`<details class="lg:hidden">`) | tampil | tampil | tersembunyi |
| Grup tombol Masuk/Mulai gratis | tersembunyi | tersembunyi | tampil |
| Kolom hero | menumpuk (1 kolom) | menumpuk (1 kolom) | bersebelahan (6 + 6) |
| Padding vertikal section | `py-16` = 64px | `py-24` = 96px | `py-32` = 128px |
| Padding tepi container | `px-6` = 24px | `px-6` = 24px | `px-8` = 32px |
| Jarak antar kolom | `gap-10` = 40px | `gap-12` = 48px | `gap-16` = 64px |
| Ukuran `<h1>` | `text-4xl` = 36px | `text-5xl` = 48px | `text-6xl` = 60px |
| Ukuran paragraf hero | `text-base` = 16px | `text-lg` = 18px | `text-lg` = 18px |
| Dua tombol CTA | `w-full`, menumpuk | `sm:w-auto`, sebaris | `sm:w-auto`, sebaris |
| Baris logo | membungkus 2 baris | sebaris | sebaris |

Tiga hal yang tidak boleh berubah di lebar mana pun:
- Lebar konten terkunci `max-w-7xl` (1280px). Di layar lebih lebar, sisanya jadi margin.
- Paragraf hero terkunci `max-w-lg` (512px). Jangan melebar mengikuti kolom.
- Semua tombol dan link tinggi minimal 44px (`min-h-11` / `size-11`).

---

### BLOK 7 — CHECKLIST SEBELUM MENGIRIM JAWABAN

Cek satu per satu. Ada yang gagal → perbaiki dulu, baru kirim.

1. Jumlah `<script>` di output tepat 1 (hanya tag CDN Tailwind).
2. Tidak ada string `onclick`, `addEventListener`, atau `javascript:`.
3. Tidak ada `<!-- ISI:` tersisa di output.
4. Jumlah `<h1>` tepat 1.
5. Tidak ada karakter `#` diikuti kode hex warna di dalam atribut `class`.
6. Setiap class di output ada di whitelist Blok 5.
7. Struktur kerangka Blok 3 utuh: `<header>` lalu `<section>`, tidak ada div tambahan.
8. Tag `<meta name="viewport" content="width=device-width, initial-scale=1">` ada, tanpa `user-scalable=no` dan tanpa `maximum-scale`.
9. Setiap baris di tabel Blok 6 cocok dengan kelas di output.
10. Tidak ada lebar px tetap (`w-[1200px]`), tidak ada `w-screen`, tidak ada `100vw`.
11. Output dimulai `<!doctype html>` dan diakhiri `</html>`.
12. Tidak ada teks penjelasan di luar blok kode.

---

### BLOK 8 — TIGA ATURAN PALING KRITIS (ulangan)

1. **NOL JavaScript.** Satu-satunya `<script>` adalah CDN Tailwind. Menu mobile pakai `<details>`.
2. **JANGAN ubah kerangka Blok 3.** Hanya ganti tanda `<!-- ISI:n -->`. Class, tag, dan urutan tetap.
3. **Keluarkan HANYA kode.** Mulai `<!doctype html>`, akhiri `</html>`, tanpa satu kalimat penjelasan.

## ▲ COPY-PASTE SAMPAI SINI
