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
| `.claude/skills/.../references/setup.md` | Boilerplate `<head>`, blok `@theme`, komponen shadcn-style. |
| `.claude/skills/.../references/no-js-patterns.md` | Resep interaksi CSS-only siap copy (accordion, tabs, modal, dst). |
| `.claude/skills/.../references/responsive.md` | Kontrak responsif: 5 lebar uji, peta runtuh grid, skala tipografi, checklist 17 poin. |
| `.claude/skills/.../references/prompt-template.md` | Cara menulis prompt yang tahan model murah (struktur 8 blok). |
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
├── sections/
│   ├── 01-hero/index.html        ← navbar + hero, bisa dibuka sendiri
│   ├── 02-features/index.html
│   └── 03-pricing/index.html
└── prompts/
    ├── 01-hero.md                ← prompt siap paste ke AI lain
    ├── 02-features.md
    └── 03-pricing.md
```

Buka `index.html` — langsung jalan, tidak perlu server, tidak perlu `npm install`.

---

## 3. Cara Pakai Folder `prompts/`

Ini bagian yang bikin kamu tidak tergantung Claude terus.

Setiap file `.md` di `prompts/` berisi satu blok copy-paste yang sudah dirancang
supaya **model AI termurah pun keluar hasil yang sama dengan model mahal**.

**Alurnya:**

1. Buka misalnya `prompts/02-features.md`.
2. Copy semua isi antara `▼ COPY-PASTE MULAI DARI SINI` dan `▲ COPY-PASTE SAMPAI SINI`.
3. Paste ke Gemini Flash / ChatGPT / AI apa pun, lampirkan gambar section itu.
4. Hasilnya = file HTML lengkap yang langsung nyambung dengan section lain
   (token `@theme`-nya sama persis).
5. Simpan ke `sections/02-features/index.html`, timpa yang lama.

**Kenapa bisa sama dengan model mahal?** Prompt-nya tidak menyuruh model berpikir.
Kerangka HTML sudah jadi, model tinggal mengisi tanda `<!-- ISI:n -->`; semua teks
ada di tabel COPY; class dibatasi whitelist; ditutup checklist yang model cek sendiri.
Nol tempat untuk menebak = nol tempat untuk salah.

Contoh jadi: [`_template/prompts/01-hero.md`](_template/prompts/01-hero.md)

---

## 4. Aturan yang Berlaku Otomatis

Tidak perlu kamu sebut. Sudah tertanam di `CLAUDE.md`.

| # | Aturan |
|---|---|
| 1 | **Nol JavaScript perilaku.** Interaksi pakai `<details>`, `peer` + checkbox, `:target`, `scroll-snap`. |
| 2 | **Tailwind v4 saja.** Play CDN + `@theme`. Tanpa `tailwind.config.js`. |
| 3 | **shadcn = token + markup tulis tangan.** Bukan React, bukan `import`. |
| 4 | **Cocokkan gambar, jangan berimprovisasi.** Yang tidak terlihat → dicatat di `NOTES.md`. |
| 5 | **Struktur output selalu sama.** Full + per-section + prompts. |
| 5b | **Navbar/menu/tabs bukan section.** Melebur ke file section induknya. |
| 6 | **File prompt `.md`**, bukan `.txt`. |
| 6b | **Prompt ditulis untuk model termurah.** Struktur 7 blok, nol kata sifat kualitas. |
| 7 | **Spacing skala 4px, konsisten lintas section.** |
| 7b | **Responsif diuji di 320 / 375 / 768 / 1024 / 1440px**, lulus checklist 17 poin. |
| 8 | **Tidak menyentuh backend.** |
| 9 | **Tidak `git commit`/`push`/`reset --hard`** tanpa izin. |

---

## 5. Yang Bisa & Tidak Bisa Tanpa JS

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

## 6. Catatan Penting

**Ada satu `<script>` di setiap file.** Itu tag CDN compiler Tailwind, bukan logika
aplikasi. Kalau kamu mau benar-benar nol tag `<script>`:

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

## 7. Kalau Hasilnya Belum Pas

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

## 8. Perintah Cepat

```
convert gambar ini jadi web, nama project: <nama>     # bikin dari nol
regenerate section 03-pricing                          # ulang satu section
tambahkan section testimonials di antara 03 dan 04     # sisipkan section
cek ulang semua section, pastikan nol JS               # audit
samakan spacing semua section                          # rapikan ritme
```
