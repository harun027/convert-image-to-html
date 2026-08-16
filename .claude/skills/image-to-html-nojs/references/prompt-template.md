# Menulis Prompt yang Tahan Model Murah

Target: hasil dari Gemini Flash kelas **tidak bisa dibedakan** dari hasil model mahal.
Caranya bukan menulis prompt lebih panjang — tapi memindahkan semua **penalaran**
dari model ke dalam prompt.

**Prompt panjang bukan prompt kuat.** Yang bekerja hanya kerangka HTML dan tabel
COPY. Sisanya penjelasan yang model murah lewati. Format wajib: **5 blok**, kerangka
mengambil ~80% isi file.

## Kenapa model murah gagal

Model mahal bisa menyimpulkan. Model murah tidak. Setiap kali prompt menuntut
kesimpulan, model murah menebak — dan tebakannya jelek.

| Yang menuntut penalaran (gagal) | Ganti jadi (berhasil) |
|---|---|
| "judul besar dan tebal" | `text-4xl md:text-5xl lg:text-6xl font-semibold tracking-tight` |
| "spacing yang nyaman" | `mt-6` paragraf, `mt-8` tombol, `mt-12` logo |
| "warna sesuai brand" | `bg-primary text-primary-foreground` |
| "buat rapi dan menarik" | (hapus — tidak bisa diverifikasi, jadi tidak berguna) |
| "tulis copy yang cocok" | teks literal di tabel COPY |
| "susun layout dua kolom" | kerangka HTML jadi, model tinggal isi |

**Aturan emas: kalau sebuah instruksi tidak bisa dicek benar/salah secara
mekanis, model murah akan mengabaikannya. Buang atau ubah jadi angka/class.**

## Struktur Prompt — 5 blok, urutan mengikat

| Blok | Isi | Kenapa |
|---|---|---|
| 1 · KONTRAK | Peran, format output, 3 larangan berpasangan SALAH→BENAR | Primacy — paling diingat |
| 2 · KERANGKA | HTML lengkap dengan tanda `<!-- ISI:n -->` | Inti pekerjaan, 80% isi file |
| 3 · COPY | Tabel teks literal per tanda ISI | Menghilangkan mengarang |
| 4 · CEK | 6 poin verifikasi mekanis | Model menilai dirinya sendiri |
| 5 · KUNCI | Ulang 2 aturan paling kritis | Recency — paling diingat kedua |

Contoh jadi: `_template/prompts/01-hero.md`. Tiru bentuknya persis.

## Yang SUDAH DIHAPUS dari format lama — jangan dihidupkan lagi

| Blok lama | Kenapa dibuang |
|---|---|
| Whitelist class per slot | Redundan. Kerangka sudah memuat tiap class secara literal; larangan "jangan ubah kerangka" sudah menutupnya. Ganti satu baris di Blok 4: *"setiap class di output ada juga di kerangka Blok 2"*. |
| Tabel perilaku responsif | Redundan. Semua kelas `md:`/`lg:` ada di kerangka. Ganti satu poin cek: *"semua kelas `md:` dan `lg:` dari kerangka utuh"*. |
| Larangan 4–6 | Terserap ke larangan "jangan ubah kerangka". Lebih dari 3 larangan mulai dijatuhkan model murah di tengah daftar. |
| Checklist 12 poin | Poin 7–12 mengulang larangan Blok 1. Cukup 6 poin yang bisa dicek dengan mata. |

Tiap blok yang dihidupkan lagi menambah teks yang harus dibaca model **tanpa
menambah satu pun batasan baru**. Itu menurunkan kepatuhan, bukan menaikkan.

## 7 Teknik Wajib

1. **Beri kerangka HTML jadi.** Semua tag pembungkus + class-nya sudah ditulis.
   Model hanya mengisi `<!-- ISI:n -->`. Pengungkit terbesar — mengisi jauh lebih
   mudah daripada mengarang.
2. **Nol kata sifat.** Tidak ada "modern", "bersih", "elegan", "profesional".
   Model murah tidak punya representasi stabil untuk kata-kata itu.
