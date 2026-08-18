# Menulis File Prompt

File prompt itu **brief desain berbentuk prosa**, bukan kerangka HTML. Isinya
menjelaskan *apa yang harus dibangun* dengan nama class Tailwind yang konkret;
generator di seberang yang menulis markup-nya.

**Satu section = satu file = seluruh isinya adalah prompt.** Tidak ada front-matter,
tidak ada judul markdown, tidak ada penanda `COPY MULAI`. User blok seluruh file,
salin, tempel. Kalau ada sesuatu di file yang tidak boleh ikut ter-paste, formatnya salah.

## Dua aturan keras

1. **Maksimum 5.000 karakter per file.** Dihitung dari karakter pertama sampai
   terakhir, **termasuk spasi, tab, dan setiap baris baru** — persis seperti hitungan
   kotak input tool generator markup (`5000/5000` di pojoknya). Lewat satu karakter
   pun, ujungnya terpotong.
2. **Nol tab, nol indentasi.** Setiap baris mulai di kolom 1 (butir memakai `- `).
   Indentasi memakan jatah karakter tanpa menambah kejelasan.

Menghitungnya:

```powershell
$s=[IO.File]::ReadAllText("prompts\01-hero.md",[Text.Encoding]::UTF8)
"$($s.Length) karakter " + $(if($s.Length -le 5000){"- AMAN"}else{"- LEWAT $($s.Length-5000)"})
```

Hitung **karakter**, bukan byte. `wc -c` menghitung byte dan tanda seperti `—` memakai
3 byte, jadi hasilnya meleset ke atas.

**Simpan file dengan akhiran baris LF, bukan CRLF.** Di Windows, CRLF menambah satu
karakter per baris — file 70 baris jadi 70 karakter lebih gemuk tanpa satu kata pun
bertambah. Itu cukup untuk menendang file yang tadinya pas jadi lewat batas.

## Kerangka file — enam blok, urutannya mengikat

| Blok | Isi |
|---|---|
| Kalimat pembuka | Satu kalimat: bangun section apa, dengan apa, palet apa |
| `LAYOUT —` | Satu kalimat: susunan besarnya dari atas ke bawah |
| Blok per bagian | `SECTION CONTAINER:`, `HEADER BLOCK:`, `PRICING CARDS:`, … |
| Blok data | Daftar isi yang berulang: nama kartu, harga, teks fitur |
| `STYLE REQUIREMENTS:` | Palet, radius, ukuran sentuh, aturan responsif |
| `CONSTRAINTS:` | Yang dilarang. Selalu paling akhir |

Judul blok ditulis **KAPITAL semua** tanpa `#` markdown. Itu yang membuat strukturnya
terbaca oleh model tanpa memakan karakter untuk sintaks.

## Cara menulis butirnya

Setiap butir menyebut **elemennya, teksnya, lalu class-nya dalam kurung**:

```
- Small pill badge reading "Pricing" — inline-flex, rounded-full, border border-neutral-200, bg-white, px-3 py-1, text-xs font-medium, text-neutral-500
```

| Salah | Benar |
|---|---|
| "judul besar dan tebal" | `text-4xl, md:text-5xl, lg:text-6xl, font-bold, tracking-tight` |
| "spacing yang nyaman" | `mt-6` untuk paragraf, `mt-8` untuk tombol |
| "warna sesuai brand" | `bg-neutral-900, text-white` |
| "tulis copy yang cocok" | teks literal di dalam tanda kutip |
| "buat rapi dan menarik" | (hapus — tidak bisa dicek, jadi diabaikan) |

**Aturan emas: kalau sebuah instruksi tidak bisa dicek benar/salah secara mekanis,
model akan mengabaikannya. Buang, atau ubah jadi nama class.**

Teks yang tampil di layar **selalu ditulis literal di dalam tanda kutip**. Jangan
pernah menyuruh model mengarang copy — hasilnya filler yang buruk dan berbeda tiap
kali di-generate ulang.

## Elemen berulang

Tulis strukturnya sekali di blok bagian, lalu datanya sebagai daftar bernomor:

