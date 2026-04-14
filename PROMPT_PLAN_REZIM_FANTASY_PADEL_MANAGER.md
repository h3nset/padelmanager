# 🎾 PROMPT PLAN: REZIM FANTASY PADEL MANAGER
> Dokumen ini adalah ringkasan dan prompt plan untuk membangun game fantasy padel manager berbasis komunitas **Rezim70 Padel Community**.
> Dibuat berdasarkan sesi perencanaan dan data statistik pemain (Update: 140426).

---

## 🎯 VISI & TUJUAN

**Rezim Fantasy Padel Manager** adalah game berbasis web yang:
- Mengolah data statistik nyata 24 pemain Rezim70 menjadi simulasi pasangan
- Membantu menemukan kombinasi pasangan paling solid secara analitik
- Berfungsi sebagai alat **Tournament Planning Tool** (Mode Super Manager)
- Menjadi platform engagement komunitas (Mode Fantasy League)
- Dibangun sebagai **Single HTML File** — distribusi via WhatsApp, zero server cost

---

## 📊 DATABASE PEMAIN (Sumber: playerstats.jpeg — Update 140426)

| No | Pemain | Attacking | Defense | Physical | Tactical | Mental | Overall | Player Type |
|---|---|---|---|---|---|---|---|---|
| 1 | Agung | 74 | 75 | 76 | 73 | 77 | 75.0 | Defensive |
| 2 | Ai | 75 | 79 | 76 | 78 | 77 | 77.0 | Pure Defensive |
| 3 | Andi | 82 | 83 | 80 | 86 | 80 | 82.2 | Balance All Rounder |
| 4 | Bimo | 75 | 77 | 72 | 77 | 75 | 75.2 | Defensive |
| 5 | Bobby | 76 | 75 | 73 | 76 | 76 | 75.2 | Attacker |
| 6 | Bowo Prasetyo | 77 | 78 | 78 | 77 | 78 | 77.6 | Attacker |
| 7 | Bowo Satrio | 75 | 77 | 76 | 75 | 74 | 75.4 | Defensive |
| 8 | Enal | 79 | 76 | 80 | 76 | 77 | 77.6 | Aggressive Attacker |
| 9 | Gammar | 77 | 78 | 77 | 79 | 78 | 77.8 | Defensive |
| 10 | Hendra | 75 | 76 | 75 | 75 | 76 | 75.4 | Balance All Rounder |
| 11 | Heru | 85 | 85 | 83 | 88 | 83 | 84.8 | Balance All Rounder |
| 12 | Irul | 75 | 76 | 76 | 75 | 79 | 76.2 | Defensive |
| 13 | Jino | 75 | 75 | 73 | 73 | 76 | 74.4 | Balance All Rounder |
| 14 | Kresna | 86 | 76 | 88 | 77 | 80 | 81.4 | Aggressive Attacker |
| 15 | Kukuh | 78 | 79 | 85 | 80 | 79 | 80.2 | Balance All Rounder |
| 16 | Mel | 78 | 77 | 77 | 80 | 79 | 78.2 | Attacker |
| 17 | Omar | 78 | 75 | 75 | 78 | 77 | 76.6 | Attacker |
| 18 | Ponti | 76 | 78 | 74 | 77 | 76 | 76.2 | Defensive |
| 19 | Rafly | 77 | 79 | 77 | 79 | 78 | 78.0 | Defensive |
| 20 | Rifky | 80 | 78 | 77 | 77 | 78 | 78.0 | Attacker |
| 21 | Shinta | 73 | 77 | 73 | 74 | 74 | 74.2 | Defensive |
| 22 | Surya | 74 | 78 | 75 | 75 | 76 | 75.6 | Pure Defensive |
| 23 | Tikie | 78 | 80 | 76 | 79 | 78 | 78.2 | Balance All Rounder |
| 24 | Yosa | 77 | 78 | 74 | 77 | 76 | 76.4 | Balance All Rounder |

**5 Dimensi Statistik:**
- **Attacking & Power** — Kemampuan serangan dan power smash
- **Defense & Control** — Kemampuan bertahan, akurasi, kontrol bola
- **Physical Agility** — Stamina, kecepatan, kelincahan
- **Tactical & Technical** — Strategi bermain, positioning, teknik
- **Mental Strength** — Konsistensi, ketahanan tekanan, clutch

