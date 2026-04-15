# Rezim Fantasy Padel Manager 🎾

A comprehensive, **single-file** web application for managing the Rezim70 padel community. Built entirely with HTML, Vanilla JavaScript, and Tailwind CSS. No backend database or server installation required!

## 🌟 Key Features

*   **👥 Player Database & Stats:** Manage an interactive database of players with detailed ratings (Attacking, Defense, Physical, Tactical, Mental) and overall chemistry logic.
*   **🤖 AI Optimal Pair Generator:** Automatically calculates and suggests the most balanced player pairings for tournaments based on current form, rating, and chemistry.
*   **⚔️ Head-to-Head (H2H) Analyzer:** Compare two different pairs visually using a Radar Chart to analyze strengths and weaknesses before a match.
*   **🏆 Tournament Simulator:** Simulate Americano format knockout tournaments with dynamic brackets.
*   **📈 Performance Tracking:** Track wins, losses, win-streaks, MVP awards, and view rating progression via line charts.
*   **💬 WhatsApp Report Generator:** One-click generation of the weekly leaderboard and MVP stats, formatted neatly for WhatsApp.
*   **☁️ Cloud Sync (P2P):** Seamless database synchronization using temporary cloud storage (JSONBlob) without needing a dedicated backend.

## 🚀 How to Use / Run

Because this is a completely frontend-based application, running it is incredibly simple:

1.  Clone this repository or download the source code.
2.  Open the `index.html` file in any modern web browser.
3.  *(Optional but Recommended)* Host the `index.html` file on **GitHub Pages**, Vercel, or Netlify to fully utilize the Cloud Sync feature (which may be blocked if run directly via the `file://` protocol due to CORS).

## 🔒 Super Manager Access

Certain features like bulk match recording and data synchronization are protected under the **Super Manager** tab for administrators. 

*   **Default Password:** `rezim70`

## 🛠️ Technology Stack

*   HTML5
*   Vanilla JavaScript (ES6+)
*   Tailwind CSS (via CDN)
*   Chart.js (via CDN)

---
*Built with ❤️ by REZIM70*
