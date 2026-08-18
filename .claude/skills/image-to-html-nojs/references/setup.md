# Setup — Tailwind v4 tanpa build, token shadcn-style

## Boilerplate `<head>` — hanya untuk `index.html`

Blok ini ditulis **satu kali saja**, di `index.html`. File di `sections/*/index.html`
adalah markup section murni — tanpa `<head>`, `<body>`, `<script>`, maupun `<style>`
(lihat §Bentuk file section di bawah).

Ganti nilai di `:root` dengan hasil ukur dari gambar.

```html
<!doctype html>
<html lang="id" class="scroll-smooth">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Nama Project</title>

  <!-- Satu-satunya <script> yang boleh ada: compiler Tailwind, bukan logika aplikasi -->
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

  <style type="text/tailwindcss">
    /* 1 · Nilai mentah menempel di :root — itu elemen <html>. Hasil sampel dari gambar. */
    :root {
      --background:         oklch(1 0 0);
      --foreground:         oklch(0.145 0 0);
      --card:               oklch(1 0 0);
      --card-foreground:    oklch(0.145 0 0);
      --muted:              oklch(0.97 0 0);
      --muted-foreground:   oklch(0.556 0 0);
      --primary:            oklch(0.205 0 0);
      --primary-foreground: oklch(0.985 0 0);
      --accent:             oklch(0.97 0 0);
      --accent-foreground:  oklch(0.145 0 0);
      --border:             oklch(0.922 0 0);
      --input:              oklch(0.922 0 0);
      --ring:               oklch(0.708 0 0);
    }

    /* 2 · Dark mode ikut OS. Nol JS, jadi bukan class .dark — cukup timpa :root.
           Kalau user benar-benar minta tombol, lihat no-js-patterns.md §Dark mode toggle. */
    @media (prefers-color-scheme: dark) {
      :root {
        --background:         oklch(0.145 0 0);
        --foreground:         oklch(0.985 0 0);
        --card:               oklch(0.205 0 0);
        --card-foreground:    oklch(0.985 0 0);
        --muted:              oklch(0.269 0 0);
        --muted-foreground:   oklch(0.708 0 0);
        --primary:            oklch(0.985 0 0);
        --primary-foreground: oklch(0.205 0 0);
        --accent:             oklch(0.269 0 0);
        --border:             oklch(0.269 0 0);
      }
    }

    /* 3 · Petakan ke utility. */
    @theme inline {
      --color-background:         var(--background);
      --color-foreground:         var(--foreground);
      --color-card:               var(--card);
      --color-card-foreground:    var(--card-foreground);
      --color-muted:              var(--muted);
      --color-muted-foreground:   var(--muted-foreground);
      --color-primary:            var(--primary);
      --color-primary-foreground: var(--primary-foreground);
      --color-accent:             var(--accent);
      --color-accent-foreground:  var(--accent-foreground);
      --color-border:             var(--border);
      --color-input:              var(--input);
      --color-ring:               var(--ring);
      --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;
    }
  </style>
</head>
<body class="bg-background text-foreground font-sans antialiased">

  <!-- isi sections/01-hero/index.html ditempel di sini, apa adanya -->
  <!-- isi sections/02-features/index.html ditempel di sini, apa adanya -->

</body>
</html>
```

### Kenapa tiga blok, bukan satu `@theme`

Ini pola shadcn di Tailwind v4, dan bukan sekadar gaya:

| Blok | Perannya |
|---|---|
| `:root { --primary: … }` | Nilai mentah, menempel di elemen `<html>`. Ini yang di-override saat dark mode. |
| `@media … { :root { … } }` | Timpa nilai mentahnya saja. `@theme` **tidak sah** di dalam `@media` — hanya di level teratas. |
| `@theme inline { --color-primary: var(--primary) }` | Bikin utility. Kata `inline` bikin `bg-primary` jadi `background-color: var(--primary)`. |

Tanpa `inline`, Tailwind membekukan nilai `--primary` saat kompilasi, jadi `bg-primary`
keluar `oklch(0.205 0 0)` mati — override `:root` di blok 2 tidak ke-baca dan dark mode
diam-diam tidak jalan. Itu alasan `inline` wajib, bukan opsional.

`@theme inline` hanya untuk **warna dan font**. Radius tidak ditokenkan — pakai skala
bawaan Tailwind langsung di markup (`rounded-lg`, `rounded-xl`, `rounded-2xl`,
`rounded-full`), supaya nilainya kelihatan saat membaca kode.

Hasilnya utility biasa di markup: `--color-primary` → `bg-primary` / `text-primary` /
`border-primary`.

## Bentuk file section

`sections/<nn>-<nama>/index.html` = **markup section murni**. Baris pertama langsung
tag akarnya, baris terakhir tag penutupnya.

```html
<section class="bg-white text-neutral-900 py-16 md:py-24 lg:py-32">
  <div class="mx-auto max-w-7xl px-6 lg:px-8">
    <div class="rounded-2xl border border-neutral-200 bg-white p-6 lg:p-8">…</div>
  </div>
</section>
```

Dilarang ada di file ini: `<!doctype>`, `<html>`, `<head>`, `<meta>`, `<title>`,
`<body>`, `<script>`, `<style>`, `@theme`, dan komentar penanda apa pun.

