# Rezim Fantasy Padel Manager 🎾

Aplikasi web **satu-file** *(single-file)* yang komprehensif untuk mengelola komunitas padel Rezim70. Dibangun sepenuhnya menggunakan HTML, Vanilla JavaScript, dan Tailwind CSS. Tanpa perlu *database backend* atau instalasi *server*!

## 🌟 Fitur Utama

*   **👥 Database & Statistik Pemain:** Kelola database pemain interaktif dengan rating detail (Attacking, Defense, Physical, Tactical, Mental) dan logika *chemistry* keseluruhan.
*   **🤖 AI Optimal Pair Generator:** Secara otomatis menghitung dan menyarankan racikan pasangan pemain paling seimbang untuk turnamen berdasarkan performa, rating, dan *chemistry* saat ini.
*   **⚔️ Head-to-Head (H2H) Analyzer:** Bandingkan dua pasangan secara visual menggunakan *Radar Chart* untuk menganalisis kekuatan dan kelemahan sebelum pertandingan dimulai.
*   **🏆 Tournament Simulator:** Simulasikan turnamen sistem gugur format Americano dengan bagan *(bracket)* yang dinamis.
*   **📈 Pelacakan Performa:** Lacak kemenangan, kekalahan, rekor kemenangan beruntun *(win-streaks)*, penghargaan MVP, serta lihat riwayat perkembangan rating melalui grafik garis.
*   **💬 WhatsApp Report Generator:** Buat klasemen mingguan dan statistik MVP secara otomatis hanya dengan satu klik, diformat sangat rapi untuk ditempel langsung ke grup WhatsApp.
*   **☁️ Cloud Sync (P2P):** Sinkronisasi database yang mulus antar pengguna menggunakan penyimpanan *cloud* sementara (JSONBlob) tanpa memerlukan *backend* khusus.

## 🚀 Cara Penggunaan / Menjalankan

Karena murni merupakan aplikasi berbasis *frontend*, menjalankannya sangatlah mudah:

1.  *Clone* repositori ini atau unduh kode sumbernya (.zip).
2.  Buka file `index.html` (atau file utama HTML) di browser web modern mana pun.
3.  *(Opsional namun Disarankan)* *Host* file HTML tersebut di **GitHub Pages**, Vercel, atau Netlify untuk memanfaatkan fitur *Cloud Sync* sepenuhnya. Fitur pemanggilan jaringan (Fetch API) mungkin akan diblokir oleh browser jika Anda menjalankan file secara langsung dari penyimpanan komputer (melalui protokol `file://`) karena kebijakan CORS.

## 🔒 Akses Super Manager

Fitur-fitur rahasia dan manajerial (seperti input data pertandingan massal, peracikan algoritma simulasi, dan mem-publish sinkronisasi data) dikunci di bawah tab **Super Manager** khusus untuk para administrator. 

*   **Default Password:** `********************`

## 🛠️ Stack Teknologi

*   HTML5
*   Vanilla JavaScript (ES6+)
*   Tailwind CSS (via CDN)
*   Chart.js (via CDN)

---
*Dibuat dengan ❤️ oleh REZIM70*
