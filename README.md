# Image → Static HTML (Tailwind v4, No JS)

Kirim gambar desain, dapat website statis yang mirip 100% — tanpa JavaScript.
Hasilnya tiga bentuk sekaligus: satu halaman penuh, file terpisah per section,
dan prompt siap copy-paste per section.

---

## 1. Isi Repo

| Path | Fungsi |
|---|---|
| `CLAUDE.md` | **Ruler.** Aturan keras yang dibaca Claude Code otomatis setiap sesi. |
| `.claude/skills/image-to-html-nojs/SKILL.md` | **Skill.** Workflow 5 langkah + checklist verifikasi. |
| `.claude/skills/.../references/setup.md` | Boilerplate `<head>`, token `:root` + `@theme inline`, komponen shadcn-style. |
| `.claude/skills/.../references/no-js-patterns.md` | Resep interaksi CSS-only siap copy (accordion, tabs, modal, dst). |
| `.claude/skills/.../references/responsive.md` | Kontrak responsif: 5 lebar uji, peta runtuh grid, skala tipografi, checklist 17 poin. |
| `.claude/skills/.../references/prompt-template.md` | Cara menulis prompt yang tahan model murah (struktur 5 blok). |
| `_template/` | Contoh output jadi. Jangan diedit — dipakai sebagai acuan bentuk. |
| `output/<nama-project>/` | Hasil kerja kamu. Dibuat otomatis. |

---

## 2. Cara Pakai — Alur Utama

### Langkah 1 · Buka Claude Code di folder ini

```bash
cd c:\Users\Lenovo\Downloads\convert-image-to-html
claude
```

`CLAUDE.md` terbaca otomatis. Tidak perlu ketik apa-apa untuk mengaktifkannya.

### Langkah 2 · Kirim gambar + satu kalimat

Drag gambar ke terminal, lalu tulis:

```
convert gambar ini jadi web, nama project: toko-baju
```

Itu saja. Skill jalan sendiri karena `CLAUDE.md` sudah mewajibkannya.

Kalau kirim beberapa gambar, sebutkan mana yang utama:

```
ini desktop, ini mobile. desktop yang jadi acuan. nama project: toko-baju
```

### Langkah 3 · Tunggu, lalu cek hasilnya

```
output/toko-baju/
├── index.html                    ← buka ini di browser, halaman penuh
├── NOTES.md                      ← token hasil ukur + daftar asumsi
├── sections/                     ← POTONGAN kode, bukan halaman
│   ├── 01-hero/index.html        ← token warna + <header> + <section>, navbar menyatu
│   ├── 02-features/index.html    ← token warna + <section> … </section>
│   └── 03-pricing/index.html
└── prompts/
    ├── 01-hero.md                ← prompt siap paste ke AI lain
    ├── 02-features.md
    └── 03-pricing.md
```

Buka `index.html` — langsung jalan, tidak perlu server, tidak perlu `npm install`.

**File di `sections/` isinya markup section murni.** Tidak ada `<head>`, `<body>`,
`<meta>`, `<script>`, atau `<style>` — baris pertama langsung tag section-nya:

```html
<section class="bg-white text-neutral-900 py-16 lg:py-24">
  <div class="mx-auto max-w-6xl px-4 sm:px-6 lg:px-8">
    <h2 class="text-3xl lg:text-5xl font-bold tracking-tight">Simple, transparent pricing</h2>
    <div class="rounded-2xl border border-neutral-200 bg-white p-6 lg:p-8">…</div>
  </div>
</section>
```

**Warnanya sengaja palet bawaan Tailwind** — `bg-white`, `text-neutral-500`,
`border-neutral-200`. Bukan token proyek seperti `bg-primary`. Alasannya: class bawaan
selalu ada di Tailwind mana pun, jadi file section tampil hitam-putih-abu yang
menyerupai `index.html` di preview editor, playground, atau halaman lain — tanpa satu
baris CSS di dalamnya.

**Warna penuh ada di `index.html`**, disimpan sekali di `<head>` dengan pola `:root` +
`@theme inline`. Padanan dua sisi (`bg-white` ↔ `bg-background`, `bg-neutral-900` ↔
`bg-primary`, …) ditulis di `NOTES.md` §Peta warna — itu yang dipakai saat menempel
section ke `index.html`.

---

## 3. Minta Prompting-nya

Cukup ketik kalimat ini ke Claude Code:

```
buatkan promtingnya
```

Itu perintah resminya. Claude akan menulis satu file `.md` per section ke
`prompts/`, format 5 blok, siap copy-paste ke AI lain.

| Yang kamu ketik | Yang dibuat |
|---|---|
| `buatkan promtingnya` | Semua section yang ada di `sections/` |
| `buatkan promtingnya untuk 03-pricing` | Satu file itu saja |
| `buatkan promtingnya` (baru punya gambar, belum ada section) | Claude ukur token dari gambar dulu, baru tulis prompt-nya |

---

## 4. Cara Copas Hasil Prompting-nya

Ini bagian yang bikin kamu tidak tergantung Claude terus. Lima langkah.