**Warna pakai palet bawaan Tailwind, bukan token proyek.** Token (`bg-primary`,
`bg-background`, `bg-brand-*`) hidup di `<head>` `index.html`; kalau dipakai di file
section, section itu tampil tanpa warna di mana pun token-nya tidak dimuat. Palet
bawaan selalu ada, jadi file section tetap tampil hitam-putih-abu yang menyerupai
`index.html` di editor mana pun.

| Peran | Di file section (bawaan) | Di `index.html` (token) |
|---|---|---|
| Latar halaman / kartu | `bg-white` | `bg-background` / `bg-card` |
| Teks utama | `text-neutral-900` | `text-foreground` |
| Teks sekunder | `text-neutral-500` | `text-muted-foreground` |
| Garis | `border-neutral-200` | `border-border` |
| Permukaan redup | `bg-neutral-100` | `bg-muted` |
| Tombol utama | `bg-neutral-900 text-white` | `bg-primary text-primary-foreground` |
| Aksen berwarna | padanan bawaan terdekat (`bg-blue-500`) | token brand-nya |
| Ring fokus | `outline-neutral-400` | `outline-ring` |

Peta ini **wajib ditulis di `NOTES.md`** tiap project (§Peta warna), karena dia yang
dipakai saat menempel section ke `index.html`. Nol `var(--…)` di file section.

**Tag akar membawa warnanya sendiri.** `bg-white text-neutral-900` di tag `<section>` —
atau `bg-neutral-100` / `bg-neutral-950` kalau gambar memang menunjukkan section itu
beda. Warna tidak boleh menggantung ke `<body>`.

**Radius pakai skala bawaan Tailwind:** `rounded-lg` (tombol/input), `rounded-xl`
atau `rounded-2xl` (kartu), `rounded-full` (badge/pil). Jangan bikin token radius
kustom seperti `--radius-card` → `rounded-card`; itu satu lapis tak perlu antara
gambar dan kode, dan bikin nilai radius tidak kelihatan saat membaca markup.

Konsekuensi: file section tidak memuat Tailwind sendiri, jadi double-click murni
menampilkan HTML tanpa gaya. Tapi begitu dirender di lingkungan mana pun yang
menyediakan Tailwind (preview editor, playground, halaman lain), tampilannya langsung
benar — karena semua class-nya bawaan, tidak ada yang perlu didefinisikan.

## Kalau user minta tanpa CDN sama sekali

Play CDN memuat satu `<script>` compiler. Untuk benar-benar nol `<script>`:

```bash
npx @tailwindcss/cli -i ./src/input.css -o ./output/<project>/styles.css --minify
```

Lalu ganti tag script dengan `<link rel="stylesheet" href="styles.css">`.
Konsekuensi: `index.html` tidak lagi berdiri sendiri (butuh `styles.css` di sebelahnya).
Tanya user mana yang dipilih; default = Play CDN karena satu file.

## Komponen shadcn-style di HTML murni

shadcn/ui adalah komponen React — tidak bisa di-`import` ke HTML. Yang direplikasi
adalah token + kelas visualnya. Tulis tangan:

```html
<!-- Button: primary / secondary / ghost -->
<a href="#"
   class="inline-flex min-h-11 items-center justify-center gap-2 rounded-lg
          bg-primary px-5 text-sm font-medium text-primary-foreground
          transition-colors hover:bg-primary/90
          focus-visible:ring-3 focus-visible:ring-ring/50">
  Get started
</a>

<button type="button"
   class="inline-flex min-h-11 items-center justify-center gap-2 rounded-lg border
          border-input bg-background px-5 text-sm font-medium
          transition-colors hover:bg-accent hover:text-accent-foreground
          focus-visible:ring-3 focus-visible:ring-ring/50">
  Learn more
</button>

<!-- Card -->
<div class="rounded-2xl border border-border bg-card p-6 lg:p-8">
  <h3 class="text-lg font-semibold tracking-tight">Judul</h3>
  <p class="mt-2 text-sm text-muted-foreground">Deskripsi pendek.</p>
</div>

<!-- Input -->
<label class="block">
  <span class="text-sm font-medium">Email</span>
  <input type="email" required placeholder="nama@email.com"
         class="mt-2 block w-full min-h-11 rounded-lg border border-input bg-background
                px-3 text-sm placeholder:text-muted-foreground
                focus-visible:ring-3 focus-visible:ring-ring/50
                invalid:not-placeholder-shown:border-red-500">
</label>

<!-- Badge -->
<span class="inline-flex items-center rounded-full border border-border bg-muted
             px-2.5 py-0.5 text-xs font-medium text-muted-foreground">Baru</span>
```

## Ikon

Inline `<svg>` saja (Lucide / Heroicons, disalin manual). Tanpa emoji, tanpa icon-font,
tanpa sprite loader.

```html
<svg class="size-5" viewBox="0 0 24 24" fill="none" stroke="currentColor"
     stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
  <path d="M5 12h14M12 5l7 7-7 7"/>
</svg>
```

Ikon dekoratif → `aria-hidden="true"`.
Tombol yang isinya cuma ikon → wajib `aria-label`.
