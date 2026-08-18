# Resep Interaksi CSS-Only

Semua pola di bawah nol JavaScript. Copy-paste, sesuaikan class dengan token project.
Kalau sebuah interaksi tidak ada di sini dan tidak bisa dibuat tanpa JS →
sederhanakan desainnya dan catat di `NOTES.md`. Jangan tambah script.

---

## Accordion / FAQ — `<details>`

Native, aksesibel, keyboard-ready gratis.

```html
<div class="divide-y divide-border rounded-2xl border border-border">
  <details class="group p-6 open:bg-muted/40">
    <summary class="flex cursor-pointer list-none items-center justify-between gap-4
                    font-medium marker:content-none
                    focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring">
      Apa itu produk ini?
      <svg class="size-5 shrink-0 text-muted-foreground transition-transform duration-200 group-open:rotate-180"
           viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true">
        <path d="m6 9 6 6 6-6"/>
      </svg>
    </summary>
    <p class="mt-4 text-sm leading-relaxed text-muted-foreground">Jawabannya di sini.</p>
  </details>
  <!-- ulangi <details> berikutnya -->
</div>
```

Hanya satu boleh terbuka → beri `name="faq"` yang sama di setiap `<details>`
(exclusive accordion, didukung browser modern).

---

## Menu mobile — `<details>` sebagai drawer

```html
<details class="lg:hidden">
  <summary class="flex size-11 cursor-pointer items-center justify-center rounded-lg
                  list-none marker:content-none
                  focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring"
           aria-label="Buka menu">
    <svg class="size-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true">
      <path d="M4 6h16M4 12h16M4 18h16"/>
    </svg>
  </summary>
  <nav class="absolute inset-x-0 top-full border-b border-border bg-background p-6">
    <ul class="flex flex-col gap-1">
      <li><a href="#fitur" class="block rounded-lg px-3 py-3 hover:bg-muted">Fitur</a></li>
      <li><a href="#harga" class="block rounded-lg px-3 py-3 hover:bg-muted">Harga</a></li>
    </ul>
  </nav>
</details>
```

---

## Segmented control / toggle (tanpa panel) — bungkus per segmen

Dipakai untuk toggle harga bulanan/tahunan, filter, dan tab yang cuma mengubah
tampilan tombolnya. **Tiap input + label dibungkus div sendiri**, lalu pakai `peer`
polos — bukan `peer/<nama>`.

```html
<div class="inline-flex items-center rounded-full border border-border bg-muted p-1">
  <div>
    <input type="radio" name="billing" id="bill-m" class="peer sr-only" checked>
    <label for="bill-m" class="inline-flex min-h-11 cursor-pointer items-center rounded-full px-5 text-sm font-medium text-muted-foreground transition-colors peer-checked:bg-primary peer-checked:text-primary-foreground">Bulanan</label>
  </div>
  <div>
    <input type="radio" name="billing" id="bill-y" class="peer sr-only">
    <label for="bill-y" class="inline-flex min-h-11 cursor-pointer items-center rounded-full px-5 text-sm font-medium text-muted-foreground transition-colors peer-checked:bg-primary peer-checked:text-primary-foreground">Tahunan</label>
  </div>
</div>
```

**Kenapa dibungkus.** `peer-checked:` menghasilkan selector `.peer:checked ~ *`, dan
`~` hanya menjangkau **saudara dalam induk yang sama**. Begitu tiap pasangan punya
div sendiri, sebuah radio hanya bisa memengaruhi label di dalam div-nya. Kontaminasi
silang jadi mustahil — bukan dicegah oleh aturan, tapi oleh struktur.

**Efek sampingnya yang paling berharga: kedua label memakai string class yang sama
persis.** Tidak ada yang perlu dibedakan per label, jadi tidak ada yang bisa keliru
ditukar — baik oleh manusia maupun oleh model yang meng-generate markup-nya.

### Jangan pakai pola ini

```html
<!-- SALAH: named peer, semua input di atas, label bergantung nama -->
<input class="peer/m sr-only" checked><input class="peer/y sr-only">
<label class="peer-checked/m:bg-primary">Bulanan</label>
<label class="peer-checked/y:bg-primary">Tahunan</label>
```

Kelihatan benar dan **memang benar saat pertama render**. Tapi begitu satu label
tidak sengaja ikut membawa varian milik label lain, kedua segmen menyala bersamaan
setelah diklik — bug yang tidak muncul di keadaan awal, jadi lolos dari pemeriksaan
sekilas. Ini kegagalan nyata yang sudah terjadi di repo ini.

## Tabs dengan panel — di sini named peer memang perlu

Kalau ada panel yang harus ikut berganti, panel bukan saudara dari input di dalam
div pembungkus, jadi bungkus per segmen tidak bisa dipakai. Named peer wajib:

```html
<div class="w-full">
  <input type="radio" name="tab" id="tab-1" class="peer/t1 sr-only" checked>
  <input type="radio" name="tab" id="tab-2" class="peer/t2 sr-only">

  <div role="tablist" class="inline-flex gap-1 rounded-lg bg-muted p-1">
    <label for="tab-1" class="cursor-pointer rounded-lg px-4 py-2 text-sm font-medium text-muted-foreground peer-checked/t1:bg-background peer-checked/t1:text-foreground peer-checked/t1:shadow-sm">Bulanan</label>
    <label for="tab-2" class="cursor-pointer rounded-lg px-4 py-2 text-sm font-medium text-muted-foreground peer-checked/t2:bg-background peer-checked/t2:text-foreground peer-checked/t2:shadow-sm">Tahunan</label>
  </div>

  <div class="mt-6 hidden peer-checked/t1:block">Isi panel bulanan.</div>
  <div class="mt-6 hidden peer-checked/t2:block">Isi panel tahunan.</div>
</div>
```