**Klasifikasi Player Type:**
| Type | Warna | Deskripsi |
|---|---|---|
| Aggressive Attacker | Kuning | Serangan kuat, cocok jadi ujung tombak |
| Attacker | Putih | Menyerang seimbang |
| Balance All Rounder | Hijau | Serba bisa, adaptif |
| Defensive | Tidak berwarna | Bertahan solid |
| Pure Defensive | Biru | Spesialis bertahan |

---

## 🎮 GAME MODES

### MODE 1: Quick Tournament Simulator ⭐ (Prioritas Utama)
- User merakit **pasangan dari 24 pemain** Rezim70
- Sistem otomatis menghitung **Chemistry Score** antar pasangan
- Simulasi **bracket tournament** (knockout 8-12 pair)
- Output: Pemenang, stats performa, MVP

### MODE 2: Fantasy League (Weekly)
- Setiap member menjadi "manajer"
- Pilih 3 pasangan untuk kompetisi mingguan
- Poin berbasis: Win (+10), Games won (+2/game), Chemistry bonus (+5 jika >90%)
- Leaderboard antar manajer

### MODE 3: Super Manager Dashboard ⭐ (Real Tournament Tool)
- Password-protected — untuk manajer turnamen nyata
- Rekomendasi pasangan optimal berbasis data
- Analisis head-to-head vs tim lawan
- **Input dan update statistik** pemain dari hasil latihan
- Export lineup untuk turnamen real

---

## 🧮 ALGORITMA INTI

### 1. Chemistry Score (0–100)

```
CHEMISTRY = 
  Player Type Compatibility × 40%
  + Stats Complementary Score × 40%
  + Overall Rating Similarity × 20%
```

**Tabel Kompatibilitas Player Type:**
| Pasangan | Chemistry Awal |
|---|---|
| Aggressive Attacker + Pure Defensive | 95% |
| Attacker + Defensive | 90% |
| All Rounder + Any | 85% |
| Sesama tipe (misal 2 Attacker) | 70% |

**Stats Complementary:**
- Jika Player A lemah di Defense (76), Player B kuat di Defense (83) → nilai tinggi
- Pasangan seimbang (saling menutupi kelemahan) → skor lebih tinggi

**Overall Rating Similarity:**
| Selisih Overall | Skor |
|---|---|
| < 3 poin | 100% |
| 3–5 poin | 85% |
| > 5 poin | 70% |

**Contoh Kalkulasi:**
```
Heru (84.8) + Andi (82.2):
  Type Match (Both All Rounder): 85%
  Stats Complement: 92% (sangat seimbang)
  Rating Gap (2.6 poin): 100%

  FINAL CHEMISTRY = (85×0.4) + (92×0.4) + (100×0.2) = 90.8%
```

### 2. Pair Rating

```
Pair Rating = ((Overall_A + Overall_B) / 2) × Chemistry_Multiplier
```

**Chemistry Multiplier:**
| Chemistry | Multiplier |
|---|---|
| 90–100% | 1.10x |
| 80–89% | 1.08x |
| 70–79% | 1.05x |
| < 70% | 1.00x |

**Contoh:**
```
Heru+Andi: (84.8 + 82.2) / 2 × 1.09 = 91.0
Kresna+Kukuh: (81.4 + 80.2) / 2 × 1.08 = 87.3
Win Probability Heru/Andi: 65%
```

### 3. Match Simulation (Americano Format)

> **Format scoring Rezim70 adalah Americano: Total poin = 21, 1 set per match**

```
Contoh score: 13-8, 12-9, 11-10, 15-6, 17-4, dll.
```

**Kategori Kemenangan:**
| Margin | Kategori | Impact |
|---|---|---|
| 10+ poin | DOMINANT | Winner: +2 stat terkuat |
| 5–9 poin | COMFORTABLE | Winner: +1 stat terkuat |
| 1–4 poin | CLOSE | Winner: +1 Mental (clutch!) |

**Score Distribution berdasarkan Rating Gap:**
| Gap Rating | Prediksi Score |
|---|---|
| 0–2 (Very Close) | 11-10, 12-9, 10-11 |
| 3–5 (Slight Favorite) | 13-8, 12-9, 14-7 |
| 6–9 (Clear Favorite) | 15-6, 14-7, 16-5 |
| 10+ (Mismatch) | 18-3, 17-4, 19-2 |

---

## 📈 SISTEM UPDATE STATISTIK

### Prinsip Dasar
- **Manual only** — tidak ada auto-decay
- **Stats stagnan** jika pemain tidak main/tidak ada input
- **Stats naik** hanya melalui input Super Manager
- Pemain bisa request update via **WhatsApp** ke Super Manager

