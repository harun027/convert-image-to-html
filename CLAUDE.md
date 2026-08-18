# Project Rules — Image → Static HTML (Tailwind v4, No JS)

Repo ini punya satu tujuan: **ubah gambar desain jadi HTML statis yang 100% mirip, tanpa JavaScript perilaku.**

Untuk setiap task konversi gambar → web, **WAJIB baca dan ikuti**
`.claude/skills/image-to-html-nojs/SKILL.md` sebelum menulis satu baris HTML.
Berlaku otomatis, walau user tidak menyebut skill-nya.

---

## Hard Rules (tidak bisa dinegosiasi)

1. **ZERO behavioral JavaScript.** Tidak ada `<script>` berisi logika, tidak ada
   event handler inline (`onclick`, `onchange`, …), tidak ada `javascript:` href.
   Semua interaksi pakai CSS/HTML native. Satu-satunya `<script>` yang boleh ada
   adalah tag CDN compiler Tailwind (lihat rule 2).
2. **Tailwind CSS v4 saja.** Mode default: Play CDN satu file —
   `<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>`
   plus `<style type="text/tailwindcss">` berisi `:root` (nilai mentah) + `@theme inline`
   (pemetaan ke utility) — pola shadcn v4. `@theme` polos membekukan nilai dan mematikan
   dark mode; `@theme` di dalam `@media` tidak sah.
   Tidak ada Tailwind v3 syntax (`tailwind.config.js`, `theme.extend`, `@tailwind base`).
3. **shadcn = token + markup, bukan React.** shadcn/ui tidak bisa dipakai di HTML murni.
   Yang direplikasi adalah design token-nya (`--color-background`, `--color-primary`,
   `--radius`, dst.) dan struktur visual komponennya, ditulis tangan sebagai HTML + class Tailwind.
   Jangan pernah `import` atau menulis JSX.
4. **Cocokkan gambar, jangan berimprovisasi.** Warna, ukuran font, radius, shadow,
   jumlah kolom, dan urutan elemen diambil dari gambar. Kalau ada yang tidak
   terlihat di gambar, tulis asumsinya di `NOTES.md`, jangan diam-diam mengarang.
5. **Struktur output selalu sama** (lihat SKILL.md §Output Structure):
   `index.html` full + `sections/<nn>-<nama>/index.html` + `prompts/<nn>-<nama>.md`.
5a. **File section = markup section murni.** `sections/<nn>-<nama>/index.html` mulai
   dari tag akar section dan berakhir di tag penutupnya. Dilarang ada `<!doctype>`,
   `<html>`, `<head>`, `<meta>`, `<title>`, `<body>`, `<script>`, `<style>`, `@theme`,
   atau komentar penanda. Tag akar wajib membawa warnanya sendiri
   (`bg-background text-foreground`, atau `bg-muted`/`bg-card` sesuai gambar).
   Radius pakai skala bawaan Tailwind (`rounded-lg`/`rounded-xl`/`rounded-2xl`/
   `rounded-full`) — dilarang bikin token radius kustom. Prompt di `prompts/` ikut
   bentuk ini persis.
5d. **Warna di file section = palet bawaan Tailwind.** `bg-white`, `text-neutral-900`,
   `text-neutral-500`, `border-neutral-200`, `bg-neutral-100`, `bg-neutral-900`, dan
   padanan bawaan terdekat untuk aksen. Token proyek (`bg-primary`, `bg-background`,
   `bg-brand-*`) dan `var(--…)` **dilarang** di `sections/` — mereka hanya hidup di
   `index.html`. Peta padanan dua sisi wajib ditulis di `NOTES.md` §Peta warna, dan
   dipakai saat menempel section ke `index.html`.
5b. **Hanya section halaman nyata yang dapat folder sendiri.** Navbar, menu, tabs,
   dropdown, accordion, breadcrumb, pagination, sidebar, dan komponen sejenis
   **BUKAN** section — mereka melebur ke dalam file section tempat mereka berada.
   Navbar masuk ke `01-hero/index.html`, tabs harga masuk ke `0n-pricing/index.html`.
   Jangan pernah membuat `sections/01-navbar/`, `sections/xx-tabs/`, atau sejenisnya.
6. **Satu section = satu file prompt `.md`.** Namanya `prompts/<nn>-<nama>.md`,
   sama persis dengan nama folder section-nya.
6b. **File prompt = transkrip prosa dari file section, bukan kerangka HTML dan bukan
   brief desain baru.** Tiap class di prompt wajib ada di `sections/<nn>/index.html`,
   dan sebaliknya — cek balik satu per satu. Dilarang "memperbaiki" nilai: `p-6` tetap
   `p-6`, bukan `p-6 lg:p-8`. Teks literal ditulis dalam tanda kutip.
   **Maksimum 5.000 karakter per file** — seluruh isi file termasuk spasi, tab, dan
   baris baru (batas kotak input tool generator), dihitung bukan dikira, **nol tab / nol indentasi**, dan **disimpan LF** (CRLF menambah satu
   karakter per baris). Wajib memuat blok `SPACING` dengan satu nilai per slot —
   nol rentang seperti "p-6 to p-8". Ikon disebut dengan nama Lucide (`check`, `x`,
   `menu`, `arrow-right`), bukan dideskripsikan bentuknya. Seluruh isi file adalah prompt —
   dilarang ada front-matter, judul markdown, penanda copy, atau blok kode.
   Enam blok, urut: kalimat pembuka · `LAYOUT —` · blok per bagian · blok data ·
   `STYLE REQUIREMENTS:` · `CONSTRAINTS:` (`references/prompt-template.md`).
   Nol kata sifat kualitas: "modern", "rapi", "menarik", "profesional".
