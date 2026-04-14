# 🧮 Dokumentasi Algoritma Simulasi
## Rezim Fantasy Padel Manager

> Dokumen ini menjelaskan seluruh mekanisme perhitungan simulasi pertandingan,
> dilengkapi contoh nyata dan panduan pencatatan match yang melibatkan **pemain luar Rezim70**.

---

## 📋 Daftar Isi

1. [Data Dimensi Statistik Pemain](#1-data-dimensi-statistik-pemain)
2. [Algoritma Chemistry Score](#2-algoritma-chemistry-score)
3. [Algoritma Pair Rating](#3-algoritma-pair-rating)
4. [Algoritma Win Probability](#4-algoritma-win-probability)
5. [Simulasi Skor Americano](#5-simulasi-skor-americano)
6. [Kategorisasi Hasil Pertandingan](#6-kategorisasi-hasil-pertandingan)
7. [Update Statistik Pasca Pertandingan](#7-update-statistik-pasca-pertandingan)
8. [Match Solo vs Tim Luar Rezim (External Player)](#8-match-solo-vs-tim-luar-rezim-external-player)
9. [Contoh Lengkap: Enal+Ai vs Rafly+Rifky](#9-contoh-lengkap-enal--ai-vs-rafly--rifky)
10. [Ringkasan Formula](#10-ringkasan-formula)

---

## 1. Data Dimensi Statistik Pemain

Setiap pemain memiliki **5 dimensi statistik** dengan rentang nilai **65–95**:

| Dimensi | Kode | Deskripsi |
|---|---|---|
| Attacking & Power | `atk` | Kemampuan serangan, smash, agresivitas |
| Defense & Control | `def` | Kemampuan bertahan, akurasi, kontrol bola |
| Physical Agility | `phy` | Stamina, kecepatan, kelincahan |
| Tactical & Technical | `tac` | Strategi, positioning, teknik bermain |
| Mental Strength | `men` | Ketahanan tekanan, konsistensi, clutch |

**Overall Rating** dihitung sebagai rata-rata aritmatika:

```
Overall = (atk + def + phy + tac + men) / 5
```

### Tipe Pemain (Player Type)

Tipe pemain menentukan peran dan kompatibilitas dalam pasangan:

| Tipe | Keterangan |
|---|---|
| Aggressive Attacker | Serangan kuat, ujung tombak permainan |
| Attacker | Menyerang seimbang |
| Balance All Rounder | Serba bisa, adaptif |
| Defensive | Bertahan solid |
| Pure Defensive | Spesialis bertahan |

---

## 2. Algoritma Chemistry Score

Chemistry Score mengukur **kecocokan dua pemain** sebagai pasangan, dengan rentang **0–100**.

### Formula Utama

```
Chemistry = (TypeCompat × 0.40)
           + (StatsComplement × 0.40)
           + (RatingSimilarity × 0.20)
           + HistoryBonus
```

> **`HistoryBonus`** adalah akumulasi bonus dari latihan bersama sesama Rezim70 (+2 per match, maksimal +15 poin).

---

### Komponen 1 — Type Compatibility (Bobot 40%)

Matriks kompatibilitas antar tipe pemain:

| | Agg. Attacker | Attacker | All Rounder | Defensive | Pure Defensive |
|---|---|---|---|---|---|
| **Agg. Attacker** | 70% | 75% | 82% | 88% | **95%** |
| **Attacker** | 75% | 70% | 83% | **90%** | 85% |
| **All Rounder** | 82% | 83% | 85% | 84% | 82% |
| **Defensive** | 88% | **90%** | 84% | 70% | 80% |
| **Pure Defensive** | **95%** | 85% | 82% | 80% | 70% |

> 💡 **Prinsip:** Pasangan dengan peran saling melengkapi (attacker + defender) mendapat skor lebih tinggi dari pada pasangan tipe sama.

---

### Komponen 2 — Stats Complementary (Bobot 40%)

Mengukur sejauh mana kedua pemain **saling menutupi kelemahan** satu sama lain.

**Formula per dimensi:**
```
coverScore(dimensi) = min(100,  70 + gap × 1.5)
  di mana gap = |nilai_A - nilai_B|
```

**Rata-rata 5 dimensi:**
```
StatsComplement = (coverScore_atk + coverScore_def + coverScore_phy
                 + coverScore_tac + coverScore_men) / 5
```

> 💡 **Interpretasi:** Semakin besar perbedaan nilai pada satu dimensi, semakin tinggi potensi saling cover. Pasangan terbaik adalah yang menutup kelemahan satu sama lain, bukan yang sama kuat atau sama lemah.

---

### Komponen 3 — Rating Similarity (Bobot 20%)

Mengukur keseimbangan level antar pemain dalam satu pasangan:

| Selisih Overall | Skor Similarity |
|---|---|
| < 3 poin | 100% |
| 3 – 5 poin | 85% |
| > 5 poin | 70% |

> 💡 **Prinsip:** Pasangan dengan level terlalu jauh (misal 84.8 vs 74.0) cenderung tidak bisa bermain sinkron.

---

## 3. Algoritma Pair Rating

Pair Rating adalah **kekuatan gabungan** sebuah pasangan untuk simulasi pertandingan.

### Formula

```
BaseRating    = (Overall_P1 + Overall_P2) / 2
ChemMult      = lihat tabel di bawah
InjuryModP1   = 0.9 jika Minor Injury, 1.0 jika Fit
InjuryModP2   = 0.9 jika Minor Injury, 1.0 jika Fit
FormBonus     = bonus/penalti berdasarkan streak kemenangan

PairRating = (BaseRating × InjuryModP1 × InjuryModP2 × ChemMult) + FormBonus
```

### Chemistry Multiplier

| Chemistry Score | Multiplier |
|---|---|
| 90 – 100% | **1.10×** |
| 80 – 89% | **1.08×** |
| 70 – 79% | **1.05×** |
| < 70% | **1.00×** |

### Form Bonus (per pemain)

| Status Form | Syarat | Bonus ke PairRating |
|---|---|---|
| 🔥 Hot | Streak ≥ 5 win | +2.5 |
| ✅ Good | Streak ≥ 3 win | +1.5 |
| ➡️ Normal | 0 – 2 win | 0 |
| ❄️ Cold | Streak ≤ -3 loss | -1.5 |

> Total FormBonus = FormBonus_P1 + FormBonus_P2

---

## 4. Algoritma Win Probability

Win Probability menggunakan fungsi **sigmoid** untuk mengkonversi selisih rating menjadi probabilitas menang:

```
ratingDiff    = PairRating_A - PairRating_B
winProbA      = 1 / (1 + e^(-ratingDiff × 0.15))

winProbA      = clamp(winProbA, min=0.10, max=0.90)
winProbB      = 1 - winProbA
```

> `clamp(x, min, max)` memastikan probabilitas tidak pernah 0% atau 100% — selalu ada kemungkinan upset.

### Interpretasi Selisih Rating

| Selisih Rating | Win Probability Unggulan |
|---|---|
| 0 (setara) | ~50% |
| 3 | ~58% |
| 5 | ~65% |
| 8 | ~73% |
| 10 | ~78% |
| 15+ | ~86–90% (di-cap) |

---

## 5. Simulasi Skor Americano

Format pertandingan Rezim70 adalah **Americano**: total poin = **21**, satu set.

### Mekanisme Simulasi Point-by-Point

```
untuk setiap point (1 sampai 21):
    adjustedProb = winProbA + (momentum × 0.02)
    adjustedProb = clamp(adjustedProb, 0.10, 0.90)

    jika random() < adjustedProb:
        scoreA += 1
        momentum = min(3, momentum + 1)   ← momentum Pair A naik
    lainnya:
        scoreB += 1
        momentum = max(-3, momentum - 1)  ← momentum Pair B naik
```

**Penjelasan Momentum:**
- Sistem momentum mensimulasikan efek psikologis di lapangan
- Jika satu pair mencetak beberapa poin berturut-turut, probabilitas mereka naik tipis (maks +6%)
- Ini menciptakan simulasi yang lebih realistis (ada momentum swing, comeback, dsb.)

---

## 6. Kategorisasi Hasil Pertandingan

Berdasarkan **margin** kemenangan (selisih skor):

| Margin | Kategori | Contoh Skor |
|---|---|---|
| ≥ 10 poin | **DOMINANT** | 17-4, 18-3, 19-2 |
| 5 – 9 poin | **COMFORTABLE** | 13-8, 14-7, 15-6 |
| 1 – 4 poin | **CLOSE** | 11-10, 12-9, **12-9** |

---

## 7. Update Statistik Pasca Pertandingan

Update dilakukan secara **manual oleh Super Manager** melalui form input.

### Aturan Perubahan Stats

#### Untuk Pemenang:

| Kategori | Perubahan Stats |
|---|---|
| DOMINANT (margin ≥ 10) | **+2** ke stat terkuat masing-masing pemain |
| COMFORTABLE (margin 5–9) | **+1** ke stat terkuat masing-masing pemain |
| CLOSE (margin 1–4) | **+1 Mental** (clutch performance!) |

#### Untuk Pemenang bergelar MVP (tambahan):

```
+1 ekstra ke stat terkuat
+1 Mental Strength
```

#### Untuk Pihak yang Kalah:

| Situasi | Perubahan Stats |
|---|---|
| Kalah CLOSE (margin 1–4) | **+0.5 Mental** (berjuang keras) |
| Kalah COMFORTABLE / DOMINANT | Tidak ada perubahan stats |

> 💡 **Filosofi:** Kekalahan tidak mengurangi stats. Stats hanya naik dari kemenangan atau kompetisi ketat. Data stagnan jika pemain tidak aktif bermain.

#### Chemistry Update — Aturan Lengkap:

Aturan chemistry berbeda tergantung **jenis match** dan **komposisi pair**:

| Jenis Match | Komposisi Pair | Hasil | Chemistry |
|---|---|---|---|
| REZIM (Internal) | Keduanya anggota Rezim | Menang | ✅ +2 |
| REZIM (Internal) | Keduanya anggota Rezim | Kalah | ✅ +2 (tetap dapat) |
| SOLO (vs External) | Keduanya anggota Rezim | **Menang** | ✅ +2 |
| SOLO (vs External) | Keduanya anggota Rezim | **Kalah** | ❌ 0 |
| SOLO / REZIM | Salah satu / keduanya External | Win/Loss | ❌ 0 |

> 💡 **Prinsip:** Chemistry hanya tumbuh ketika dua pemain Rezim **berjuang dan menang bersama** melawan lawan external. Kalah tidak menghasilkan chemistry growth di luar Rezim.

```
chemistry[Pair] += 2    ← per match yang memenuhi syarat di atas
chemistry max  = base + 15 poin bonus kumulatif
```

#### Form / Streak Update:

```
Pemenang: currentStreak = max(0, currentStreak) + 1
Kalah   : currentStreak = min(0, currentStreak) - 1
```

---

## 8. Match Solo vs Tim Luar Rezim (External Player)

> Fitur ini memungkinkan Super Manager mencatat match ketika pemain Rezim70 bermain
> **di luar sesi resmi Rezim** (solo, friendly, kompetisi eksternal) melawan tim dari komunitas lain.

### 8.1 Konsep Pemain External

Aplikasi menyediakan **3 slot dummy** sebagai placeholder lawan di luar Rezim70:

| Slot | Nama | Fungsi |
|---|---|---|
| Slot 1 | 🌐 External A | Placeholder lawan |
| Slot 2 | 🌐 External B | Placeholder lawan |
| Slot 3 | 🌐 External C | Placeholder lawan |

```
Aturan External Player:
- Muncul di dropdown pilihan pemain (grup "Tim Luar Rezim" di bagian bawah)
- TIDAK punya stats dalam sistem → tidak ada update statistik
- TIDAK bisa dipilih sebagai MVP
- TIDAK menghasilkan chemistry bonus
- Hanya sebagai pencatat identitas lawan di match history
```

### 8.2 Dua Skenario Utama

#### Skenario A — Pemain Rezim Solo (partner juga External)

```
Konfigurasi: [Rezim Player] + [External A]  vs  [External B] + [External C]
Jenis Match : SOLO
Contoh      : Hendra + External A  vs  External B + External C
```

| Aspek | Hasil |
|---|---|
| Stat Update Hendra | ✅ Sesuai kategori menang/kalah |
| Stat Update External | ❌ Tidak ada |
| Chemistry Hendra–ExtA | ❌ 0 (partner bukan Rezim) |
| Win/Loss Count Hendra | ✅ Tercatat |
| Form/Streak Hendra | ✅ Terupdate |

#### Skenario B — Dua Pemain Rezim Berpasangan vs External

```
Konfigurasi: [Rezim P1] + [Rezim P2]  vs  [External A] + [External B]
Jenis Match : SOLO
Contoh      : Ponti + Kresna  vs  External A + External B
```

| Aspek | Jika Menang | Jika Kalah |
|---|---|---|
| Stat Update (Ponti & Kresna) | ✅ Sesuai kategori | ⚪ Hanya CLOSE loss +0.5 Mental |
| Chemistry Ponti–Kresna | ✅ **+2** (keduanya Rezim & menang) | ❌ 0 (kalah SOLO) |
| Win/Loss Count | ✅ Win +1 masing-masing | ✅ Loss +1 masing-masing |
| Form/Streak | ✅ +1 | ✅ −1 |

### 8.3 Contoh Kasus: Hendra Solo — 2 Match Beruntun

**Match 1:** Hendra + External A vs External B + External C, Skor **12-9**
```
Margin   : 3  →  CLOSE WIN
Update Hendra:
  Mental  : 76 → 77  (+1 Mental, CLOSE winner)
  Overall : (75+76+75+75+77) / 5 = 75.6  (naik dari 75.4)
  Win     : +1
  Streak  : +1  →  Normal
Chemistry Hendra–ExtA  : 0  (partner external)
```

**Match 2:** Hendra + External A vs External B + External C, Skor **11-10**
```
Margin   : 1  →  CLOSE WIN
Update Hendra:
  Mental  : 77 → 78  (+1 Mental lagi)
  Overall : (75+76+75+75+78) / 5 = 75.8
  Win     : +2 kumulatif
  Streak  : +2  →  Normal (butuh streak 3 untuk Good)
Chemistry Hendra–ExtA  : 0  (partner external)
```

### 8.4 Contoh Kasus: Ponti + Kresna vs External (Menang)

**Setup:** Ponti + Kresna vs External A + External B, Skor **13-8**
```
Margin   : 5  →  COMFORTABLE WIN
Jenis    : SOLO
```

| Pemain | Stat Terkuat | Perubahan |
|---|---|---|
| **Ponti** | DEF 78 | DEF: 78 → 79, Overall: 76.2 → 76.4 |
| **Kresna** | PHY 88 | PHY: 88 → 89, Overall: 81.4 → 81.6 |

```
Chemistry[Ponti–Kresna] += 2  ✅
  (keduanya Rezim, menang, SOLO match → chemistry naik)
```

### 8.5 Contoh Kasus: Ponti + Kresna vs External (Kalah)

**Setup:** Ponti + Kresna vs External A + External B, Skor **8-13**
```
Margin   : 5  →  COMFORTABLE LOSS
Jenis    : SOLO

Update:
  Ponti  : stats tidak berubah, Loss +1, Streak -1
  Kresna : stats tidak berubah, Loss +1, Streak -1

Chemistry[Ponti–Kresna] = 0  ❌
  (kalah SOLO match → tidak dapat chemistry)
```

### 8.6 Cara Input di Super Manager

```
1. Buka Super Manager → tab "Input Match"
2. Pair A → pilih Ponti + Kresna  (dropdown Rezim)
3. Pair B → pilih 🌐 External A + 🌐 External B  (grup "Tim Luar Rezim")
4. Isi Score: 13 - 8
5. Jenis Latihan: "Solo / Di Luar Rezim"
6. MVP: pilih Ponti atau Kresna jika ada, atau kosongkan
7. Klik Preview → lihat banner biru "🌐 Match vs Tim Luar Rezim"
8. Klik Submit & Update → data tersimpan, bisa dicek di Match History
```

> ⚠️ **Perhatian:** Pemain External tidak bisa dipilih sebagai MVP.
> Preview Panel akan menampilkan banner biru sebagai indikator visual match external.

---

## 9. Contoh Lengkap: Enal + Ai vs Rafly + Rifky

> **Hasil nyata:** Score **9 - 12**, Rafly+Rifky menang
> **Jenis latihan:** Sesama Rezim70
> **MVP:** (tidak ada)

---

### 9.1 Data Awal Pemain

| Pemain | ATK | DEF | PHY | TAC | MEN | Overall | Tipe |
|---|---|---|---|---|---|---|---|
| **Enal** | 79 | 76 | 80 | 76 | 77 | **77.6** | Aggressive Attacker |
| **Ai** | 75 | 79 | 76 | 78 | 77 | **77.0** | Pure Defensive |
| **Rafly** | 77 | 79 | 77 | 79 | 78 | **78.0** | Defensive |
| **Rifky** | 80 | 78 | 77 | 77 | 78 | **78.0** | Attacker |

---

### 9.2 Chemistry Score — Enal + Ai

**Komponen 1 — Type Compatibility:**
```
Aggressive Attacker + Pure Defensive = 95%   ✅ Kombinasi ideal!
```

**Komponen 2 — Stats Complementary:**
```
ATK: Enal=79, Ai=75  → gap=4  → min(100, 70 + 4×1.5) = 76.0
DEF: Enal=76, Ai=79  → gap=3  → min(100, 70 + 3×1.5) = 74.5  ✅ Ai cover DEF Enal
PHY: Enal=80, Ai=76  → gap=4  → min(100, 70 + 4×1.5) = 76.0
TAC: Enal=76, Ai=78  → gap=2  → min(100, 70 + 2×1.5) = 73.0  ✅ Ai cover TAC Enal
MEN: Enal=77, Ai=77  → gap=0  → min(100, 70 + 0×1.5) = 70.0

StatsComplement = (76.0 + 74.5 + 76.0 + 73.0 + 70.0) / 5 = 73.9%
```

**Komponen 3 — Rating Similarity:**
```
Selisih Overall = |77.6 - 77.0| = 0.6   (< 3 poin)
RatingSimilarity = 100%
```

**Final Chemistry Enal+Ai:**
```
= (95 × 0.40) + (73.9 × 0.40) + (100 × 0.20)
= 38.00 + 29.56 + 20.00
= 87.56%  ≈  88%
```

---

### 9.3 Chemistry Score — Rafly + Rifky

**Komponen 1 — Type Compatibility:**
```
Defensive + Attacker = 90%   ✅ Kombinasi solid
```

**Komponen 2 — Stats Complementary:**
```
ATK: Rafly=77, Rifky=80  → gap=3  → min(100, 70 + 3×1.5) = 74.5  ✅ Rifky cover ATK Rafly
DEF: Rafly=79, Rifky=78  → gap=1  → min(100, 70 + 1×1.5) = 71.5
PHY: Rafly=77, Rifky=77  → gap=0  → min(100, 70 + 0×1.5) = 70.0
TAC: Rafly=79, Rifky=77  → gap=2  → min(100, 70 + 2×1.5) = 73.0  ✅ Rafly cover TAC Rifky
MEN: Rafly=78, Rifky=78  → gap=0  → min(100, 70 + 0×1.5) = 70.0

StatsComplement = (74.5 + 71.5 + 70.0 + 73.0 + 70.0) / 5 = 71.8%
```

**Komponen 3 — Rating Similarity:**
```
Selisih Overall = |78.0 - 78.0| = 0.0   (kembar identik!)
RatingSimilarity = 100%
```

**Final Chemistry Rafly+Rifky:**
```
= (90 × 0.40) + (71.8 × 0.40) + (100 × 0.20)
= 36.00 + 28.72 + 20.00
= 84.72%  ≈  85%
```

---

### 9.4 Pair Rating

**Pair A — Enal + Ai:**
```
BaseRating  = (77.6 + 77.0) / 2 = 77.3
Chemistry   = 88%  →  Multiplier = 1.08×  (rentang 80–89%)
InjuryMod   = 1.0 × 1.0  (keduanya Fit)
FormBonus   = 0 + 0  (keduanya Normal form)

PairRating (Enal+Ai) = 77.3 × 1.0 × 1.0 × 1.08 + 0 = 83.5
```

**Pair B — Rafly + Rifky:**
```
BaseRating  = (78.0 + 78.0) / 2 = 78.0
Chemistry   = 85%  →  Multiplier = 1.08×  (rentang 80–89%)
InjuryMod   = 1.0 × 1.0  (keduanya Fit)
FormBonus   = 0 + 0  (keduanya Normal form)

PairRating (Rafly+Rifky) = 78.0 × 1.0 × 1.0 × 1.08 + 0 = 84.2
```

> **Selisih Pair Rating hanya 0.7** — pertandingan ini sangat kompetitif!

---

### 9.5 Win Probability

```
ratingDiff = PairRating_A - PairRating_B
           = 83.5 - 84.2
           = -0.7   ← Rafly+Rifky sedikit lebih kuat

winProbA (Enal+Ai)    = 1 / (1 + e^(-(-0.7) × 0.15))
                      = 1 / (1 + e^(0.105))
                      = 1 / (1 + 1.1107)
                      = 1 / 2.1107
                      = 0.4738  ≈  47.4%

winProbB (Rafly+Rifky) = 1 - 0.474 = 52.6%
```

**Interpretasi:**
```
Enal+Ai     →  47.4% peluang menang  (underdog tipis)
Rafly+Rifky →  52.6% peluang menang  (favorit tipis)

Keduanya hampir setara — hasil bisa ke mana saja!
Probabilitas pasangan mana pun menang antara 47–53%.
```

---

### 9.6 Skor Akhir: 9 - 12

```
Hasil nyata : Enal+Ai = 9,  Rafly+Rifky = 12
Margin      : |12 - 9| = 3
Kategori    : CLOSE  (margin 1–4)

Kesimpulan  : Rafly+Rifky menang dengan kemenangan tipis,
              sesuai dengan prediksi algoritma (52.6% favorit).
```

**Apakah hasil ini realistis?**
```
Win Prob Rafly+Rifky = 52.6%
Selisih Pair Rating  = 0.7  (sangat kecil)
Kategori CLOSE       ✅ Sangat masuk akal secara statistik

Jika diulang 10 kali simulasi, kemungkinan:
  - 5–6 kali: Rafly+Rifky menang
  - 4–5 kali: Enal+Ai menang (upset)
```

---

### 9.7 Update Statistik Setelah Pertandingan

#### Input Super Manager:
```
Match Type : Sesama Rezim70
Score      : Enal+Ai  9  vs  Rafly+Rifky  12
Margin     : 3  →  Kategori: CLOSE
MVP        : (tidak ada)
```

#### Perubahan Stats:

**Rafly — Winner (CLOSE):**
```
+1 Mental Strength
Mental: 78 → 79
Overall: (77+79+77+79+79) / 5 = 391 / 5 = 78.2   (naik dari 78.0)
Win streak: +1
Form: Normal → Normal (belum mencapai 3 win berturut)
```

**Rifky — Winner (CLOSE):**
```
+1 Mental Strength
Mental: 78 → 79
Overall: (80+78+77+77+79) / 5 = 391 / 5 = 78.2   (naik dari 78.0)
Win streak: +1
```

**Enal — Loser (CLOSE loss):**
```
+0.5 Mental  ← bonus karena berjuang keras hingga akhir
Mental: 77 → 77.5
Overall: (79+76+80+76+77.5) / 5 = 388.5 / 5 = 77.7   (naik dari 77.6)
Loss streak: -1
```

**Ai — Loser (CLOSE loss):**
```
+0.5 Mental  ← same, bonus berjuang keras
Mental: 77 → 77.5
Overall: (75+79+76+78+77.5) / 5 = 385.5 / 5 = 77.1   (naik dari 77.0)
Loss streak: -1
```

#### Chemistry Update (latihan Sesama Rezim70):
```
Chemistry[Enal-Ai]     += 2  (bonus latihan bareng)
Chemistry[Rafly-Rifky] += 2  (bonus latihan bareng)
```

#### Ringkasan Perubahan:

| Pemain | Status | Mental | Overall | Streak |
|---|---|---|---|---|
| **Rafly** | Winner ✅ | 78 → **79** | 78.0 → **78.2** | +1 |
| **Rifky** | Winner ✅ | 78 → **79** | 78.0 → **78.2** | +1 |
| **Enal** | Loser, dekat ⚔️ | 77 → **77.5** | 77.6 → **77.7** | -1 |
| **Ai** | Loser, dekat ⚔️ | 77 → **77.5** | 77.0 → **77.1** | -1 |

---

## 10. Ringkasan Formula

```
┌─────────────────────────────────────────────────────────────────┐
│                    RINGKASAN SEMUA FORMULA                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  OVERALL                                                        │
│  Overall = (atk + def + phy + tac + men) / 5                    │
│                                                                 │
│  CHEMISTRY SCORE (0–100)                                        │
│  TypeCompat    → lihat matriks tipe (40%)                       │
│  StatsComplement = avg[min(100, 70 + gap×1.5)] per 5 dim (40%)  │
│  RatingSimilarity = 100/85/70 berdasarkan selisih overall (20%) │
│  Chemistry = TypeCompat×0.4 + Stats×0.4 + Rating×0.2            │
│            + HistoryBonus (latihan bareng, maks +15)            │
│                                                                 │
│  PAIR RATING                                                    │
│  Base    = (Overall_P1 + Overall_P2) / 2                        │
│  ChemMult= 1.10 / 1.08 / 1.05 / 1.00 (90/80/70/<70%)            │
│  Rating  = Base × InjuryP1 × InjuryP2 × ChemMult + FormBonus    │
│                                                                 │
│  WIN PROBABILITY                                                │
│  diff    = PairRating_A - PairRating_B                          │
│  probA   = clamp(1/(1+e^(-diff×0.15)), 0.10, 0.90)              │
│  probB   = 1 - probA                                            │
│                                                                 │
│  STAT UPDATE (setelah match)                                    │
│  DOMINANT  (margin ≥10)  : Winner +2 stat terkuat               │
│  COMFORTABLE (margin 5–9): Winner +1 stat terkuat               │
│  CLOSE  (margin 1–4)     : Winner +1 Mental                     │
│  CLOSE loss              : Loser +0.5 Mental                    │
│  MVP bonus               : +1 stat terkuat, +1 Mental           │
│                                                                 │
│  CHEMISTRY UPDATE                                               │
│  REZIM match, Rezim pair, WIN   : Pair chemistry +2             │
│  REZIM match, Rezim pair, LOSS  : Pair chemistry +2             │
│  SOLO match,  Rezim pair, WIN   : Pair chemistry +2             │
│  SOLO match,  Rezim pair, LOSS  : 0 (tidak dapat)               │
│  Pair dengan External player    : 0 (selalu)                    │
│                                                                 │
│  EXTERNAL PLAYER (Tim Luar Rezim)                               │
│  Stat Update  : TIDAK (external tidak punya stats)              │
│  MVP          : TIDAK bisa dipilih                              │
│  Chemistry    : TIDAK menghasilkan bonus                        │
│  Match Record : YA, tercatat di history sebagai lawan           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

*Dokumen ini terakhir diupdate pada 14 April 2026.*
*Berdasarkan implementasi `rezim-padel-manager.html` v1.1*

**Contoh pertandingan terdokumentasi:**
- Enal+Ai (9) vs Rafly+Rifky (12) — Internal Rezim70, CLOSE WIN
- Hendra solo 2x vs External (12-9, 11-10) — SOLO, CLOSE WIN
- Ponti+Kresna vs External (13-8) — SOLO, COMFORTABLE WIN
- Ponti+Kresna vs External (8-13) — SOLO, COMFORTABLE LOSS