### Alur Update
```
Latihan selesai 
→ Pemain WA Super Manager (kirim score PadelUp/Americano)
→ Super Manager login ke app
→ Input match result (form)
→ Preview perubahan stats
→ Confirm & Save
→ Stats tersimpan, mempengaruhi simulasi berikutnya
```

### Formula Stat Growth (Per Match)
```
WIN (Dominant ≥10): 
  - Winner: +2 ke stat terkuat
  - Loser: no change

WIN (Comfortable 5-9): 
  - Winner: +1 ke stat terkuat
  - Loser: no change

WIN (Close 1-4): 
  - Winner: +1 Mental Strength
  - Loser: +0.5 Mental (berjuang keras)

MVP Bonus: 
  - +1 ekstra ke stat terkuat
  - +1 Mental Strength
```

### Sistem Chemistry (Latihan)
```
Latihan sesama Rezim70: chemistry += 2 per match
Latihan solo (di luar): chemistry += 0
Chemistry maximum: 100
```

**Timeline Chemistry:**
```
Heru + Andi awal: 85
→ 10 matches sesama Rezim → Chemistry: 100 (MAX)
```

### Form/Streak Tracker
| Status Form | Syarat | Bonus Simulasi |
|---|---|---|
| 🔥 Hot | 5+ win streak | +5% |
| ✅ Good | 3–4 win streak | +3% |
| ➡️ Normal | 0–2 win | 0% |
| ❄️ Cold | 3+ loss streak | -3% |

---

## 🏥 SISTEM INJURY

### Status Pemain
| Status | Icon | Kondisi | Dampak ke Simulasi |
|---|---|---|---|
| Fit | ✅ | Kondisi penuh | Stats 100% |
| Minor Injury | ⚠️ | Cedera ringan | Stats × 90% |
| Injured | ❌ | Tidak bisa main | Tidak bisa dipilih |

### Risiko Cedera per Match (Simulasi)
| Physical Rating | Injury Risk |
|---|---|
| ≥ 85 | 5% |
| 75–84 | 10% |
| < 75 | 15% |

---

## 👤 DUAL ROLE SYSTEM

### Regular Manager (Semua Member)
- Bisa merakit tim fantasy sendiri
- Simulasi tournament dengan pasangan pilihan
- Kompetisi di leaderboard
- **TIDAK bisa** update statistik real

### Super Manager (Manajer Real Turnamen)
- Login dengan password khusus
- Input semua hasil latihan ke database
- Update statistik nyata pemain
- Akses analytics lanjutan
- Export lineup untuk turnamen real
- Manage player status (Active / On Break / Injured)

---

## 🛠️ TECH STACK & PLATFORM

### Pilihan Teknologi
```
Core:        Vanilla JavaScript (ES6+)
Styling:     Tailwind CSS (via CDN)
Data:        localStorage (auto-save)
Platform:    Single HTML File (~500–800 KB)
```

### Alasan Pilihan Ini
- ✅ **Zero deployment** — cukup 1 file HTML
- ✅ **Distribusi via WhatsApp** — kirim file, langsung pakai
- ✅ **Offline-capable** — data tersimpan di browser
- ✅ **Mobile-friendly** — responsive di HP
- ✅ **No server cost** — gratis selamanya

### Roadmap Platform
| Phase | Platform | Kapan |
|---|---|---|
| Phase 1 | Single HTML file | Sekarang (MVP) |
| Phase 2 | Hosted PWA (GitHub Pages/Netlify) | Jika perlu data sync antar device |
| Phase 3 | Mobile App Wrapper | Jika komunitas berkembang |

---

## 🎨 UI/UX FLOW

### Navigasi Utama
```
HOME
├── [Quick Tournament Simulator]
├── [Fantasy League]
├── [Super Manager*] (* password protected)
├── [Player Database]
└── [Statistics]
```

### Screen: Team Builder
```
Available Players (24)          Your Pairs (0/12)
┌─────────────────┐            ┌─────────────────┐
│ Heru  (84.8) ⭐  │ ──drag──>  │ [Empty Pair 1]  │
│ Andi  (82.2) 🛡️  │            │ Chemistry: --   │
│ Kresna(81.4) ⚔️ │            │ Rating: --      │
│ ...             │            │                 │
└─────────────────┘            └─────────────────┘

[AI Suggest Pairs] [Reset] [Simulate Tournament]
```

