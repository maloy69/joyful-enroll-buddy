# Panduan Unggah Dokumen + Tur Langkah-demi-Langkah di Halaman Pendaftaran

## Tujuan
Membantu pendaftar (wali murid, sering memakai ponsel) memahami dokumen apa saja yang harus diunggah, format yang diterima, dan cara mengunggahnya — tanpa bingung.

## Perubahan

### 1. Panel "Panduan Unggah Dokumen" di langkah 5 (Dokumen)
Di atas daftar `DocumentUploader` pada langkah Dokumen, tambahkan kartu panduan berbahasa Indonesia sederhana berisi:

- **Daftar dokumen yang diminta** — sesuai `DOC_TYPES` (rapor, ijazah/SKL, akta kelahiran, KK, pas foto, dll.), ditandai wajib/opsional, dengan penjelasan singkat tiap dokumen (mis. "Pas foto 3×4 latar polos, wajah terlihat jelas").
- **Format yang diterima** — PDF, PNG, JPG, TIFF, dengan contoh nyata: "hasil scan/ foto ijazah dari kamera HP biasanya JPG — langsung boleh diunggah".
- **Ukuran maksimal** — 2 MB per berkas, dengan catatan bahwa foto otomatis dikecilkan menjadi WebP kualitas 50% sehingga hasil foto HP hampir selalu bisa diunggah tanpa perlu mengecilkan sendiri.
- **Tips singkat** — foto dokumen di tempat terang, jangan blur/miring, pastikan teks terbaca.

Panel ini juga bisa dibuka dari langkah mana pun lewat tombol kecil "Panduan dokumen" di bilah "Dokumen persyaratan" (bagian atas halaman, dekat tombol Unggah Dokumen).

### 2. Tutorial on-screen (tur terpandu dengan sorotan elemen)
Komponen baru `src/components/DocGuideTour.tsx`:

- Layar gelap tipis (overlay) dengan "lubang" yang menyorot elemen target: area elemen diberi bingkai menyala, elemen di-*scroll* otomatis ke tengah layar (`scrollIntoView`) agar benar-benar terlihat.
- Tooltip langkah dengan teks sederhana, tombol **Lanjut / Kembali / Lewati**, indikator "Langkah 1 dari N".
- Urutan langkah (langkah 5 Dokumen):
  1. Panel panduan — "Baca dulu dokumen apa saja yang harus disiapkan"
  2. Kartu dokumen pertama — "Ini kartu satu dokumen; baca nama & statusnya"
  3. Tombol "Pilih berkas" — "Tekan di sini untuk memilih berkas dari HP/komputer"
  4. Area status/unggahan — "Setelah diunggah, berkas tampil di sini dan menunggu verifikasi"
  5. Tombol lanjut/ringkasan — "Jika semua wajib sudah terunggah, lanjut ke Ringkasan"
- Tombol **"Lihat Tutorial"** (ikon tanya) di langkah Dokumen untuk memulai tur kapan saja; tur otomatis ditawarkan sekali untuk pengguna baru (flag `localStorage` `spmb-tour-docs-seen`, bisa diulang manual).
- Implementasi murni CSS/React (overlay + posisi elemen via `getBoundingClientRect`, dihitung ulang saat scroll/resize) — tanpa library tur baru, ringan dan aman untuk SSR (`ssr: false` sudah aktif di rute ini).

### 3. Penguatan teks di DocumentUploader
- Teks format di tiap kartu dipertajam: "PDF, PNG, JPG, TIFF — maks. 2 MB · foto otomatis dikecilkan".
- Pesan error format/ukuran menyebutkan panduan ("Lihat panduan unggah di atas").

## File yang disentuh
- `src/routes/pendaftaran.tsx` — panel panduan + tombol tutorial + pemasangan tur (langkah Dokumen).
- `src/components/DocGuideTour.tsx` — komponen tur baru (overlay sorot elemen).
- `src/components/DocumentUploader.tsx` — penajaman teks bantuan saja.

Tidak ada perubahan database, backend, atau logika unggah (format/WebP 50%/2 MB sudah berjalan — panduan hanya menjelaskannya).

## Verifikasi
- `bunx tsgo --noEmit` bersih, cek `build-errors.log`.
- Playwright: buka halaman pendaftaran sebagai pengguna masuk, cek panel panduan tampil, jalankan tutorial dan pastikan sorotan fokus ke elemen yang benar (screenshot tiap langkah).
