# 📝 Catatan MoM

Aplikasi web sederhana untuk mencatat **Minutes of Meeting (MoM)**: rekam percakapan, transkrip live, lalu export ke **Word (.docx)** atau **Notepad (.txt)**. Langsung jalan di browser, tanpa server, tanpa install, dan bisa di-hosting gratis di GitHub Pages.

Cocok untuk meeting Zoom, Microsoft Teams, maupun offline, dengan bahasa Indonesia atau English.

## Fitur

- **Form MoM**: judul, tanggal & jam, platform, peserta (tag), agenda, catatan diskusi, keputusan, action items (Tugas / PIC / Deadline / Status), dan open issues.
- **Rekam percakapan**
  - *Mikrofon saja*, untuk meeting offline
  - *Mikrofon + audio tab/sistem*, untuk Zoom/Teams supaya suara peserta lain ikut terekam
  - Jeda / lanjut / stop, timer, dan indikator level suara
  - Potongan rekaman disimpan otomatis tiap 5 detik ke IndexedDB. Kalau browser crash, rekaman bisa dipulihkan saat aplikasi dibuka lagi.
- **Transkrip live** (Web Speech API): Bahasa Indonesia atau English, timestamp `[mm:ss]` per kalimat, bisa diedit, dan tersambung lagi otomatis kalau terputus.
- **Export**: Word (.docx), Notepad (.txt), Transkrip (.txt), Rekaman audio (.webm).
- **Copy prompt ringkasan**: menyalin prompt berisi info meeting, catatan, dan transkrip. Tinggal paste ke Claude untuk dirapikan jadi MoM terstruktur.
- **Riwayat**: auto-save, buka / duplikat / hapus MoM lama. *Duplikat* membawa judul, peserta, agenda, dan action item yang belum selesai, cocok untuk meeting rutin.
- Mode terang/gelap, responsif (laptop & HP), dan shortcut `Ctrl+S` untuk simpan.

## Deploy ke GitHub Pages

1. Buat repo baru di GitHub (Public), misalnya `catatan-mom`.
2. Upload semua file dengan struktur seperti di bawah (**Add file → Upload files**, lalu drag seluruh isi folder, termasuk folder `js/`).
3. Buka **Settings → Pages**.
4. Di *Build and deployment*, pilih **Source: Deploy from a branch**, **Branch: `main`**, folder **`/ (root)`**, lalu **Save**.
5. Tunggu 1–2 menit. Aplikasi bisa diakses di `https://<username>.github.io/catatan-mom/`.

## Coba di komputer sendiri

Mikrofon hanya bisa diakses lewat HTTPS atau `localhost`. Jalankan server lokal dari folder ini:

```bash
python -m http.server 8000
```

Lalu buka `http://localhost:8000`.

## Struktur file

```
index.html        Halaman utama
style.css         Tampilan (tema terang/gelap, responsif)
js/storage.js     Simpan MoM (localStorage) & rekaman (IndexedDB)
js/recorder.js    Rekam mikrofon / mikrofon + audio tab
js/transcript.js  Transkrip live (Web Speech API)
js/export.js      Export Word, Notepad, transkrip, prompt AI
js/app.js         Menghubungkan semua ke antarmuka
```

## Cara merekam meeting online (Zoom/Teams)

1. Pilih sumber audio **Mikrofon + audio tab/sistem**, lalu klik **Mulai**.
2. Di jendela berbagi layar:
   - **Zoom/Teams versi web**: pilih tab meeting, lalu centang **Share tab audio**.
   - **Zoom/Teams aplikasi desktop (Windows)**: pilih *Entire screen*, lalu centang **Share system audio**.
3. Suara kamu (mic) dan suara peserta lain (tab/sistem) akan digabung dalam satu rekaman.

> macOS tidak mendukung "system audio" lewat browser. Gunakan Zoom/Teams versi web di Chrome supaya bisa pakai "Share tab audio".

## Keterbatasan

- **Transkrip live hanya mendengar mikrofon default**, bukan audio tab. Untuk meeting online, pakai speaker (bukan headset) supaya suara peserta lain tertangkap, atau andalkan file rekaman.
- **Satu bahasa per sesi transkripsi.** Untuk meeting campuran Indonesia–English, pilih Bahasa Indonesia. Istilah English kadang salah tangkap.
- Transkrip live didukung **Chrome dan Edge** (desktop paling stabil) dan butuh internet. Firefox tidak didukung.
- Rekam audio tab/sistem hanya di browser desktop. Di HP, gunakan *Mikrofon saja*.
- Export Word memakai library [docx](https://github.com/dolanmiu/docx) dari CDN jsDelivr. Kalau CDN tidak bisa diakses, aplikasi otomatis membuat file `.doc` yang tetap bisa dibuka di Word.

## Privasi

- Semua catatan, transkrip, dan rekaman tersimpan **hanya di browser kamu** (localStorage & IndexedDB). Tidak ada server atau database.
- **Pengecualian:** saat transkrip live aktif, Chrome/Edge mengirim audio mikrofon ke layanan speech recognition Google/Microsoft untuk diubah jadi teks.
- Data tidak sinkron antar perangkat atau browser. Clear browsing data akan menghapus semua MoM, jadi export file penting secara berkala.
- Minta izin peserta sebelum merekam meeting.