### Screen: Match Input (Super Manager)
```
📝 INPUT HASIL AMERICANO
━━━━━━━━━━━━━━━━━━━━━━━
Tanggal    : [14/04/2026]
Pair A     : [Heru ▼] + [Andi ▼]
Pair B     : [Kresna ▼] + [Kukuh ▼]
Score      : Pair A [13] - Pair B [8]  (Total = 21 ✓)
Jenis      : (•) Sesama Rezim  ( ) Solo
MVP        : [Heru ▼]
Notes      : [Heru banyak smash winner]

[PREVIEW CHANGES]  [SUBMIT]
```

### Screen: Stats Preview
```
📈 STATS UPDATE PREVIEW
━━━━━━━━━━━━━━━━━━━━━━━
HERU (Winner + MVP)
  Overall: 84.8 → 85.4 ⬆️ (+0.6)
  Tactical: 88 → 90 ⬆️ (+2)
  Mental: 83 → 84 ⬆️ (+1)
  Chemistry with Andi: 85 → 87 ⬆️ (+2)

ANDI (Winner)
  Overall: 82.2 → 82.4 ⬆️ (+0.2)
  Tactical: 86 → 87 ⬆️
  Chemistry with Heru: 85 → 87 ⬆️

KRESNA (Loser)
  Stats: No change | Form: -1

KUKUH (Loser)
  Stats: No change | Form: -1

[CONFIRM & SAVE]  [CANCEL]
```

### Screen: Tournament Result
```
🏆 TOURNAMENT SIMULATION RESULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FINAL:  Heru+Andi  vs  Kresna+Kukuh
SCORE:  14 - 7  (COMFORTABLE WIN)

Champion: Heru + Andi ⭐
Chemistry: 90.8% | Pair Rating: 91.0
MVP: Heru (3 crucial smash winners)

[View Full Bracket] [Try Different Pairs] [Save Team]
```

---

## 📋 DATA STRUCTURE (JSON)

### Player Object
```json
{
  "id": 11,
  "name": "Heru",
  "stats": {
    "attacking": 85,
    "defense": 85,
    "physical": 83,
    "tactical": 88,
    "mental": 83
  },
  "overall": 84.8,
  "playerType": "Balance All Rounder",
  "performance": {
    "wins": 0,
    "losses": 0,
    "winRate": 0,
    "currentStreak": 0,
    "formStatus": "Normal",
    "mvpCount": 0,
    "lastUpdated": null,
    "status": "Fit"
  },
  "matchHistory": []
}
```

### Match Object (Americano)
```json
{
  "id": "match_001",
  "date": "2026-04-14",
  "pairA": { "player1": "Heru", "player2": "Andi", "score": 13 },
  "pairB": { "player1": "Kresna", "player2": "Kukuh", "score": 8 },
  "winner": "pairA",
  "margin": 5,
  "category": "COMFORTABLE",
  "type": "REZIM",
  "mvp": "Heru",
  "statsChanges": {
    "Heru": { "tactical": 2, "mental": 1, "overall": 0.6 },
    "Andi": { "tactical": 1, "overall": 0.2 }
  },
  "chemistryChanges": {
    "Heru-Andi": 2
  }
}
```

### Chemistry Matrix
```json
{
  "Heru-Andi": 85,
  "Heru-Kresna": 82,
  "Kresna-Kukuh": 79,
  ...
}
```

---

## 🚀 ROADMAP PENGEMBANGAN

### Phase 1 — MVP (Week 1-2)
```
✅ Player database 24 pemain (dari playerstats.jpeg)
✅ Pair builder (drag & drop)
✅ Chemistry calculator
✅ Americano match simulator (1v1 pair)
✅ Tournament bracket 8-pair knockout
✅ Manual stats input form (Super Manager)
✅ LocalStorage auto-save
✅ Basic match history log
```

### Phase 2 — Super Manager Tools (Week 3-4)
```
✅ Password-protected Super Manager login
✅ Bulk match input (multiple matches per sesi)
✅ Chemistry tracker (latihan sesama Rezim)
✅ Data export/import (JSON file)
✅ Player performance trends & charts
✅ Player status management (Fit / On Break / Injured)
```

### Phase 3 — Advanced Features (Week 5-6)
```
✅ AI Optimal Pair Recommendation
✅ Head-to-head analysis antar pasangan
✅ Tournament bracket visualization (animated)
✅ Print/Export lineup untuk turnamen real
✅ Mobile PWA (installable di HP)
```

