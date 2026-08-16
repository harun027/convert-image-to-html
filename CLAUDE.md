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
   plus `<style type="text/tailwindcss">` untuk `@theme`.
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
5b. **Hanya section halaman nyata yang dapat folder sendiri.** Navbar, menu, tabs,
   dropdown, accordion, breadcrumb, pagination, sidebar, dan komponen sejenis
   **BUKAN** section — mereka melebur ke dalam file section tempat mereka berada.
   Navbar masuk ke `01-hero/index.html`, tabs harga masuk ke `0n-pricing/index.html`.
   Jangan pernah membuat `sections/01-navbar/`, `sections/xx-tabs/`, atau sejenisnya.
6. **Format file prompt = `.md`**, bukan `.txt`. Alasannya: code fence menjaga
   snippet HTML tetap utuh, ada front-matter untuk metadata section, dan langsung
   ter-render rapi di editor/GitHub.
6b. **Prompt ditulis untuk model termurah.** Target: Gemini Flash kelas menghasilkan
   output identik dengan model mahal. Nol penalaran boleh dibebankan ke model —
   kerangka HTML sudah jadi dengan tanda `<!-- ISI:n -->`, class ditulis literal,
   teks ada di tabel COPY, class dibatasi whitelist, ditutup checklist verifikasi.
   Struktur 7 blok wajib (`references/prompt-template.md`). Nol kata sifat kualitas:
   "modern", "rapi", "menarik", "profesional" dilarang muncul di file prompt.
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
- Pakai `tailwind.config = {...}` → itu v3. Pakai `@theme` di CSS.
- Warna hardcode `#hex` acak per komponen → semua warna harus lewat token `@theme`.
- Section dengan padding beda-beda tanpa alasan → samakan ke skala.
- Bilang "sudah 100% mirip" tanpa membandingkan ulang ke gambar → bandingkan dulu.
- Gambar punya 7 section tapi output cuma 4 → kerjakan semuanya.
- Bikin folder `sections/01-navbar/` atau `sections/xx-tabs/` → salah. Lebur ke section induknya.
- Prompt berisi "buat yang rapi dan menarik" → model murah mengabaikannya. Ganti jadi angka/class.
- Prompt menyuruh model mengarang copy atau menyimpulkan class → tulis literal di tabel COPY / kerangka.
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