### Langkah 1 · Buka file prompt-nya

```
output/toko-baju/prompts/02-features.md
```

Buka di VS Code, Notepad, atau langsung di GitHub — semua sama saja, isinya teks biasa.

### Langkah 2 · Blok teks di antara dua penanda

Di dalam file ada dua baris penanda:

```
## ▼ COPY MULAI          ← mulai blok dari BAWAH baris ini
   … isi prompt …
## ▲ COPY SELESAI        ← berhenti di ATAS baris ini
```

Copy semua yang ada di antaranya. **Jangan** ikut mengcopy front-matter
(`---` … `---` di paling atas) dan judul `# Prompt — Section 02: …` —
itu catatan untuk kamu, bukan untuk AI.

> Cara cepat di VS Code: klik di awal baris setelah `▼ COPY MULAI`,
> scroll ke `▲ COPY SELESAI`, lalu `Shift + Klik` di akhir baris sebelumnya → `Ctrl + C`.

### Langkah 3 · Paste ke AI mana pun + lampirkan gambar

Buka Gemini / ChatGPT / DeepSeek / Claude — bebas, yang gratis pun cukup.

1. Lampirkan gambar section itu (crop dari desain aslinya, misal `refs/02-features.png`).
2. Paste prompt yang tadi di-copy.
3. Kirim.

Model gratis kelas Gemini Flash sudah cukup. Prompt-nya memang dirancang untuk itu.

### Langkah 4 · Ambil kodenya

Jawaban AI = satu blok kode berisi `<section>` sampai `</section>`, tidak lebih.
Klik tombol copy di pojok blok kode itu.

Dua hal yang sering nyelip dan harus dibuang:

- Penjelasan di luar blok kode ("Berikut adalah kode HTML-nya…") — ambil isi blok kode-nya saja.
- Pembungkus halaman (`<!doctype html>`, `<html>`, `<head>`, `<body>`, `<style>`) — AI
  melanggar Blok 1. Balas: `"markup section saja, mulai <section, tanpa head/body/style"`

### Langkah 5 · Timpa file section-nya

Paste ke:

```
output/toko-baju/sections/02-features/index.html
```

File ini markup murni (tanpa `<script>` Tailwind), jadi double-click langsung
menampilkan HTML polos. Preview-nya lewat preview editor / playground yang sudah
menyediakan Tailwind — di sana tampilannya benar, hitam-putih-abu.

Untuk versi berwarna penuh, tempel ke `index.html` di posisi section itu **sambil
menerjemahkan warnanya** lewat `NOTES.md` §Peta warna. Atau minta Claude:
`"gabungkan ulang semua section ke index.html"` — dia yang menerjemahkan.

### Kalau hasilnya meleset

| Gejala | Sebabnya | Perbaikannya |
|---|---|---|
| Ada `<!doctype html>` / `<head>` / `<style>` di output | AI melanggar Blok 1 larangan 1 | Balas: `"markup section saja, tanpa head/body/style"` |
| Section tanpa warna saat dibuka sendiri | Normal — token ada di `index.html` | Bukan bug. Preview lewat `index.html`. |
| Warna section beda dari yang lain | AI mengarang class warna | Balas: `"pakai persis class di kerangka Blok 2"` |
| Muncul `<script>` toggle menu | AI melanggar Blok 1 | Balas: `"nol JavaScript. Ulangi, pakai <details>"` |
| Masih ada `<!-- ISI:3 -->` di output | AI berhenti di tengah | Balas: `"lanjutkan, ganti semua tanda ISI yang tersisa"` |
| Layout ditambah-tambahi sendiri | AI mengubah kerangka | Balas: `"kembalikan persis ke kerangka Blok 2, jangan tambah elemen"` |
| Berulang kali jelek | Prompt-nya masih menyisakan celah tebakan | Kirim output jeleknya ke Claude → titik yang bikin AI menebak akan ditambal jadi nilai literal |

### Kenapa model murah bisa sebagus model mahal?

Prompt-nya tidak menyuruh model berpikir sama sekali:

- Kerangka HTML sudah jadi — model cuma mengganti tanda `<!-- ISI:n -->`.
- Semua teks tampilan ada di tabel COPY, tidak ada yang dikarang.
- Class ditulis literal di kerangka, bukan disimpulkan.
- Ditutup 6 poin cek yang bisa diverifikasi model sendiri.

Nol tempat untuk menebak = nol tempat untuk salah.

**Formatnya sengaja pendek.** Prompt panjang bukan prompt kuat — bagian tengah
prompt panjang justru dilewati model murah. Yang bekerja cuma kerangka dan tabel
COPY, jadi sisanya dipangkas: 5 blok, tidak lebih.

Contoh jadi: [`_template/prompts/01-hero.md`](_template/prompts/01-hero.md)

---

## 5. Aturan yang Berlaku Otomatis

Tidak perlu kamu sebut. Sudah tertanam di `CLAUDE.md`.

