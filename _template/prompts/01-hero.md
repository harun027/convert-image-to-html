---
section: 01-hero
order: 1
file: sections/01-hero/index.html
image: refs/01-hero.png
includes: [navbar, menu-mobile]
---

# Prompt — Section 01: Hero (navbar menyatu)

Lampirkan `refs/01-hero.png`, lalu copy seluruh isi di antara dua penanda di bawah
ke AI mana pun. Ditulis untuk model termurah — nol penalaran diperlukan.

Output prompt ini berupa **potongan HTML** (`<header>` + `<section>`), bukan halaman
utuh. `<head>`, `<body>`, dan blok `@theme` milik `index.html` induk.

## ▼ COPY MULAI

### BLOK 1 — KONTRAK

Kamu generator potongan HTML. Tugasmu mengisi kerangka di Blok 2.

Format output: HANYA potongan HTML dalam satu blok kode, mulai `<header` dan
akhiri `</section>`. Tanpa satu kalimat penjelasan.

Tiga larangan. Setiap pelanggaran = output ditolak.

1. **Tanpa pembungkus halaman.**
   SALAH: `<!doctype html>` · `<html>` · `<head>` · `<body>` · `<meta>` · `<title>` · `<script>` · `<style>` · `@theme`
   BENAR: mulai langsung dari `<header class="sticky top-0 …">`. Token warna dan Tailwind sudah dimuat oleh halaman induk.

2. **Tanpa JavaScript dan tanpa warna hardcode.**
   SALAH: `<button onclick="toggleMenu()">` · `class="bg-[#0F172A]"` · `class="text-gray-500"`
   BENAR: `<details><summary>` untuk buka-tutup menu · `class="bg-primary"` · `class="text-muted-foreground"`

3. **Tanpa mengubah kerangka.**
   SALAH: menambah `<div>` pembungkus, badge, gradient, atau footer; mengganti `text-4xl` jadi `text-5xl`; menukar urutan elemen.
   BENAR: hanya mengganti setiap `<!-- ISI:n -->` dengan teks dari tabel Blok 3.

---

### BLOK 2 — KERANGKA HTML SIAP ISI

Salin apa adanya. Ganti setiap `<!-- ISI:n -->` dengan teks dari tabel Blok 3.
**Jangan ubah satu pun class, tag, atribut, atau urutan di luar tanda ISI.**

```html
<header class="sticky top-0 z-40 border-b border-border bg-background/80 backdrop-blur">
  <div class="mx-auto flex max-w-7xl items-center justify-between gap-6 px-6 py-4 lg:px-8">

    <a href="#" class="flex items-center gap-2 text-base font-semibold tracking-tight">
      <span class="grid size-8 place-items-center rounded-btn bg-primary text-primary-foreground">◆</span>
      <!-- ISI:1 -->
    </a>

    <nav class="hidden lg:block">
      <ul class="flex items-center gap-8 text-sm text-muted-foreground">
        <li><a href="#hero" class="transition-colors hover:text-foreground"><!-- ISI:2 --></a></li>
        <li><a href="#hero" class="transition-colors hover:text-foreground"><!-- ISI:3 --></a></li>
        <li><a href="#hero" class="transition-colors hover:text-foreground"><!-- ISI:4 --></a></li>
      </ul>
    </nav>

    <div class="hidden items-center gap-3 lg:flex">
      <a href="#" class="inline-flex min-h-11 items-center px-3 text-sm font-medium text-muted-foreground transition-colors hover:text-foreground focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring"><!-- ISI:5 --></a>
      <a href="#" class="inline-flex min-h-11 items-center rounded-btn bg-primary px-5 text-sm font-medium text-primary-foreground transition-opacity hover:opacity-90 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring"><!-- ISI:6 --></a>
    </div>

    <details class="lg:hidden">
      <summary class="grid size-11 cursor-pointer list-none place-items-center rounded-btn marker:content-none hover:bg-muted focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring" aria-label="Buka menu">
        <svg class="size-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><path d="M4 6h16M4 12h16M4 18h16"/></svg>
      </summary>
      <nav class="absolute inset-x-0 top-full border-b border-border bg-background px-6 py-4">
        <ul class="flex flex-col gap-1 text-sm">
          <li><a href="#hero" class="block rounded-btn px-3 py-3 hover:bg-muted"><!-- ISI:2 --></a></li>
          <li><a href="#hero" class="block rounded-btn px-3 py-3 hover:bg-muted"><!-- ISI:3 --></a></li>
          <li><a href="#hero" class="block rounded-btn px-3 py-3 hover:bg-muted"><!-- ISI:4 --></a></li>
        </ul>
        <a href="#" class="mt-4 inline-flex min-h-11 w-full items-center justify-center rounded-btn bg-primary px-5 text-sm font-medium text-primary-foreground"><!-- ISI:6 --></a>
      </nav>
    </details>

  </div>
</header>

<section id="hero" class="py-16 md:py-24 lg:py-32">
  <div class="mx-auto grid max-w-7xl items-center gap-10 px-6 md:gap-12 lg:grid-cols-12 lg:gap-16 lg:px-8">

    <div class="lg:col-span-6">

      <span class="inline-flex items-center rounded-full border border-border px-3 py-1 text-xs font-medium uppercase tracking-widest text-muted-foreground">
        <!-- ISI:7 -->
      </span>

      <h1 class="mt-6 text-4xl font-semibold leading-[1.1] tracking-tight md:text-5xl lg:text-6xl">
        <!-- ISI:8 -->
      </h1>

      <p class="mt-6 max-w-lg text-base leading-relaxed text-muted-foreground md:text-lg">
        <!-- ISI:9 -->
      </p>

      <div class="mt-8 flex flex-wrap items-center gap-3">
        <a href="#" class="inline-flex min-h-11 w-full items-center justify-center rounded-btn bg-primary px-6 sm:w-auto text-sm font-medium text-primary-foreground transition-opacity hover:opacity-90 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring">
          <!-- ISI:10 -->
        </a>
        <a href="#" class="inline-flex min-h-11 w-full items-center justify-center gap-2 rounded-btn border border-border px-6 sm:w-auto text-sm font-medium transition-colors hover:bg-muted focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring">
          <!-- ISI:11 -->
          <svg class="size-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
      </div>

      <div class="mt-12 flex flex-wrap items-center gap-x-8 gap-y-4 text-sm text-muted-foreground">
        <span><!-- ISI:12 --></span>
        <span class="font-medium text-foreground"><!-- ISI:13 --></span>
        <span class="font-medium text-foreground"><!-- ISI:14 --></span>
        <span class="font-medium text-foreground"><!-- ISI:15 --></span>
      </div>

    </div>

    <div class="lg:col-span-6">
      <div class="aspect-[4/3] w-full rounded-card border border-border bg-muted"></div>
    </div>

  </div>
</section>
```

