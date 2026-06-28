# מערכת ניהול מאמן כושר

## מה הפרויקט
אפליקציית ניהול למאמן כושר — קובץ HTML בודד (`fitness.html`), עברית RTL, עיצוב כהה.
הנתונים נשמרים ב-**Firebase Firestore** (ענן של Google).

## קבצים
- `fitness.html` — כל האפליקציה (HTML + CSS + JS בקובץ אחד)
- `client.html` — פורטל ללקוחות (לקוח מתחבר עם טלפון + 4 ספרות אחרונות)
- `firestore.rules` — חוקי אבטחת Firestore (מחייב trainerId בכתיבה)
- `architecture-diagram.html` — דיאגרמת ארכיטקטורה

## GitHub
- Repository: `https://github.com/ron11moore11/fitness-trainer`
- אתר חי: `https://ron11moore11.github.io/fitness-trainer/fitness.html`
- כדי לעדכן את האתר אחרי שינוי:
  ```
  git add fitness.html
  git commit -m "תיאור השינוי"
  git push
  ```

## Firebase
- Project: `fitness-trainer-84b8c`
- Console: `https://console.firebase.google.com/project/fitness-trainer-84b8c`
- משתמשים ב-Firestore Compat SDK v9
- Firestore Rules מוגדרים ופעילים (פורסמו 28.6.2026) — מחייבים trainerId בכל כתיבה חוץ מ-exercises

## Collections ב-Firestore
| Collection | תוכן |
|---|---|
| `trainers` | מאמנים — name, phone, pin, isAdmin, createdAt |
| `clients` | לקוחות — name, phone, email, age, goals, notes, trainerId |
| `sessions` | אימונים — clientId, date, time, duration, notes, trainerId |
| `plans` | תוכניות אימון — clientId, name, isTemplate, exercises[], trainerId |
| `payments` | תשלומים — clientId, amount, date, status, package, trainerId |
| `exercises` | בנק תרגילים — name, category, videoUrl (משותף לכל המאמנים) |

## תכונות קיימות
- **לקוחות** — CRUD + דשבורד סיכום
- **לוח זמנים** — לוח שנה חודשי + רשימת אימונים
- **בנק תרגילים** — תרגילים עם קטגוריה + קישור סרטון
- **תוכניות אימון** — תבניות גנריות + תוכניות ללקוחות, ייצוא Word, שליחה לוואטסאפ
- **תשלומים** — מעקב עם סטטוס paid/pending + הנפקת מסמכים דרך Morning
- **Morning (חשבונית ירוקה)** — תשתית API להנפקת חשבונית מס וקבלה (צריך להכניס מפתחות)
- **התחברות מאמנים** — טלפון + סיסמה (4 ספרות שהמאמן בוחר בכניסה ראשונה). שדה `pin` ב-trainers.
- **התחברות לקוחות** — טלפון + 4 ספרות אחרונות של הטלפון (ב-client.html)
- **מרובה מאמנים** — כל מאמן רואה רק את הנתונים שלו (trainerId על כל מסמך). תרגילים משותפים. deleteItem בודק trainerId לפני מחיקה.
- **ניהול מאמנים (admin)** — טאב admin למאמן עם isAdmin=true: הוספה/מחיקה של מאמנים, מיגרציה של נתונים יתומים
- **פורטל לקוחות** — client.html: לוח שנה, אימון חי, גרפי התקדמות. מסנן לפי trainerId.
- **תזכורות** — אימונים 7 ימים קדימה + תזכורת WhatsApp

## עיצוב (CSS tokens)
```
--bg: #1a1a2e  |  --card: #16213e  |  --border: #0f3460
--accent: #e94560  |  --accent2: #a8dadc
```

## דברים שעדיין לא נעשו (המשך אפשרי)
- [x] מסך התחברות / login
- [x] מרובה מאמנים + ניהול admin
- [x] Morning API — תשתית מוכנה, ממתין למפתחות
- [ ] גיבוי ידני — כפתור "ייצא נתונים ל-JSON"
- [ ] שליחת תוכניות מ-WhatsApp עם קובץ מצורף אוטומטית (דורש שרת)
- [ ] דוחות / גרפים (הכנסות לפי חודש, מתאמנים פעילים)
- [ ] אפליקציית מובייל (React Native / PWA)

## מאמנים רשומים
| שם | טלפון | Admin | הערות |
|---|---|---|---|
| דניאל מור | 0504453292 | ✓ | מאמן ראשי |
| ליבנה | 0505100104 | | |

## איך לעבוד עם הקוד
- כל ה-JS נמצא בתוך `<script>` בתחתית הקובץ
- `load('clients')` קורא מה-cache (לא ישירות מ-Firestore)
- `saveItem('clients', obj)` שומר ב-cache + שולח ל-Firestore (מוסיף trainerId אוטומטית)
- `deleteItem('clients', id)` מוחק מ-cache + מ-Firestore (מוודא trainerId לפני מחיקה)
- `loginAsTrainer(doc)` — פונקציית login משותפת (login רגיל + כניסה ראשונה)
- כל פונקציה שמשנה נתונים היא `async`
- initDB מסנן נתונים לפי trainerId (חוץ מ-exercises שמשותפים)