```
THE FOUR CARDS, left to right:
- Starter — price "Free", no unit label, note "Forever", button "Join now" styled bg-neutral-100 text-neutral-900
- Pro — "$12" with unit "/mo", note "Per user", button "Start for free" styled bg-blue-500 text-white

FEATURES — every card lists exactly five rows, in this order:
1. Full library access
2. the asset quota: Starter "5 assets / mo", Pro "20 assets / mo"
3. Regular updates
All rows use a checkmark icon, except row 4 on the Starter card only: use a cross icon and mute the whole row with text-neutral-500.
```

Jumlahnya selalu eksplisit ("exactly five rows", "left to right"), tidak pernah
"beberapa" atau "sesuai gambar". Pengecualian ditulis sebagai satu kalimat di akhir
daftar, bukan disebar per baris.

## CONSTRAINTS — selalu lima baris ini

Blok penutup wajib, karena ini yang paling diingat model (recency):

```
CONSTRAINTS:
- Output the section markup only — no doctype, html, head, meta, title, or body tags
- No custom CSS, no style attributes, no style blocks
- No JavaScript at all — <interaksi section ini> must work with <pola CSS-only-nya>
- Icons are inline SVG only — no emoji, no icon fonts
- Use only Tailwind CSS class names for all styling, layout, and responsiveness
```

Baris ketiga diisi sesuai section: menu mobile → `details and summary`, toggle harga →
`radio inputs and peer variants`, FAQ → `details and summary`, modal → `the :target
pseudo-class`. Larangan telanjang "jangan pakai JS" sering dilanggar; menyebut
penggantinya tidak.

## Aturan nomor satu: prompt diturunkan dari file, bukan dari selera

File prompt adalah **transkrip** `sections/<nn>-<nama>/index.html`, bukan brief desain
baru. Buka file section-nya, baca class-nya satu per satu, tulis ulang jadi kalimat.
Kalau file menulis `p-6`, prompt menulis `p-6` — bukan `p-6, lg:p-8` karena "lebih
lega", bukan `p-6 to p-8` karena kedengarannya fleksibel.

Ini kesalahan yang paling mahal, karena hasilnya tetap terlihat "masuk akal" sehingga
tidak ketahuan sampai di-render berdampingan. Contoh nyata dari repo ini — satu prompt
menyimpang di 14 titik sekaligus, semuanya hasil improvisasi:

| Di file section | Ditulis di prompt | Akibatnya |
|---|---|---|
| toggle aktif `bg-neutral-900 text-white` | `bg-white text-neutral-900 shadow-sm` | desainnya terbalik |
| `p-6` | `p-6, lg:p-8` | kartu melebar di desktop, tidak lagi cocok |
| `gap-6` | `gap-6, lg:gap-8` | grid renggang sendiri |
| `text-3xl` harga | `text-4xl` | hierarki berubah |
| `mt-4` nama→harga | `mt-5` | ritme kartu meleset |
| `items-start` + ikon `mt-0.5` | `items-center` | ikon tidak sejajar baris pertama |
| SVG Mastercard | dihilangkan | elemen hilang |
| `focus-visible:outline-*` | tidak disebut | ring fokus hilang |

**Prosedurnya:** salin daftar class tiap elemen dari file section ke dalam kalimat
prompt, lalu **cek balik satu per satu** — tiap nilai di prompt harus benar-benar ada
di file. Nilai yang tidak ada di file berarti kamu mengarang; hapus.

Kalimat pembuka prompt wajib mengunci ini:

```
Every class below is exact: use it verbatim, never substitute, never add one not listed.
```

Dan `STYLE REQUIREMENTS` wajib memuat:

```
- Do not add md: or lg: variants that are not listed, and never swap a value for a similar one
```

### Kalau class-nya berulang, beri alias sekali

Rangkaian panjang yang muncul di banyak elemen ditulis sekali di atas, lalu dirujuk:

```
FOCUS RING = focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-neutral-400
```

Lalu cukup tulis "plus the FOCUS RING". Hemat ratusan karakter tanpa kehilangan satu
class pun — dan itu yang membuat transkrip selengkap ini tetap muat di 5.000.

## Tiga hal yang paling sering bikin hasilnya berantakan

Model generator boleh mengarang di titik yang tidak kamu kunci. Tiga titik ini yang
paling sering lolos dan hasilnya langsung terlihat tidak rapi.

### 1. Spacing — kunci angkanya, jangan diserahkan

