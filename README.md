# SGT - Junior Frontend Technical Test v2

Proyek ini adalah hasil pengerjaan Technical Test untuk posisi Junior Frontend Developer. Proyek ini mereplikasi fitur *scroll-toggle* yang interaktif menggunakan ekosistem Vue.js dan animasi tingkat tinggi dari GSAP.

## 🚀 Tech Stack
- **Framework:** Vue.js 2 (Options API)
- **Animation:** GSAP (GreenSock Animation Platform)
- **Plugins:** ScrollTrigger & ScrollToPlugin
- **Styling:** SASS / SCSS (Pre-processor)
- **Deployment:** Vercel

## ✨ Fitur Utama
1. **Section 1: Interactive Pinning**
   - Menggunakan `ScrollTrigger` untuk mengunci layar (*pinning*).
   - Navigasi teks yang sinkron dengan posisi scroll.
   - Transisi gambar menggunakan teknik *Scale & Fade* untuk memberikan kesan kedalaman.
2. **Section 2: Cinematic Scrubbing**
   - Animasi yang sepenuhnya interaktif mengikuti putaran wheel mouse (*scrubbing*).
   - Efek *zoom-out* gambar yang halus tanpa merusak *masking* kontainer.
   - Dinamis counter (1-3) dan judul yang berubah sesuai progres animasi.
3. **Smooth Navigation**
   - Fitur *Click-to-Scroll* menggunakan `ScrollToPlugin` untuk berpindah antar konten secara presisi.

## 📁 Struktur Folder
Berdasarkan standar pengerjaan, kode diorganisir sebagai berikut:
- `src/assets/`: Menyimpan gambar proyek dan file ikon SVG.
- `src/components/`: Berisi logic modular untuk Section 1 dan Section 2.
- `src/App.vue`: Entry point utama yang menggabungkan seluruh section.
