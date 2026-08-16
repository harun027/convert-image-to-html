# Setup — Tailwind v4 tanpa build, token shadcn-style

## Boilerplate `<head>`

Blok ini **identik** di `index.html` dan di setiap `sections/*/index.html`.
Ganti nilai `@theme` dengan hasil ukur dari gambar.

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
    @theme {
      /* --- warna: hasil sampel dari gambar --- */
      --color-background:        oklch(1 0 0);
      --color-foreground:        oklch(0.145 0 0);
      --color-card:              oklch(1 0 0);
      --color-card-foreground:   oklch(0.145 0 0);
      --color-muted:             oklch(0.97 0 0);
      --color-muted-foreground:  oklch(0.556 0 0);
      --color-primary:           oklch(0.205 0 0);
      --color-primary-foreground:oklch(0.985 0 0);
      --color-accent:            oklch(0.97 0 0);
      --color-border:            oklch(0.922 0 0);
      --color-ring:              oklch(0.708 0 0);

      /* --- bentuk --- */
      --radius-card: 0.75rem;
      --radius-btn:  0.5rem;

      /* --- tipografi --- */
      --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;
    }

    /* Dark mode otomatis mengikuti OS. Tanpa JS tidak ada tombol toggle;
       kalau butuh toggle manual, lihat no-js-patterns.md §Dark mode toggle. */
    @media (prefers-color-scheme: dark) {
      @theme {
        --color-background:       oklch(0.145 0 0);
        --color-foreground:       oklch(0.985 0 0);
        --color-card:             oklch(0.205 0 0);
        --color-muted:            oklch(0.269 0 0);
        --color-muted-foreground: oklch(0.708 0 0);
        --color-primary:          oklch(0.985 0 0);
        --color-primary-foreground: oklch(0.205 0 0);
        --color-border:           oklch(0.269 0 0);
      }
    }
  </style>
</head>
<body class="bg-background text-foreground font-sans antialiased">
  <!-- section di sini -->
</body>
</html>
```

Token `@theme` otomatis jadi utility: `--color-primary` → `bg-primary`, `text-primary`,
`border-primary`. `--radius-card` → `rounded-card`.

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
   class="inline-flex min-h-11 items-center justify-center gap-2 rounded-btn
          bg-primary px-5 text-sm font-medium text-primary-foreground
          transition-colors hover:opacity-90
          focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring">
  Get started
</a>

<button type="button"
   class="inline-flex min-h-11 items-center justify-center gap-2 rounded-btn border
          border-border bg-background px-5 text-sm font-medium
          transition-colors hover:bg-muted
          focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring">
  Learn more
</button>

<!-- Card -->
<div class="rounded-card border border-border bg-card p-6 lg:p-8">
  <h3 class="text-lg font-semibold tracking-tight">Judul</h3>
  <p class="mt-2 text-sm text-muted-foreground">Deskripsi pendek.</p>
</div>

<!-- Input -->
<label class="block">
  <span class="text-sm font-medium">Email</span>
  <input type="email" required placeholder="nama@email.com"
         class="mt-2 block w-full min-h-11 rounded-btn border border-border bg-background
                px-3 text-sm placeholder:text-muted-foreground
                focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-ring
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
