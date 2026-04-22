# מערכת ניהול מאמן כושר

## מה הפרויקט
אפליקציית ניהול למאמן כושר — קובץ HTML בודד (`fitness.html`), עברית RTL, עיצוב כהה.
הנתונים נשמרים ב-**Firebase Firestore** (ענן של Google).

## קבצים
- `fitness.html` — כל האפליקציה (HTML + CSS + JS בקובץ אחד)
- `tictactoe.html` — משחק איקס עיגול (פרויקט ראשון, לא קשור)

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
- **חשוב:** Firestore פועל ב-test mode — תוקף 30 יום. אחרי זה צריך לעדכן Rules.

## Collections ב-Firestore
| Collection | תוכן |
|---|---|
| `clients` | לקוחות — name, phone, email, age, goals, notes |
| `sessions` | אימונים — clientId, date, time, duration, notes |
| `plans` | תוכניות אימון — clientId, name, isTemplate, exercises[] |
| `payments` | תשלומים — clientId, amount, date, status, package |
| `exercises` | בנק תרגילים — name, category, videoUrl |

## תכונות קיימות
- **לקוחות** — CRUD + דשבורד סיכום
- **לוח זמנים** — לוח שנה חודשי + רשימת אימונים
- **בנק תרגילים** — תרגילים עם קטגוריה + קישור סרטון
- **תוכניות אימון** — תבניות גנריות + תוכניות ללקוחות, ייצוא Word, שליחה לוואטסאפ
- **תשלומים** — מעקב עם סטטוס paid/pending + הנפקת מסמכים דרך Morning
- **Morning (חשבונית ירוקה)** — תשתית API להנפקת חשבונית מס וקבלה (צריך להכניס מפתחות)
- **התחברות** — מסך סיסמה (SHA-256 hash, localStorage)
- **תזכורות** — אימונים 7 ימים קדימה + תזכורת WhatsApp

## עיצוב (CSS tokens)
```
--bg: #1a1a2e  |  --card: #16213e  |  --border: #0f3460
--accent: #e94560  |  --accent2: #a8dadc
```

## דברים שעדיין לא נעשו (המשך אפשרי)
- [x] מסך התחברות / login
- [x] Morning API — תשתית מוכנה, ממתין למפתחות
- [ ] גיבוי ידני — כפתור "ייצא נתונים ל-JSON"
- [ ] שליחת תוכניות מ-WhatsApp עם קובץ מצורף אוטומטית (דורש שרת)
- [ ] דוחות / גרפים (הכנסות לפי חודש, מתאמנים פעילים)
- [ ] אפליקציית מובייל (React Native / PWA)

## איך לעבוד עם הקוד
- כל ה-JS נמצא בתוך `<script>` בתחתית הקובץ
- `load('clients')` קורא מה-cache (לא ישירות מ-Firestore)
- `saveItem('clients', obj)` שומר ב-cache + שולח ל-Firestore
- `deleteItem('clients', id)` מוחק מ-cache + מ-Firestore
- כל פונקציה שמשנה נתונים היא `async`