3. **Class ditulis literal, bukan disimpulkan.**
4. **Copy literal di tabel.** Setiap teks yang tampil ditulis apa adanya.
5. **Maksimum 3 larangan, tiap satu berpasangan SALAH → BENAR.** Larangan
   telanjang ("jangan pakai JS") sering dilanggar; pasangan contoh tidak.
6. **Kalimat pendek, bernomor, imperatif.** Tanpa paragraf, tanpa anak kalimat.
7. **Satu prompt satu section.** Tidak ada "sekalian buatkan footer juga".

## Kerangka Blok 2 — pola `<!-- ISI:n -->`

Isinya **sama persis dengan isi `sections/<nn>-<nama>/index.html`**: markup section
murni. Tanpa `<!doctype>`, `<html>`, `<head>`, `<meta>`, `<title>`, `<body>`,
`<script>`, `<style>`.

Warna dan radius muncul sebagai class di tag — `bg-white`, `text-neutral-500`,
`rounded-2xl` — dan ditulis literal di kerangka. **Palet bawaan Tailwind saja**, bukan
token proyek: prompt ini dipakai di AI luar yang tidak punya `@theme` project ini, jadi
`bg-primary` akan keluar tanpa warna di sana. Model tidak pernah diminta menyimpulkan
warna; kalau diminta, ia akan mengarang nilai yang beda dari section lain.

Tulis semua pembungkus + class. Beri tanda hanya di titik yang boleh diisi.

````markdown
```html
<section class="py-16 md:py-24 lg:py-32">
  <div class="mx-auto grid max-w-7xl items-center gap-10 px-6 md:gap-12 lg:grid-cols-12 lg:gap-16 lg:px-8">
    <div class="lg:col-span-6">
      <h1 class="text-4xl font-semibold leading-[1.1] tracking-tight md:text-5xl lg:text-6xl">
        <!-- ISI:1 -->
      </h1>
      <p class="mt-6 max-w-lg text-base leading-relaxed text-muted-foreground md:text-lg">
        <!-- ISI:2 -->
      </p>
    </div>
  </div>
</section>
```

Jangan ubah satu pun class, tag, atribut, atau urutan di luar tanda ISI.
````

Kalimat penutup itu wajib ada. Tanpa itu, model murah akan "memperbaiki" kerangka.

## Checklist file prompt sebelum dianggap selesai

- [ ] Front-matter terisi (`section`, `order`, `file`, `image`, `includes` bila ada komponen melebur).
- [ ] Tepat 5 blok, urut, tanpa blok tambahan.
- [ ] Kerangka Blok 2 **markup section murni** — mulai tag akar, akhiri tag penutupnya. Nol `<!doctype>`, `<html>`, `<head>`, `<meta>`, `<title>`, `<body>`, `<script>`, `<style>`.
- [ ] Tag akar di kerangka membawa `bg-*` + `text-*` sendiri; radius pakai skala bawaan (`rounded-lg`/`rounded-xl`/`rounded-2xl`/`rounded-full`).
- [ ] Nol token proyek dan nol `var(--…)` di kerangka — palet bawaan Tailwind saja.
- [ ] Nol kata sifat kualitas ("bagus", "rapi", "modern", "menarik").
- [ ] Setiap teks yang tampil ada di tabel COPY — tidak ada yang disuruh dikarang.
- [ ] Larangan tepat 3, tiap satu punya pasangan SALAH → BENAR.
- [ ] Kalimat "jangan ubah apa pun di luar tanda ISI" ada.
- [ ] Penanda `▼ COPY MULAI` dan `▲ COPY SELESAI` ada, dan semua di antaranya berdiri sendiri tanpa membuka file lain.
- [ ] Tidak ada file prompt untuk navbar/tabs/dropdown — melebur ke prompt section induknya.

## Uji cepat sebelum dikirim ke user

Dua pertanyaan:

1. **"Kalau saya model bodoh yang hanya mencocokkan pola, ada titik yang butuh
   saya menebak?"** Ada satu saja → tambal dengan nilai literal.
2. **"Kalau kalimat ini dihapus, apakah ada output yang jadi lolos padahal
   seharusnya ditolak?"** Tidak → hapus kalimatnya.