### Phase 4 — Cloud Sync (Future)
```
✅ Hosted version (Netlify/Vercel)
✅ Cloud database (Supabase/Firebase free tier)
✅ Real-time sync antar device
✅ Multi-tournament tracking history
```

---

## 💡 FITUR UNIK PEMBEDA

| Fitur | Deskripsi |
|---|---|
| **Hyper-local** | Fokus 100% ke komunitas Rezim70 (24 pemain nyata) |
| **Americano Scoring** | Simulasi sesuai format bermain nyata (21 poin) |
| **Chemistry Evolution** | Chemistry bertumbuh organik dari latihan bareng |
| **WA-driven Update** | Pemain kirim score via WA, Super Manager input — simpel! |
| **Pair-centered** | Bukan individual star, tapi kekuatan pasangan |
| **Actionable** | Hasil simulasi langsung applicable ke turnamen nyata |
| **Zero Infrastructure** | 1 file HTML, kirim via WA, langsung pakai |

---

## 📝 PROMPT UNTUK CODING (AI PROMPT)

> Gunakan prompt berikut saat memerintahkan AI untuk mulai coding:

---

### 🤖 MASTER PROMPT — BUILD MVP

```
Bangun game "Rezim Fantasy Padel Manager" sebagai single HTML file menggunakan:
- HTML5, Vanilla JavaScript (ES6+), Tailwind CSS (via CDN)
- Data persistence: localStorage

KONTEN APLIKASI:
1. DATABASE: 24 pemain Rezim70 (terlampir di bawah)
2. 3 HALAMAN UTAMA: Home, Team Builder, Tournament Simulator
3. SUPER MANAGER MODE (password: rezim70) untuk update stats

FITUR MVP:
A. Player Pool Display (24 cards dengan stats bars visual)
B. Pair Builder (drag & drop / click to select max 2 pemain per pair)
C. Chemistry Calculator:
   - Player Type Compatibility (40%)
   - Stats Complementary (40%)  
   - Overall Rating Similarity (20%)
D. Tournament Simulator (Americano format, total 21 poin):
   - Input 4–8 pair untuk bracket knockout
   - Simulasi point-by-point probabilistik
   - Tampilkan bracket, score tiap match, champion
E. Super Manager Panel:
   - Input hasil match Americano (date, pair A, pair B, score A/B, type: Rezim/Solo, MVP)
   - Validasi total score = 21
   - Preview stats changes sebelum save
   - Auto-update: Win Comfortable (5-9 margin) = +1 best stat; Win Dominant (10+) = +2; MVP = +1 extra + +1 Mental
   - Chemistry update +2 jika type = Rezim
   - Form tracker (Hot/Good/Normal/Cold)
   - Match history log

DESIGN: Dark theme, premium UI, animated stats bars, color-coded player types (Aggressive=Yellow, Attacker=White, All Rounder=Green, Defensive=Gray, Pure Defensive=Blue), card-based layout, mobile responsive.

PLAYER DATA:
[Agung: ATK74,DEF75,PHY76,TAC73,MEN77,Overall75,Defensive]
[Ai: ATK75,DEF79,PHY76,TAC78,MEN77,Overall77,Pure Defensive]
[Andi: ATK82,DEF83,PHY80,TAC86,MEN80,Overall82.2,Balance All Rounder]
[Bimo: ATK75,DEF77,PHY72,TAC77,MEN75,Overall75.2,Defensive]
[Bobby: ATK76,DEF75,PHY73,TAC76,MEN76,Overall75.2,Attacker]
[Bowo Prasetyo: ATK77,DEF78,PHY78,TAC77,MEN78,Overall77.6,Attacker]
[Bowo Satrio: ATK75,DEF77,PHY76,TAC75,MEN74,Overall75.4,Defensive]
[Enal: ATK79,DEF76,PHY80,TAC76,MEN77,Overall77.6,Aggressive Attacker]
[Gammar: ATK77,DEF78,PHY77,TAC79,MEN78,Overall77.8,Defensive]
[Hendra: ATK75,DEF76,PHY75,TAC75,MEN76,Overall75.4,Balance All Rounder]
[Heru: ATK85,DEF85,PHY83,TAC88,MEN83,Overall84.8,Balance All Rounder]
[Irul: ATK75,DEF76,PHY76,TAC75,MEN79,Overall76.2,Defensive]
[Jino: ATK75,DEF75,PHY73,TAC73,MEN76,Overall74.4,Balance All Rounder]
[Kresna: ATK86,DEF76,PHY88,TAC77,MEN80,Overall81.4,Aggressive Attacker]
[Kukuh: ATK78,DEF79,PHY85,TAC80,MEN79,Overall80.2,Balance All Rounder]
[Mel: ATK78,DEF77,PHY77,TAC80,MEN79,Overall78.2,Attacker]
[Omar: ATK78,DEF75,PHY75,TAC78,MEN77,Overall76.6,Attacker]
[Ponti: ATK76,DEF78,PHY74,TAC77,MEN76,Overall76.2,Defensive]
[Rafly: ATK77,DEF79,PHY77,TAC79,MEN78,Overall78,Defensive]
[Rifky: ATK80,DEF78,PHY77,TAC77,MEN78,Overall78,Attacker]
[Shinta: ATK73,DEF77,PHY73,TAC74,MEN74,Overall74.2,Defensive]
[Surya: ATK74,DEF78,PHY75,TAC75,MEN76,Overall75.6,Pure Defensive]
[Tikie: ATK78,DEF80,PHY76,TAC79,MEN78,Overall78.2,Balance All Rounder]
[Yosa: ATK77,DEF78,PHY74,TAC77,MEN76,Overall76.4,Balance All Rounder]
```

