# 🔧 מדריך פתרון בעיות

## שגיאת MIME Type

### הבעיה
```
Failed to load module script: Expected a JavaScript module script 
but the server responded with a MIME type of "application/octet-stream"
```

### הפתרונות

#### 1. ניקוי מטמון ו-node_modules
```bash
# מחיקת node_modules ו-cache
rm -rf node_modules package-lock.json
rm -rf .vite

# התקנה מחדש
npm install

# הפעלה
npm run dev
```

#### 2. בדיקת מבנה הקבצים
ודא שהמבנה נכון:
```
your-project/
├── src/
│   ├── index.tsx       ✅ (לא index.ts)
│   ├── App.tsx
│   └── styles/
│       └── index.css
├── index.html          ✅ (בשורש הפרויקט)
├── vite.config.ts
└── package.json
```

#### 3. עדכון index.html
ודא שה-script tag נכון:
```html
<!-- ✅ נכון -->
<script type="module" src="/src/index.tsx"></script>

<!-- ❌ לא נכון -->
<script type="module" src="/index.tsx"></script>
<script src="/src/index.tsx"></script>
```

#### 4. בדיקת vite.config.ts
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

#### 5. עדכון package.json
ודא שיש:
```json
{
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build"
  }
}
```

---

## שגיאות TypeScript

### Cannot find module '@/...'

**פתרון:**
```json
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

```typescript
// vite.config.ts
import path from 'path';

export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  }
});
```

---

## שגיאות Gemini API

### API Key חסר

**שגיאה:**
```
Gemini API key is not configured
```

**פתרון:**
1. צור קובץ `.env.local`:
```bash
VITE_GEMINI_API_KEY=your_actual_api_key_here
```

2. אל תשכח את הקידומת `VITE_`!

3. הפעל מחדש את השרת:
```bash
npm run dev
```

### 403 Forbidden

**גורמים אפשריים:**
- API Key לא תקין
- חריגה ממכסה
- IP חסום

**פתרון:**
```bash
# בדוק API Key
echo $VITE_GEMINI_API_KEY

# צור key חדש בכתובת:
# https://makersuite.google.com/app/apikey
```

---

## בעיות Build

### Build נכשל

```bash
# ניקוי מלא
npm run clean
rm -rf dist

# Build מחדש
npm install
npm run build
```

### Bundle גדול מדי

**אופטימיזציה:**
```typescript
// vite.config.ts
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'react-vendor': ['react', 'react-dom'],
          'ui-vendor': ['lucide-react']
        }
      }
    }
  }
});
```

---

## בעיות סגנון (CSS)

### Tailwind לא עובד

**פתרון 1: CDN**
```html
<!-- index.html -->
<script src="https://cdn.tailwindcss.com"></script>
```

**פתרון 2: התקנה מלאה**
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```javascript
// tailwind.config.js
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

---

## בעיות פונטים

### פונט עברי לא נטען

**בדיקה:**
1. פתח Developer Tools (F12)
2. לך ל-Network
3. סנן Fonts
4. חפש שגיאות 404

**פתרון:**
```html
<!-- index.html -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Frank+Ruhl+Libre:wght@400;700&display=swap" rel="stylesheet">
```

---

## בעיות ביצועים

### האפליקציה איטית

**דיאגנוסטיקה:**
```bash
npm run build
npm run preview

# ניתוח bundle
npx vite-bundle-visualizer
```

**אופטימיזציות:**
1. השתמש ב-`useMemo` ו-`useCallback`
2. Code splitting
3. Lazy loading:
```typescript
const HeavyComponent = lazy(() => import('./HeavyComponent'));
```

---

## בעיות RTL

### טקסט לא מיושר ימינה

**פתרון:**
```html
<!-- index.html -->
<html lang="he" dir="rtl">
```

```css
/* index.css */
* {
  direction: rtl;
}
```

---

## פקודות שימושיות

```bash
# בדיקת תקינות
npm run type-check

# ניקוי מלא
npm run clean && npm install

# עדכון תלויות
npm update

# בדיקת גרסאות
npm outdated

# תיקון ESLint אוטומטי
npm run lint -- --fix

# פורמט קוד
npm run format
```

---

## דיבאג מתקדם

### Enable Verbose Logging
```bash
# Windows
set DEBUG=vite:* && npm run dev

# Linux/Mac
DEBUG=vite:* npm run dev
```

### Chrome DevTools
1. פתח F12
2. לך ל-Sources
3. הפעל breakpoints
4. בדוק Network tab לבעיות טעינה

---

## עזרה נוספת

אם הבעיה נמשכת:

1. ✅ בדוק את [Issues ב-GitHub](https://github.com/yourrepo/issues)
2. ✅ חפש ב-[Stack Overflow](https://stackoverflow.com/questions/tagged/vite)
3. ✅ שאל ב-[Vite Discord](https://chat.vitejs.dev)
4. ✅ קרא [Vite Docs](https://vitejs.dev)

---

## Checklist לפני פנייה לעזרה

- [ ] ניקוי node_modules והתקנה מחדש
- [ ] בדיקת console ל-errors
- [ ] בדיקת Network tab
- [ ] נסיתי בדפדפן אחר
- [ ] בדיקת גרסאות Node/npm
- [ ] קריאת error message בעיון
- [ ] חיפוש בגוגל
- [ ] בדיקת .env.local
