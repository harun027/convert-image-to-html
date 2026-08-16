# NOTES — Template

Hasil Step 1 (EXTRACT). Isi file ini **sebelum** menulis HTML.
Nilai di bawah adalah contoh; ganti dengan hasil ukur dari gambar user.

## 1. Palette

| Peran | Token | Nilai (light) | Nilai (dark) |
|---|---|---|---|
| Background | `--color-background` | `oklch(1 0 0)` | `oklch(0.145 0 0)` |
| Teks utama | `--color-foreground` | `oklch(0.145 0 0)` | `oklch(0.985 0 0)` |
| Teks sekunder | `--color-muted-foreground` | `oklch(0.556 0 0)` | `oklch(0.708 0 0)` |
| Permukaan | `--color-muted` | `oklch(0.97 0 0)` | `oklch(0.269 0 0)` |
| Aksen / tombol | `--color-primary` | `oklch(0.205 0 0)` | `oklch(0.985 0 0)` |
| Border | `--color-border` | `oklch(0.922 0 0)` | `oklch(0.269 0 0)` |

## 2. Typography

- Family: Inter (grotesk netral)
- Hero: 36px mobile → 60px desktop, weight 600, `tracking-tight`, leading 1.1
- H2: 30px → 36px, weight 600
- Body: 16px → 18px, leading relaxed
- Caption / eyebrow: 12px, uppercase, `tracking-widest`

## 3. Grid & container

- Container: `max-w-7xl` (1280px), gutter `px-6` → `lg:px-8`
- Breakpoint yang dipakai: base / `md:` ≥768 / `lg:` ≥1024. `sm:` hanya untuk `sm:w-auto`.

### Peta runtuh kolom (wajib per section)

| Section | 320–767px | 768–1023px | ≥1024px | Kelas |
|---|---|---|---|---|
| `01-hero` | 1 kolom | 1 kolom | 6 + 6 | `lg:grid-cols-12` + `lg:col-span-6` |

Titik pindah nav: `lg:` (1024px). Di 768px nav masih hamburger — 768 terlalu
sempit untuk 3 link + 2 tombol tanpa berdesakan.

## 4. Spacing rhythm

| Slot | base | `md:` | `lg:` |
|---|---|---|---|
| Padding section | `py-16` | `py-24` | `py-32` |
| Gap antar kolom | `gap-10` | `gap-12` | `gap-16` |
| Padding tepi | `px-6` | `px-6` | `px-8` |

- Heading → body: `mt-6`
- Body → tombol: `mt-8`
- Tombol → social proof: `mt-12`
- Gap antar tombol: `gap-3` + `flex-wrap`
- CTA di mobile: `w-full sm:w-auto`
- Batas panjang baris: paragraf hero `max-w-lg`, paragraf section `max-w-2xl`

## 5. Radius & border

- Card: `0.75rem` · Tombol/input: `0.5rem` · Pill: full
- Border: 1px, warna `--color-border`

## 6. Shadow & depth

Flat + border. Shadow hanya pada dropdown/modal (`shadow-lg`).

## 7. Daftar section

Hanya pita horizontal halaman yang dihitung section. Komponen (navbar, tabs,
dropdown, accordion) melebur ke file section induknya — tidak dapat folder sendiri.

| # | Slug | Komponen yang melebur | Interaksi | Pola CSS-only |
|---|---|---|---|---|
| 01 | `hero` | navbar, menu mobile | buka/tutup menu | `<details>` |

## Asumsi (tidak terlihat di gambar)

- Hover state tombol → `hover:opacity-90` / `hover:bg-muted`.
- Dark mode mengikuti OS; tidak ada tombol toggle di gambar.
- Isi visual hero diganti placeholder `aspect-[4/3] bg-muted`.