| # | Aturan |
|---|---|
| 1 | **Nol JavaScript perilaku.** Interaksi pakai `<details>`, `peer` + checkbox, `:target`, `scroll-snap`. |
| 2 | **Tailwind v4 saja.** Play CDN + `:root` & `@theme inline` di `index.html`. Radius pakai skala bawaan. |
| 3 | **shadcn = token + markup tulis tangan.** Bukan React, bukan `import`. |
| 4 | **Cocokkan gambar, jangan berimprovisasi.** Yang tidak terlihat → dicatat di `NOTES.md`. |
| 5 | **Struktur output selalu sama.** Full + per-section + prompts. |
| 5b | **Navbar/menu/tabs bukan section.** Melebur ke file section induknya. |
| 5c | **File section = markup murni + palet bawaan.** Tanpa `<head>`/`<body>`/`<script>`/`<style>`, tanpa token proyek. Warna penuh hanya di `index.html`. |
| 6 | **File prompt `.md`**, bukan `.txt`. |
| 6b | **Prompt ditulis untuk model termurah.** Struktur 5 blok, pendek, nol kata sifat kualitas. |
| 7 | **Spacing skala 4px, konsisten lintas section.** |
| 7b | **Responsif diuji di 320 / 375 / 768 / 1024 / 1440px**, lulus checklist 17 poin. |
| 8 | **Tidak menyentuh backend.** |
| 9 | **Tidak `git commit`/`push`/`reset --hard`** tanpa izin. |

---

## 6. Yang Bisa & Tidak Bisa Tanpa JS

| Bisa | Polanya |
|---|---|
| Accordion / FAQ | `<details><summary>` |
| Menu mobile | `<details>` |
| Tabs | radio + `peer-checked:` |
| Modal | `:target` + anchor `#id` |
| Carousel | `overflow-x-auto` + `snap-x snap-mandatory` |
| Dropdown | `<details>` atau `<select>` native |
| Dark mode | `prefers-color-scheme` (ikut OS) |
| Validasi form | `:invalid` + `:placeholder-shown` |

| Tidak bisa | Alternatif yang dipakai |
|---|---|
| Auto-play carousel | Scroll-snap manual |
| Filter / search live | Daftar statis atau `<form>` GET |
| Toast muncul sendiri | Alert banner statis |
| Tema tersimpan setelah reload | Ikut OS |
| Focus trap di modal | Modal `:target` biasa |
| Chart dinamis | SVG statis |

Detail lengkap: [`no-js-patterns.md`](.claude/skills/image-to-html-nojs/references/no-js-patterns.md)

---

## 7. Catatan Penting

**Ada satu `<script>` di setiap file.** Itu tag CDN compiler Tailwind, bukan logika
aplikasi, dan hanya ada di `index.html`. File di `sections/` nol `<script>`.
Kalau kamu mau benar-benar nol tag `<script>`:

```bash
npx @tailwindcss/cli -i ./src/input.css -o ./output/<project>/styles.css --minify
```

lalu ganti tag script dengan `<link rel="stylesheet" href="styles.css">`.
Konsekuensinya `index.html` tidak lagi satu file berdiri sendiri.
Panduan: [`setup.md`](.claude/skills/image-to-html-nojs/references/setup.md)

**shadcn/ui tidak bisa di-import ke HTML murni** — itu library React. Yang
direplikasi adalah design token-nya (`--color-primary`, `--radius`, dst.) dan
struktur visual komponennya, ditulis tangan sebagai HTML + class Tailwind.

---

## 8. Kalau Hasilnya Belum Pas

| Masalah | Yang dilakukan |
|---|---|
| Warna meleset | Buka `NOTES.md`, perbaiki nilai di tabel Palette, minta Claude regenerate. |
| Jarak terasa sempit/longgar | Sebutkan section-nya: `"section pricing kurang lega, naikkan jarak vertikalnya"`. |
| Berantakan di HP | `"cek ulang di 320 dan 375px"` — checklist responsif 17 poin akan menangkapnya. |
| Teks melar di layar lebar | `"paragraf belum dibatasi max-w"` — biasanya kurang `max-w-2xl`. |
| Satu section salah | Regenerate satu itu saja pakai `prompts/<nn>-<nama>.md`. Section lain tidak tersentuh. |
| Ada JS nyelip | `"cek ulang, hilangkan semua JS"` — checklist skill akan menangkapnya. |
| Ada section terlewat | `"section <nama> belum ada di output"`. |
| Model murah keluar hasil jelek | Kirim outputnya ke Claude; titik yang masih butuh tebakan akan ditambal jadi nilai literal. |

---

## 9. Perintah Cepat

```
convert gambar ini jadi web, nama project: <nama>     # bikin dari nol
buatkan promtingnya                                    # tulis semua file prompts/
buatkan promtingnya untuk 03-pricing                   # satu file prompt saja
regenerate section 03-pricing                          # ulang satu section
tambahkan section testimonials di antara 03 dan 04     # sisipkan section
cek ulang semua section, pastikan nol JS               # audit
samakan spacing semua section                          # rapikan ritme
gabungkan ulang semua section ke index.html            # rakit halaman penuh
```