6c. **Trigger "buatkan promtingnya".** Kalimat ini dari user = perintah menulis file
   `prompts/<nn>-<nama>.md`. Tanpa nama section → semua section. Dengan nama section
   → satu file itu saja. Detail di SKILL.md Step 3.
7. **Spacing itu deliverable, bukan detail.** Pakai skala 4px. Rhythm section,
   container, dan gap konsisten di semua section. Ini yang dinilai pertama.
7b. **Responsif diukur, bukan diasumsikan.** Tiga breakpoint saja: base / `md:` ≥768
   / `lg:` ≥1024. Wajib diuji di **320 / 375 / 768 / 1024 / 1440px** dan lulus
   checklist 17 poin di `references/responsive.md` §9. Layout yang pecah di 320px
   atau melar tanpa batas di 1440px = gagal, sebagus apa pun tampilannya di 1440.
   Setiap section wajib punya peta runtuh kolom (desktop → 768 → 375) tertulis di `NOTES.md`.
8. **Jangan sentuh backend.** Tidak ada kode server. Data statis di HTML saja.
9. **Jangan `git commit` / `push` / `merge` / `rebase` / `reset --hard`** tanpa izin eksplisit user.

## Red Flags — berhenti dan perbaiki

- Menulis `<script>` untuk buka/tutup menu, tab, modal, atau accordion → pakai
  `<details>`, `peer` + checkbox, atau `:target`.
- Pakai `tailwind.config = {...}` → itu v3. Pakai `:root` + `@theme inline` di CSS.
- Warna hardcode `#hex` acak per komponen → semua warna harus lewat token `:root`.
- Pakai `@theme { … }` polos untuk warna → nilai beku, dark mode mati. Wajib `@theme inline`.
- Section dengan padding beda-beda tanpa alasan → samakan ke skala.
- Bilang "sudah 100% mirip" tanpa membandingkan ulang ke gambar → bandingkan dulu.
- Gambar punya 7 section tapi output cuma 4 → kerjakan semuanya.
- Bikin folder `sections/01-navbar/` atau `sections/xx-tabs/` → salah. Lebur ke section induknya.
- File di `sections/` punya `<head>`, `<body>`, `<script>`, atau `<style>` → itu markup murni. Buang.
- Tag akar section tanpa `bg-*`/`text-*` → warnanya menggantung ke `<body>`. Tambahkan.
- Token proyek atau `var(--…)` dipakai di file `sections/` → tampil tanpa warna di luar `index.html`. Pakai palet bawaan.
- Tempel section ke `index.html` tanpa menerjemahkan warna → halaman penuh jadi abu-abu.
- Bikin token radius kustom (`rounded-card`, `--radius-btn`) → pakai skala bawaan Tailwind.
- Prompt berisi "buat yang rapi dan menarik" → model murah mengabaikannya. Ganti jadi angka/class.
- Prompt menyuruh model mengarang copy atau menyimpulkan warna → tulis teks dalam tanda kutip, warna sebagai nama class.
- File prompt berisi kerangka HTML atau blok kode → itu brief prosa. Markup ditulis generator di seberang.
- File prompt > 5.000 karakter → padatkan butirnya, jangan buang nama class atau teks literal.
- Nilai spacing ditulis sebagai rentang ("p-6 to p-8") → model memilih beda tiap kartu. Satu nilai per slot.
- Nama kelas di prompt pakai placeholder (`peer-checked/X:`) → model pasang semua varian di semua elemen; tab jadi menyala semua. Tulis literal per elemen.
- Ikon dideskripsikan bentuknya → sebut nama Lucide-nya, plus `d="…"` kalau file section memakainya.
- Nilai di file prompt tidak ada di file section → itu improvisasi. Prompt = transkrip, bukan desain baru.
- File prompt punya front-matter/judul/penanda copy, atau baris ber-indentasi → seluruh isinya harus bisa di-paste apa adanya.
- Hanya cek tampilan di 1440px → uji lima lebar, mulai dari 320px.
- Paragraf tanpa `max-w-*` → melar tak terbaca di layar lebar.
- Nav desktop muncul di `md:` → tahan sampai `lg:`, 768px terlalu sempit.
- Tombol tanpa `min-h-11` → di bawah ambang sentuh 44px.

## Layout Direktori

```
.claude/skills/image-to-html-nojs/    # skill + referensi
_template/                            # contoh bentuk output yang benar
output/<nama-project>/                # hasil kerja
```
