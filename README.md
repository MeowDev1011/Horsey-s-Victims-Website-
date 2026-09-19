
# 🐴 Horsey's Victims — Horsey Dollars System

An official web application for the **Horsey's Victims** Lichess team, featuring a Horsey Dollars currency system, editable site cards, a marketplace, arcade mini-games, and a full admin panel.

![Horsey](https://img.shields.io/badge/mascot-🐴-d97706) ![License](https://img.shields.io/badge/license-MIT-blue) ![Languages](https://img.shields.io/badge/i18n-EN%20%7C%20ES%20%7C%20RU-green)

---

## ✨ Features

- 🌍 **Multi-language support** — English, Spanish, and Russian with automatic device detection
- 🐴 **Horsey Dollars** — Team currency system with real-time balance tracking
- 🏆 **Leaderboard** — Top Horsey holders with 🥇🥈🥉 medals
- 🏪 **Official Marketplace** — Buy/sell team items with Horsey Dollars
- 📌 **Editable Site Cards** — Admin can create/edit/delete info cards with raw HTML
- 🎮 **Arcade Games**
  - Catch Horsey! — Click speed game
  - Santa Horsey's Drop — Catch gifts, avoid bombs (with sound)
- 👑 **Full Admin Panel**
  - Manage user balances
  - Publish/delete market items
  - Create/edit/delete site cards & game info
  - Expel members
- 🔒 **Secure Firestore rules** — Public read, authenticated write
- 📱 **Fully responsive** — Mobile-first design

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | HTML5, Tailwind CSS (CDN), Vanilla JS |
| Backend | Firebase Firestore, Firebase Auth (Anonymous) |
| Hosting | Netlify (or any static host) |
| i18n | Custom lightweight translation system |
| Audio | Web Audio API |

---

## 📁 Project Structure

```

/
├── index.html          # Single-file app (all-in-one)
├── _redirects          # Netlify SPA redirect
├── README.md           # This file
└── (optional) assets/  # Images if you want to customize

```

The entire app is contained in **one file** (`index.html`) for simplicity.

---

## 🚀 Quick Start

### 1. Clone or download

```bash
git clone https://github.com/MeowDev1011/horseys-victims.git
cd horseys-victims
```

Or just create a folder and place index.html inside.

2. Configure Firebase

Open index.html and locate the firebaseConfig block near the bottom of the file:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Replace the placeholder values with your own Firebase project's credentials.

🔑 Where to find them: Firebase Console → Project Settings → General → Your apps → Web App → SDK setup and configuration.

⚠️ Security note: The Firebase config is not a secret (it's meant to be public in web apps). Real security comes from Firestore Rules and Authentication. Never commit private keys, service account JSON files, or admin SDK credentials.

3. Set up Firestore rules

Go to Firebase Console → Firestore Database → Rules and paste:

```javascript
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    function isSystemConfigured() {
      return exists(/databases/$(database)/documents/artifacts/horsey-dollars-v1/public/data/config/admin);
    }
    function isAuthenticated() {
      return request.auth != null;
    }

    match /artifacts/horsey-dollars-v1/public/data/config/admin {
      allow read: if true;
      allow create: if !isSystemConfigured();
      allow update, delete: if false;
    }

    match /artifacts/horsey-dollars-v1/public/data/{collectionName}/{docId} {
      allow read: if true;
      allow write: if isAuthenticated() && isSystemConfigured();
    }

    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

Click Publish.

4. Enable Anonymous Auth

Go to Firebase Console → Authentication → Sign-in method and enable Anonymous.

5. Deploy to Netlify

Create a _redirects file with:

```
/*    /index.html   200
```

Then drag-and-drop the folder into Netlify Drop, or connect your Git repo.

6. First-time setup

1. Open your deployed site.
2. The Setup Box appears asking for an admin password.
3. Enter a strong password and click Initialize System.
4. The page reloads and the full app becomes available.

---

🔑 Admin Guide

Accessing the admin panel

1. Click HORSEY DOLLARS ADMIN on the home screen.
2. Enter the password you set during setup.

Admin sections

Tab Purpose
Manage Coins Update a user's Horsey Dollars balance and reason
Market Publish new items or remove existing ones
Site Cards Create/edit/delete info cards (raw HTML allowed)
Games Edit game titles, icons, and instructions
Expel Permanently remove a user and all their balance

Editing site cards with HTML

You can use any HTML in the Card Content field, for example:

```html
<p>Welcome to <strong>Horsey's Victims</strong>!</p>
<ul>
  <li>Weekly arenas every Sunday</li>
  <li>Join our <a href="https://lichess.org/team/horseys-victims">Lichess team</a></li>
</ul>
<img src="https://example.com/banner.png" alt="Banner">
```

---

💰 How to Earn Horsey Dollars

Activity Reward
Team Battle — every 3 points +1 🐴
LMAO Team Fight — every 1 point +1 🐴
Recruitment — every 2 members +1 🐴
ACM title +20 🐴
AFM title +30 🐴
AIM title +40 🐴
AGM title +50 🐴
NM / WNM title +60 🐴
CM / WCM title +80 🐴
FM / WFM title +100 🐴
IM / WIM title +115 🐴
GM / WGM title +130 🐴

📩 All claims must be sent to @rajul_s on Lichess.

---

🌍 Adding a New Language

1. Open index.html and find the translations object.
2. Copy the en block and add a new key (e.g., fr for French).
3. Translate all values.
4. Add a language button in the switcher:

```html
<button class="lang-btn" data-lang="fr" onclick="setLanguage('fr')">🇫🇷 FR</button>
```

5. Update the detectLanguage() function to recognize the new language code:

```javascript
if (browserLang.startsWith('fr')) return 'fr';
```

---

🎨 Customization

Changing colors

The theme uses amber/orange tones. To change the primary color, search and replace in the <style> block:

· #d97706 — Primary amber
· #f59e0b — Secondary amber
· #fef3c7 — Light amber background

Changing the logo

The site uses the 🐴 emoji as the logo. To use a custom image:

1. Replace <div class="...">🐴</div> in the header with <img src="logo.png" class="w-28 h-28 ...">.
2. Add the image to your project folder.

---

🐛 Troubleshooting

Problem Solution
"Access Denied: Invalid Key" Verify the password matches what you set. If forgotten, delete the config/admin doc in Firestore.
Firestore writes fail Check the security rules are published and Anonymous Auth is enabled.
Setup box doesn't appear The config/admin doc already exists. Delete it from Firestore to re-trigger setup.
Language doesn't switch Check the browser console for errors. Clear localStorage: localStorage.removeItem('horsey_lang').
Games don't load Make sure the arcade tab was opened at least once; the canvas needs to be visible to size correctly.

---

🔐 Security Best Practices

· Never commit your Firebase config with real credentials to a public repo if you want extra privacy (optional — Firebase web config is safe to expose, but many teams keep it private anyway).
· Rotate API keys if you ever expose them accidentally: Firebase Console → Project Settings → General → Your apps → Regenerate config.
· Restrict your API key: Google Cloud Console → APIs & Services → Credentials → API key → HTTP referrers → add your Netlify domain.
· Enable App Check for extra protection against abuse.
· Backup your Firestore data regularly: Firebase Console → Firestore → Export.

---

🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository.
2. Create a feature branch (git checkout -b feature/amazing-idea).
3. Commit your changes (git commit -m 'Add amazing idea').
4. Push to the branch (git push origin feature/amazing-idea).
5. Open a Pull Request.

---

📜 License

This project is licensed under the MIT License — feel free to use, modify, and distribute.

---

🙏 Credits

· Developer: @MeowDev1011 — GitHub
· Project Owner: @GatoChess89 — Lichess
· Claim Manager: @rajul_s — Lichess
· Team: Horsey's Victims on Lichess
· Inspiration: Horsey, the Lichess Mascot

---

📞 Contact

· GitHub: github.com/MeowDev1011
· Lichess Team: lichess.org/team/horseys-victims
· Report issues: Open an issue on GitHub
· Claim Horsey Dollars: DM @rajul_s

---

<p align="center">
  <strong>🐴 Made with love for the Horsey's Victims community 🐴</strong>
</p>
