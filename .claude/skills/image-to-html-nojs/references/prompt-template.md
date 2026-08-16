# Menulis Prompt yang Tahan Model Murah

Target: hasil dari Gemini 2.5 Flash / 3 Flash **tidak bisa dibedakan** dari hasil
model mahal. Caranya bukan menulis prompt lebih panjang — tapi memindahkan semua
**penalaran** dari model ke dalam prompt.

## Kenapa model murah gagal

Model mahal bisa menyimpulkan. Model murah tidak. Setiap kali prompt menuntut
kesimpulan, model murah menebak — dan tebakannya jelek.

| Yang menuntut penalaran (model murah gagal) | Ganti jadi (model murah berhasil) |
|---|---|
| "judul besar dan tebal" | `text-4xl md:text-5xl lg:text-6xl font-semibold tracking-tight leading-[1.1]` |
| "spacing yang nyaman" | `mt-6` untuk paragraf, `mt-8` untuk tombol, `mt-12` untuk logo |
| "warna sesuai brand" | `bg-primary text-primary-foreground` |
| "buat rapi dan menarik" | (hapus — tidak bisa diverifikasi, jadi tidak berguna) |
| "tulis copy yang cocok" | teks literal di tabel COPY |
| "susun layout dua kolom" | kerangka HTML jadi, model tinggal isi |

**Aturan emas: kalau sebuah instruksi tidak bisa dicek benar/salah secara
mekanis, model murah akan mengabaikannya. Buang atau ubah jadi angka/class.**

## 12 Teknik Wajib

1. **Beri kerangka HTML jadi.** Semua tag pembungkus + class-nya sudah ditulis.
   Model hanya mengisi bagian bertanda `<!-- ISI:n -->`. Ini pengungkit terbesar —
   mengisi jauh lebih mudah daripada mengarang.
2. **Nol kata sifat.** Tidak ada "modern", "bersih", "elegan", "profesional".
   Model murah tidak punya representasi stabil untuk kata-kata itu.
3. **Class ditulis literal, bukan disimpulkan.** Cantumkan string class persis.
4. **Whitelist class.** Daftar class yang boleh dipakai per slot. Mencegah
   halusinasi (`text-gray-750`, `shadow-medium`, `rounded-2.5xl` — tidak ada di Tailwind).
5. **Copy literal di tabel.** Setiap teks yang tampil ditulis apa adanya.
   Model murah menulis filler yang buruk kalau disuruh mengarang.
6. **Larangan berpasangan SALAH → BENAR.** Larangan telanjang ("jangan pakai JS")
   sering dilanggar; pasangan contoh tidak.
7. **Maksimum 6 larangan.** Lebih dari itu, model murah mulai menjatuhkan aturan
   di tengah daftar.
8. **Kalimat pendek, bernomor, imperatif.** Tanpa paragraf. Tanpa anak kalimat.
9. **Ulangi 3 aturan paling kritis di akhir prompt.** Model murah paling ingat
   bagian awal dan akhir; bagian tengah paling sering hilang.
10. **Kontrak output eksplisit.** "Keluarkan hanya kode. Tanpa penjelasan.
    Mulai `<!doctype html>`, akhiri `</html>`." Model murah suka menambah basa-basi.
11. **Checklist verifikasi di akhir.** Model murah tidak bisa menilai kualitas,
    tapi bisa mencocokkan daftar. Ini menutup ~80% sisa gap.
12. **Satu prompt satu section.** Tidak ada "sekalian buatkan footer juga".

## Struktur Prompt (8 blok, urutannya mengikat)

| Blok | Isi | Kenapa di posisi itu |
|---|---|---|
| 1 | Peran + kontrak output | Primacy — paling diingat |
| 2 | Larangan mutlak (≤6, pasangan SALAH→BENAR) | Dibaca sebelum menulis kode |
| 3 | Kerangka HTML siap isi | Inti pekerjaan |
| 4 | Tabel COPY (teks literal) | Menghilangkan mengarang |
| 5 | Whitelist class per slot | Menghilangkan halusinasi class |
| 6 | Tabel perilaku responsif | Model murah paling sering lupa breakpoint |
| 7 | Checklist verifikasi | Model cek dirinya sendiri |
| 8 | Ulangi 3 aturan kritis | Recency — paling diingat kedua |