---

### BLOK 3 — TABEL COPY (teks literal, salin apa adanya)

| Tanda | Teks pengganti |
|---|---|
| `ISI:1` | Acme |
| `ISI:2` | Produk |
| `ISI:3` | Harga |
| `ISI:4` | Dokumentasi |
| `ISI:5` | Masuk |
| `ISI:6` | Mulai gratis |
| `ISI:7` | Versi 2.0 |
| `ISI:8` | `Ubah gambar desain<br>jadi web statis` |
| `ISI:9` | Tailwind v4, tanpa satu baris JavaScript perilaku. Satu halaman penuh plus file terpisah per section. |
| `ISI:10` | Mulai sekarang |
| `ISI:11` | Lihat contoh |
| `ISI:12` | Dipakai oleh |
| `ISI:13` | Northwind |
| `ISI:14` | Contoso |
| `ISI:15` | Fabrikam |

`ISI:8` berisi tag `<br>` — tulis apa adanya, jangan di-escape.
Jangan menambah, memotong, atau memparafrase teks mana pun di tabel ini.

---

### BLOK 4 — CEK SEBELUM MENGIRIM JAWABAN

Ada yang gagal → perbaiki dulu, baru kirim.

1. Output mulai `<header` dan berakhir `</section>`. Nol `<!doctype`, `<html>`, `<head>`, `<meta>`, `<title>`, `<body>`, `<style>`, `@theme`.
2. Nol `<script>`, `onclick`, `addEventListener`, `javascript:`.
3. Tidak ada `<!-- ISI:` tersisa; jumlah `<h1>` tepat 1.
4. Setiap class di output ada juga di kerangka Blok 2 — nol class tambahan, nol `#hex` di dalam `class`.
5. Semua kelas `md:` dan `lg:` dari kerangka utuh (`md:py-24` `lg:py-32` `lg:grid-cols-12` `lg:col-span-6` `md:text-5xl` `lg:text-6xl` `lg:hidden` `lg:flex` `lg:block` `lg:px-8`).
6. Urutan struktur utuh: `<header>` lalu `<section>`, tanpa div tambahan, tanpa teks di luar blok kode.

---

### BLOK 5 — DUA KUNCI (ulangan)

1. **POTONGAN saja.** Mulai `<header`, akhiri `</section>`. Tanpa `<head>`, `<body>`, `<style>`, `<script>`.
2. **JANGAN ubah kerangka Blok 2.** Hanya ganti `<!-- ISI:n -->`. Class, tag, dan urutan tetap.

## ▲ COPY SELESAI