Semua `<input>` di paling atas container, sebelum label dan panel — `peer` hanya
bekerja untuk saudara **sesudahnya**. Karena di sini tiap label memang harus beda,
periksa manual: label `t1` tidak boleh membawa satu pun varian `/t2`, dan sebaliknya.

---

## Modal / dialog — `:target`

Buka lewat anchor `#id`, tutup lewat anchor `#`.

```html
<a href="#modal-demo" class="text-sm font-medium underline underline-offset-4">Lihat detail</a>

<div id="modal-demo"
     class="invisible fixed inset-0 z-50 grid place-items-center bg-black/50 p-6 opacity-0
            transition-opacity target:visible target:opacity-100">
  <div class="w-full max-w-md rounded-2xl bg-card p-6 shadow-xl">
    <div class="flex items-start justify-between gap-4">
      <h2 class="text-lg font-semibold">Judul modal</h2>
      <a href="#" aria-label="Tutup"
         class="grid size-9 place-items-center rounded-lg text-muted-foreground hover:bg-muted">✕</a>
    </div>
    <p class="mt-3 text-sm text-muted-foreground">Isi modal.</p>
  </div>
</div>
```

Batasannya: menambah entri history browser dan tidak mengunci fokus.
Kalau focus-trap wajib, itu butuh JS — bilang ke user.

---

## Carousel / slider — scroll-snap

```html
<div class="flex snap-x snap-mandatory gap-6 overflow-x-auto pb-4
            [scrollbar-width:none] [&::-webkit-scrollbar]:hidden">
  <article class="w-[85%] shrink-0 snap-center rounded-2xl border border-border p-6 sm:w-80">Slide 1</article>
  <article class="w-[85%] shrink-0 snap-center rounded-2xl border border-border p-6 sm:w-80">Slide 2</article>
  <article class="w-[85%] shrink-0 snap-center rounded-2xl border border-border p-6 sm:w-80">Slide 3</article>
</div>
```

Butuh dot navigasi → beri `id` tiap slide dan pakai anchor `<a href="#slide-2">`
(`scroll-smooth` di `<html>` membuat perpindahannya halus).

---

## Dropdown menu — `<details>` + posisi absolut

```html
<details class="relative">
  <summary class="flex min-h-11 cursor-pointer list-none items-center gap-2 rounded-lg
                  border border-border px-4 text-sm marker:content-none">
    Opsi
    <svg class="size-4" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true"><path d="m6 9 6 6 6-6"/></svg>
  </summary>
  <ul class="absolute right-0 z-20 mt-2 w-48 rounded-2xl border border-border bg-card p-1 shadow-lg">
    <li><a href="#" class="block rounded-lg px-3 py-2 text-sm hover:bg-muted">Profil</a></li>
    <li><a href="#" class="block rounded-lg px-3 py-2 text-sm hover:bg-muted">Pengaturan</a></li>
  </ul>
</details>
```

Tidak bisa auto-close saat klik di luar tanpa JS. Terima itu, atau pakai `<select>` native.

---

## Dark mode toggle — checkbox + `:has()`

Default: ikut OS lewat `prefers-color-scheme` (lihat `setup.md`). Kalau user
benar-benar minta tombol manual:

```html
<style type="text/tailwindcss">
  /* variant `dark:` aktif saat checkbox #theme-toggle tercentang */
  @custom-variant dark (&:where(body:has(#theme-toggle:checked), body:has(#theme-toggle:checked) *));
</style>

<body class="bg-background text-foreground dark:bg-neutral-950 dark:text-neutral-50">
  <input type="checkbox" id="theme-toggle" class="sr-only">
  <label for="theme-toggle"
         class="grid size-11 cursor-pointer place-items-center rounded-lg hover:bg-muted"
         aria-label="Ganti tema">
    <svg class="size-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true">
      <path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z"/>
    </svg>
  </label>
  ...
</body>
```

Checkbox harus jadi anak `<body>` agar `:has()` di atas menemukannya.

Konsekuensi: pilihan tema tidak tersimpan setelah reload (butuh JS/localStorage).

---

## Validasi form — pseudo-class native

```html
<input type="email" required placeholder="nama@email.com"
       class="peer w-full min-h-11 rounded-lg border border-border px-3
              invalid:not-placeholder-shown:border-red-500">
<p class="mt-1 hidden text-xs text-red-600 peer-[:invalid:not(:placeholder-shown)]:block">
  Format email tidak valid.
</p>
```

`not-placeholder-shown` mencegah field kosong langsung ditandai merah sebelum diisi.

---

## Yang TIDAK bisa tanpa JS

Kalau gambar menuntut salah satu dari ini, laporkan ke user dan tawarkan alternatif statis:

| Fitur | Alternatif statis |
|---|---|
| Auto-play carousel | Scroll-snap manual |
| Toast / notifikasi muncul sendiri | Alert banner statis |
| Search / filter langsung | Daftar statis, atau `<form>` GET |
| Chart dinamis | SVG statis (bar/line digambar tangan) |
| Infinite scroll | Pagination dengan anchor link |
| Focus trap di modal | Modal `:target` biasa (catat batasannya) |
| Tema tersimpan setelah reload | Ikut OS via `prefers-color-scheme` |

## Anti-Pattern

- `onclick="..."` di markup — sekecil apa pun.
- `<script>` selain CDN Tailwind.
- `href="javascript:void(0)"`.
- Menghapus focus ring tanpa menggantinya.
- Emoji dipakai sebagai ikon UI (di luar contoh toggle di atas — ganti dengan `<svg>`).