### Blok 6 — tabel perilaku responsif

Kerangka Blok 3 sudah memuat semua kelas `md:`/`lg:`, jadi model tidak perlu
menghitung apa pun. Tabel ini gunanya sebagai **kunci verifikasi**: model
mencocokkan output-nya dengan tabel, bukan menyimpulkan dari tabel.

```markdown
| Elemen | 375px | 768px | 1024px+ |
|---|---|---|---|
| Nav desktop | hidden | hidden | tampil |
| Hamburger | tampil | tampil | hidden |
| Kolom hero | 1 (menumpuk) | 1 (menumpuk) | 2 (6/6) |
| Grid fitur | 1 kolom | 2 kolom | 3 kolom |
| Padding section | py-16 | py-24 | py-32 |
| Ukuran h1 | text-4xl | text-5xl | text-6xl |
| CTA utama | w-full | w-auto | w-auto |
```

Isi tabel harus cocok dengan kelas di kerangka. Kalau tidak cocok, model murah
akan bingung dan memilih salah satu secara acak.

Contoh lengkap yang sudah jadi: `_template/prompts/01-hero.md`. Tiru bentuknya persis.

---

## Kerangka Blok 3 — pola `<!-- ISI:n -->`

Tulis semua pembungkus + class. Beri tanda hanya di titik yang boleh diisi.

````markdown
```html
<section class="py-16 md:py-24 lg:py-32">
  <div class="mx-auto grid max-w-7xl items-center gap-12 px-6 lg:grid-cols-12 lg:gap-16 lg:px-8">
    <div class="lg:col-span-6">
      <span class="inline-flex items-center rounded-full border border-border px-3 py-1 text-xs font-medium uppercase tracking-widest text-muted-foreground">
        <!-- ISI:1 -->
      </span>
      <h1 class="mt-6 text-4xl font-semibold leading-[1.1] tracking-tight md:text-5xl lg:text-6xl">
        <!-- ISI:2 -->
      </h1>
    </div>
  </div>
</section>
```

Ganti setiap `<!-- ISI:n -->` dengan teks dari tabel COPY.
**Jangan ubah satu pun class, tag, atau urutan di luar tanda ISI.**
````

Kalimat penutup itu wajib ada. Tanpa itu, model murah akan "memperbaiki" kerangka.

## Checklist file prompt sebelum dianggap selesai

- [ ] Front-matter terisi (`section`, `order`, `file`, `image`, `includes` bila ada komponen melebur).
- [ ] 8 blok lengkap dan urut.
- [ ] Tabel perilaku responsif ada, dan isinya cocok dengan kelas di kerangka.
- [ ] Nol kata sifat kualitas ("bagus", "rapi", "modern", "menarik").
- [ ] Setiap teks yang tampil ada di tabel COPY — tidak ada yang disuruh dikarang.
- [ ] Setiap class ditulis literal; tidak ada yang harus disimpulkan model.
- [ ] Larangan ≤6, tiap satu punya pasangan SALAH → BENAR.
- [ ] Kalimat "jangan ubah apa pun di luar tanda ISI" ada.
- [ ] Kontrak output ada di Blok 1 dan diulang di Blok 7.
- [ ] Blok `@theme` identik dengan prompt section lain.
- [ ] Prompt bisa dipahami tanpa membuka file lain di project ini.
- [ ] Tidak ada file prompt untuk navbar/tabs/dropdown — melebur ke prompt section induknya.

## Uji cepat sebelum dikirim ke user

Baca ulang prompt sambil bertanya: **"kalau saya model bodoh yang hanya
mencocokkan pola, apakah ada satu titik pun yang butuh saya menebak?"**
Ada satu saja → tambal titik itu dengan nilai literal.