---

### 🤖 PROMPT TAHAP 2 — SUPER MANAGER FULL PANEL

```
Tambahkan ke dalam Rezim Fantasy Padel Manager:

SUPER MANAGER DASHBOARD (password: rezim70):
1. Player Status Board: 
   - Filter: Active / At Risk / On Break / Injured
   - Show last match date per player
2. Bulk Match Input:
   - Input multiple matches dalam 1 form (up to 10 matches)
   - CSV upload support (format: Date,Player1A,Player2A,Player1B,Player2B,ScoreA,ScoreB,Type,MVP)
3. Chemistry Matrix:
   - Visual grid 24×24
   - Color coded: 90-100=gold, 80-89=green, 70-79=blue, <70=gray
4. Data Export/Import:
   - Export current game state as JSON
   - Import JSON to sync data
5. Player Status Manager:
   - Set player as: Fit / Minor Injury (stats ×0.9) / Injured (cannot play) / On Break (stats stagnan)
```

---

### 🤖 PROMPT TAHAP 3 — ANALYTICS & TOURNAMENT

```
Tambahkan fitur advanced ke Rezim Fantasy Padel Manager:

1. AI PAIR RECOMMENDATION:
   - Input: Budget constraint (pisahkan pemain top, tidak boleh 2 top di 1 pair)
   - Output: Top 5 pair combinations dengan reasoning
   
2. HEAD-TO-HEAD ANALYZER:
   - Pilih Pair A vs Pair B
   - Tampilkan probabilitas menang, statistik breakdown, keunggulan masing-masing
   
3. TOURNAMENT BRACKET VISUALIZER:
   - Animasi bracket draw
   - Step-by-step match simulation dengan drama (progress bar per point)
   - Champion ceremony screen
   
4. PERFORMANCE TRENDS:
   - Chart Overall rating history tiap pemain (line chart)
   - Chemistry heatmap top 10 pair
   - Win rate per pair (historical)
   
5. PRINT/EXPORT LINEUP:
   - Generate PDF-ready lineup card
   - WhatsApp-ready text summary
```

---

## ✅ CHECKLIST FINAL SEBELUM CODING

- [x] Data 24 pemain lengkap (5 dimensi stats + player type)
- [x] Algoritma chemistry terdefinisi
- [x] Format scoring Americano (total 21) terdefinisi
- [x] Kategori kemenangan & dampak ke stats terdefinisi
- [x] Dual role system: Regular Manager vs Super Manager
- [x] WA-based update flow terdefinisi
- [x] Tech stack dipilih: Vanilla JS + Tailwind CDN
- [x] MVP scope jelas: Pair Builder + Chemistry + Americano Sim + Super Manager Input
- [ ] Desain UI mockup (opsional sebelum coding)
- [ ] Password Super Manager ditetapkan (default: rezim70)
- [ ] Deployment target ditetapkan (local file / GitHub Pages)

---

*Dokumen ini dibuat pada 14 April 2026 berdasarkan sesi perencanaan Hendra & AI.*
*Sumber data: `playerstats.jpeg` (Update 140426) & `raw_plan.md`*