"Adequate padding" dan "consistent spacing" tidak bisa dicek, jadi diabaikan. Tulis
ritmenya sebagai angka, dan pakai **skala 4px yang sama di semua section**:

```
SPACING (use exactly these, do not improvise):
- Section padding: py-16, md:py-24, lg:py-32
- Container: mx-auto, max-w-7xl, px-6, lg:px-8
- Heading to paragraph: mt-6. Paragraph to buttons: mt-8. Block to next block: mt-12, md:mt-16
- Card padding: p-6, lg:p-8. Gap between cards: gap-6, lg:gap-8
- Inside a card: title to price mt-4, price to note mt-1, note to feature list mt-8, list to button mt-8
- Gap between feature rows: gap-3. Gap between icon and its label: gap-3
```

Aturannya: **satu nilai per slot, tidak ada rentang.** "p-6 to p-8" memaksa model
memilih, dan pilihannya berbeda tiap kartu — itu sumber ketidakrapian yang paling umum.
Kalau memang berubah di breakpoint, tulis `p-6, lg:p-8`, bukan "p-6 to p-8".

Satu baris penutup yang wajib ada: *"All cards keep identical internal spacing so the
feature lists line up across cards."* Tanpa itu tiap kartu dapat ritme sendiri.

### 2. Kontrol tersegmen (tabs, toggle) — jangan pernah pakai placeholder

Bug paling sering: **kedua segmen menyala bersamaan**, jadi tidak kelihatan mana yang
aktif. Penyebabnya hampir selalu satu: prompt menulis kelasnya dengan placeholder yang
harus disubstitusi model.

```
SALAH — model harus menyimpulkan X untuk tiap label:
- Plus, X being monthly or yearly: peer-checked/X:bg-neutral-900 peer-checked/X:text-white
```

Model memasang **kedua** varian di **kedua** label. Akibatnya radio mana pun yang
aktif, dua-duanya menghitam. Tulis per label, literal:

```
BENAR:
- Both labels: inline-flex min-h-11 cursor-pointer items-center rounded-full px-5 text-sm font-medium text-neutral-500 transition-colors
- Label 1, for billing-monthly, "Monthly", adds ONLY: peer-checked/monthly:bg-neutral-900 peer-checked/monthly:text-white
- Label 2, for billing-yearly, "Yearly (30% Off)", adds ONLY: peer-checked/yearly:bg-neutral-900 peer-checked/yearly:text-white
- Never put a yearly variant on label 1 or a monthly variant on label 2: a label carrying both makes both segments dark at once
```

Empat hal yang mengikat:

1. **Nol placeholder di nama kelas.** Tidak ada `X`, `n`, atau `<nama>` yang harus
   diganti model. Setiap varian ditulis utuh, per elemen. Ini berlaku untuk semua
   kelas, bukan cuma toggle — placeholder adalah penalaran, dan penalaran adalah bug.
2. **Kata `ONLY`** di tiap baris label, plus satu baris larangan silang yang menyebut
   akibatnya. Larangan yang menyebut gejalanya jauh lebih dipatuhi.
3. **Kedua segmen berbagi kelas dasar yang sama persis** — kalau padding-nya beda,
   lebarnya lompat saat berpindah.
4. **Track punya `p-1`** supaya pil aktif tidak menempel ke tepi.

Alias seperti `FOCUS RING` tetap boleh, karena dia menggantikan rangkaian utuh yang
sama di mana-mana — bukan menyuruh model memilih nilai per elemen.

### 3. Ikon — sebut nama Lucide-nya, dan **pertahankan data SVG-nya**

shadcn/ui memakai Lucide. Sebut nama ikonnya, jangan mendeskripsikan bentuknya
("ikon centang kecil", "panah ke kanan") — model sudah hafal set-nya.

**Tapi nama saja tidak cukup.** Kalau file section memuat `d="…"`, `viewBox`, atau
atribut stroke tertentu, semuanya **wajib ikut ditulis di prompt**. Lucide punya
banyak varian dan versi; menyebut nama saja membuat generator memilih path-nya
sendiri, dan bentuk ikonnya meleset dari file section. Data SVG adalah bagian dari
transkrip — memangkasnya untuk menghemat karakter itu membuang desain, bukan
menghemat.

