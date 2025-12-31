# ⚡ פתרון מהיר - שגיאת MIME Type

## הבעיה שלך
```
index.tsx:1 Failed to load module script: Expected a JavaScript-or-Wasm module script 
but the server responded with a MIME type of "application/octet-stream"
```

## 🔥 הפתרון המיידי (5 דקות)

### שלב 1: עצור את השרת
```bash
# לחץ Ctrl+C בטרמינל
```

### שלב 2: נקה הכל
```bash
rm -rf node_modules package-lock.json .vite dist
```

### שלב 3: ודא שיש לך את הקבצים הנכונים

**קובץ: `index.html` (בשורש!)**
```html
<!DOCTYPE html>
<html lang="he" dir="rtl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>מגלה רצפי פלינדרום</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Assistant:wght@200;400;700&family=Frank+Ruhl+Libre:wght@400;700&display=swap" rel="stylesheet">
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/index.tsx"></script>
  </body>
</html>
```

**קובץ: `src/index.tsx`**
```typescript
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';

const root = createRoot(document.getElementById('root')!);
root.render(<React.StrictMode><App /></React.StrictMode>);
```

**קובץ: `vite.config.ts`**
```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000
  }
});
```

**קובץ: `package.json`**
```json
{
  "name": "tanakh-palindrome-finder",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "lucide-react": "^0.263.1"
  },
  "devDependencies": {
    "@types/react": "^18.2.43",
    "@types/react-dom": "^18.2.17",
    "@vitejs/plugin-react": "^4.2.1",
    "typescript": "^5.3.3",
    "vite": "^5.0.8"
  }
}
```

### שלב 4: התקן מחדש
```bash
npm install
```

### שלב 5: הפעל
```bash
npm run dev
```

## ✅ האם זה עובד?

אם כן - מעולה! 🎉

אם לא - נסה את זה:

### תרחיש A: עדיין אותה שגיאה
```bash
# בדוק את גרסת Node
node --version  # צריך להיות >= 18

# אם יותר נמוך, עדכן Node.js
# הורד מ: https://nodejs.org/

# נסה שוב
npm run dev
```

### תרחיש B: שגיאה אחרת
```bash
# הפעל עם debug mode
DEBUG=vite:* npm run dev

# העתק את השגיאה ותשלח לי
```

### תרחיש C: הדף לבן
1. פתח F12 (Developer Tools)
2. לך ל-Console
3. תראה מה הודעת השגיאה
4. תעדכן אותי

## 🎯 המבנה הנכון של הפרויקט

```
your-project/
├── node_modules/          (אחרי npm install)
├── src/
│   ├── index.tsx         ⭐ חשוב!
│   ├── App.tsx
│   ├── services/
│   │   └── geminiService.ts
│   └── utils/
│       └── hebrew.ts
├── index.html            ⭐ בשורש! לא בתוך src
├── vite.config.ts
├── tsconfig.json
├── package.json
└── .env.local
```

## 🚨 טעויות נפוצות

### ❌ לא נכון
```html
<!-- index.html -->
<script type="module" src="./src/index.tsx"></script>  ❌
<script type="module" src="src/index.tsx"></script>   ❌
<script src="/src/index.tsx"></script>                ❌
```

### ✅ נכון
```html
<!-- index.html -->
<script type="module" src="/src/index.tsx"></script>  ✅
```

## 💡 טיפ נוסף

אם אתה ב-Windows ויש לך בעיות, נסה:
```bash
# במקום rm -rf
rmdir /s /q node_modules
del package-lock.json

# או השתמש ב-PowerShell
Remove-Item -Recurse -Force node_modules, package-lock.json
```

## 📞 עדיין לא עובד?

שלח לי:
1. את גרסת Node: `node --version`
2. את גרסת npm: `npm --version`
3. צילום מסך של השגיאה
4. את התוכן של `package.json`
5. את מבנה התיקיות: `tree -L 2` (או `dir /s`)

---

**זמן פתרון משוער: 5-10 דקות** ⏱️
