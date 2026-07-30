# gbro-collage-broll

Mengubah satu baris naskah voice-over (~5 detik) menjadi *sharp visual idea*, lalu menghasilkan **animasi assembly kolase kertas halftone (halftone paper-collage)** bergaya editorial premium sebagai B-roll — didukung oleh generasi video first/last-frame Gemini Omni Flash.

## Hasil Akhir

- Bidang warna kertas solid yang kuat dan datar + kliping foto halftone hitam-putih + aksen kertas karton berwarna
- Elemen meluncur masuk satu per satu dari ruang kosong, terpasang, dan tersusun (bertekstur stop-motion), bukan fade-in atau zoom lambat
- Secara default menghasilkan MP4 rasio 9:16, durasi 5 detik, resolusi 720×1280, 24fps, tanpa suara — siap ditumpuk langsung di bawah voice-over

## Alur Kerja: Persetujuan Tiga Gerbang (Three-Gate Approval)

Inti dari skill ini bukan template prompt, melainkan proses persetujuan tiga tahap yang wajib dilalui, sehingga fokus Anda tercurah pada penilaian estetika, bukan menghabiskan biaya generasi:

1. **Gate 1 · Konfirmasi Metafora** — Hanya menghasilkan rencana metafora visual (makna inti / objek kunci / warna dasar / urutan perakitan), tanpa menghasilkan gambar atau video apa pun
2. **Gate 2 · Konfirmasi Frame Statis** — Setelah dikonfirmasi, baru menghasilkan frame statis kolase berwarna + contact sheet, menunggu konfirmasi Anda sekali lagi
3. **Gate 3 · Generasi Video** — Setelah frame statis disetujui, otomatis menggunakan `gemini-omni-flash-preview` untuk membuat animasi perakitan first/last-frame, dilengkapi QA lengkap (ekstraksi frame per detik, verifikasi ruang kosong pada frame pertama, perbandingan frame terakhir)

Pada mode batch, persetujuan sebagian didukung: hanya entri yang telah dikonfirmasi yang lanjut ke tahap berikutnya.

## Persyaratan Lingkungan

Saat pertama kali dipicu, skill akan otomatis menjalankan `scripts/check_setup.sh` untuk pengecekan mandiri, dan memberikan panduan konfigurasi untuk item yang hilang. Dibutuhkan:

| Dependensi | Keterangan |
| --- | --- |
| Lingkungan Codex | Generasi frame statis di Gate 2 bergantung pada tool bawaan `image_gen` |
| `GEMINI_API_KEY` | Dibuat melalui [Google AI Studio](https://aistudio.google.com/apikey), generasi video ditagih sesuai penggunaan |
| Python >= 3.10 | Digunakan untuk skrip generasi video |
| `google-genai >= 2.10.0` | Skill akan memandu pembuatan venv bersama di `~/hyperframes-projects/.omni-venv/` |
| ffmpeg / ffprobe | Untuk pemrosesan first/last-frame, penghapusan audio, dan pembuatan contact sheet |

Skrip generasi video (`scripts/generate_video.py` + `scripts/upload_file.py`) sudah disertakan bersama skill ini, tidak perlu menginstal skill tambahan lainnya.

## Instalasi

Tempatkan seluruh direktori ke dalam direktori agent skills Anda (misalnya `~/.agents/skills/` atau `~/.claude/skills/`):

```
git clone https://github.com/pyang5166/gbro-collage-broll.git ~/.agents/skills/gbro-collage-broll
```

## Cara Penggunaan

Katakan pada agent Anda:

```
collage b-roll: banyak orang mengira AI itu hadir untuk berpikir menggantikan kita, padahal ia lebih seperti cermin yang memantulkan celah dalam pertanyaan kita sendiri.
```

Kata pemicu: `collage b-roll`, `纸拼贴 b-roll`, `半调拼贴`, `拼贴风格配画面`, `gbro-collage-broll`.

Kemudian konfirmasi secara berurutan melalui Gate 1 → Gate 2 → Gate 3. Anda juga bisa memasukkan beberapa naskah sekaligus dalam mode batch — setiap kalimat menghasilkan satu metafora dan satu klip jadi.

## Struktur Direktori

```
gbro-collage-broll/
├── SKILL.md                        # Dokumen utama skill (protokol tiga gerbang + template prompt + standar QA)
├── agents/openai.yaml              # Konfigurasi interface Codex
├── evals/evals.json                # Evaluasi perilaku keempat gerbang
└── scripts/
    ├── check_setup.sh              # Pengecekan mandiri lingkungan saat pertama kali digunakan
    ├── generate_video.py           # Generasi video batch dengan Gemini Omni Flash
    ├── upload_file.py              # Bantuan unggah Files API
    └── generate_veo_first_last.py  # Alur Veo lama (hanya disimpan untuk kompatibilitas, tidak digunakan secara default)
```

## FAQ

**Mengapa harus ada dua kali konfirmasi manual?** Jika metafora atau frame statis yang salah langsung masuk ke generasi video, biaya API sungguhan akan terbuang percuma. Mengubah teks di Gate 1 gratis, dan membuat ulang satu gambar di Gate 2 jauh lebih murah daripada menjalankan ulang satu video.

**Ada sedikit kertas terlihat di tepi frame pertama hasil akhir?** Sedikit itu masih bisa diterima; jika membutuhkan ruang kosong yang benar-benar ketat, disarankan menggunakan tool animasi dengan timeline yang bisa diedit untuk menyempurnakan bagian awal.

**Bisakah mengganti model video?** Secara default menggunakan `gemini-omni-flash-preview` secara tetap; hanya beralih jika model lain ditentukan secara eksplisit.

## Lisensi

MIT License