Sama berlaku untuk SVG yang bukan ikon: logo, wordmark, ornamen. Jangan pernah
menghapusnya dari prompt hanya karena panjang.

```
- the Lucide "check" icon, size-4, shrink-0
- the Lucide "menu" icon, size-6
- the Lucide "arrow-right" icon, size-4, placed after the label
```

Sekali di `STYLE REQUIREMENTS` tulis atribut standarnya supaya semua ikon seragam:

```
- All icons are inline SVG from the Lucide set, rendered with viewBox "0 0 24 24", fill none, stroke currentColor, stroke-width 2, stroke-linecap round, stroke-linejoin round, and aria-hidden true
```

Nama yang sering dipakai: `check`, `x`, `arrow-right`, `chevron-down`, `menu`,
`search`, `star`, `plus`, `minus`, `external-link`. Ikon dekoratif → `aria-hidden`;
tombol yang isinya cuma ikon → wajib `aria-label`.

## Kalau lewat 5.000 karakter

Tempuh berurutan, berhenti begitu muat:

| # | Langkah |
|---|---|
| 1 | Gabung butir yang berdekatan jadi satu kalimat |
| 2 | Buang kata pengisi: "Optional", "consistent with existing UI", "e.g." |
| 3 | Class yang sudah disebut di `STYLE REQUIREMENTS` jangan diulang per butir |
| 4 | Daftar data dipadatkan jadi satu baris per item, bukan satu blok per item |
| 5 | Masih lewat → section-nya memang terlalu besar. Lapor angkanya ke user |

Jangan memangkas dengan membuang nama class atau mengganti teks literal jadi
deskripsi. Itu menukar panjang dengan kesalahan.

Angka nyata dari dua contoh di repo ini: `01-hero` 4.659 karakter,
`01-pricing` (4 kartu × 5 fitur + toggle + strip pembayaran) 4.972.
Section besar pun muat begitu markup-nya tidak ikut ditulis.

## Checklist sebelum file prompt dianggap selesai

- [ ] **≤ 5.000 karakter** — seluruh isi file, termasuk spasi, tab, dan baris baru. Dihitung, bukan dikira.
- [ ] Nol tab, nol baris ber-indentasi, akhiran baris LF (bukan CRLF).
- [ ] Nol front-matter, nol judul markdown, nol penanda copy — seluruh file adalah prompt.
- [ ] Nol blok kode, nol markup HTML.
- [ ] Enam blok lengkap dan urut, judulnya KAPITAL.
- [ ] Setiap teks yang tampil ada literal di dalam tanda kutip.
- [ ] Setiap ukuran, warna, dan radius disebut sebagai nama class Tailwind.
- [ ] **Cek balik ke file section: tiap class di prompt benar-benar ada di file, dan tiap class di file ada di prompt.** Nol improvisasi.
- [ ] Kalimat "use it verbatim, never substitute" ada di pembuka; larangan menambah varian `md:`/`lg:` ada di `STYLE REQUIREMENTS`.
- [ ] Palet bawaan Tailwind saja — nol token proyek, nol `var(--…)`.
- [ ] Nol kata sifat kualitas: "modern", "rapi", "menarik", "profesional".
- [ ] Ada blok `SPACING` berisi satu nilai per slot — nol rentang seperti "p-6 to p-8".
- [ ] Kontrol tersegmen: tiap segmen menyebut varian miliknya sendiri secara literal, ada kata `ONLY`, ada larangan silang. Nol placeholder (`X`, `n`) di nama kelas mana pun.
- [ ] Ikon disebut dengan nama Lucide, **dan** `d="…"`/`viewBox`/atribut stroke-nya ikut ditulis persis seperti di file section. Nol SVG yang dibuang.
- [ ] `CONSTRAINTS:` ada di paling akhir, lima baris, baris ketiga menyebut pola CSS-only-nya.
- [ ] Tidak ada file prompt untuk navbar/tabs/dropdown — melebur ke prompt section induknya.

## Uji cepat sebelum dikirim ke user

1. **"Ada satu titik pun yang butuh model menebak?"** Ada → ganti dengan nama class
   atau teks literal.
2. **"Kalau kalimat ini dihapus, ada output yang jadi lolos padahal harusnya ditolak?"**
   Tidak → hapus kalimatnya.
