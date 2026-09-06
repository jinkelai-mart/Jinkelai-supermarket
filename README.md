# Jinkelai Shop — Deploy លើ Vercel + Firebase

គម្រោងនេះជាហាងអនឡាញ (Static Website) ភ្ជាប់ទៅ **Firebase Firestore + Storage** និង **Telegram Bot**.

## ឯកសារនៅក្នុងគម្រោង

```
jinkelai-shop/
├── index.html      ← ទំព័រហាង (អតិថិជន)
├── admin.html      ← ទំព័រ Admin
├── logo.png
├── vercel.json
└── README.md
```

---

## 1. ត្រៀម Firebase (សំខាន់!)

### 1.1 បើក Firebase Console
1. ចូល [https://console.firebase.google.com](https://console.firebase.google.com)
2. ជ្រើស Project: **jinkelai-supermarket** (ឬបង្កើតថ្មី)

### 1.2 Firestore Database
- បង្កើត **Firestore Database** (Production mode)
- ទៅ **Rules** ហើយដាក់ច្បាប់ខាងក្រោម (សម្រាប់ដំណើរការដំបូង):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /products/{docId} {
      allow read: if true;
      allow write: if true;   // សម្រាប់ Admin សាមញ្ញ (ក្រោយមកអាចបន្ថែម Auth)
    }
    match /orders/{docId} {
      allow read, write: if true;
    }
  }
}
```

> ⚠️ ច្បាប់ខាងលើអនុញ្ញាតឱ្យអ្នកគ្រប់គ្នាសរសេរបាន។ ក្រោយមកគួរបន្ថែម Firebase Authentication សម្រាប់ Admin។

### 1.3 Storage Rules
- ទៅ **Storage** → **Rules**:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /products/{allPaths=**} {
      allow read: if true;
      allow write: if true;   // សម្រាប់ upload រូបពី Admin
    }
  }
}
```

### 1.4 Authentication (ស្រេចចិត្ត)
Admin បច្ចុប្បន្នប្រើ localStorage (email/pass នៅក្នុង browser)។  
ប្រសិនបើចង់ប្រើ Firebase Auth ពិតប្រាកដ ត្រូវកែកូដបន្ថែម។

### 1.5 Telegram Bot
Token និង Chat ID មានរួចហើយនៅក្នុង `index.html`។  
ត្រូវប្រាកដថា Bot អាចផ្ញើសារទៅ Chat ID នោះបាន។

---

## 2. Deploy លើ Vercel

### វិធីងាយបំផុត (แนะนำ)

1. ចូល [https://vercel.com](https://vercel.com) → Sign in (GitHub/Google)
2. ចុច **Add New Project**
3. អូសទាំងថត `jinkelai-shop` ទៅលើ (ឬភ្ជាប់ GitHub repo)
4. Framework Preset: **Other**
5. ចុច **Deploy**

### វិធីប្រើ GitHub (ល្អជាង)

```bash
# នៅក្នុងថត jinkelai-shop
git init
git add .
git commit -m "Initial Jinkelai Shop"
# បង្កើត repo នៅ GitHub រួច
git remote add origin https://github.com/YOUR_USERNAME/jinkelai-shop.git
git push -u origin main
```

បន្ទាប់មកនៅ Vercel → Import Git Repository → ជ្រើស repo នេះ → Deploy។

### បន្ទាប់ពី Deploy រួច
- URL ហាង: `https://your-project.vercel.app`
- Admin: `https://your-project.vercel.app/admin.html`  
  (ឬ `/admin` បើប្រើ vercel.json)

**Login Admin ដំបូង:**
- Email: `admin@gmail.com`
- Password: `1234`  
  (អាចប្តូរបាននៅក្នុង Admin Dashboard)

---

## 3. ការកំណត់ Firebase នៅក្នុងកូដ

Firebase Config មានរួចហើយនៅក្នុងទាំង `index.html` និង `admin.html`:

```js
const firebaseConfig = {
  apiKey: "AIzaSyBYgj8O40QZgGi9asF5CpzFBOcetNkIFJE",
  authDomain: "jinkelai-supermarket.firebaseapp.com",
  projectId: "jinkelai-supermarket",
  storageBucket: "jinkelai-supermarket.firebasestorage.app",
  messagingSenderId: "622852418497",
  appId: "1:622852418497:web:bd84d5cdf8677f00c35d40",
  measurementId: "G-VFQJ237VE2"
};
```

ប្រសិនបើអ្នកបង្កើត Project Firebase ថ្មី ត្រូវជំនួសតម្លៃទាំងនេះ។

---

## 4. បញ្ហាដែលជួបញឹកញាប់

| បញ្ហា | ដំណោះស្រាយ |
|------|-------------|
| ទំនិញមិនបង្ហាញ | ពិនិត្យ Firestore Rules (allow read) |
| Upload រូបមិនបាន | ពិនិត្យ Storage Rules |
| Telegram មិនទទួល | ពិនិត្យ Bot Token + Chat ID |
| CORS / Network Error | ប្រាកដថា Firebase Project ត្រឹមត្រូវ |

---

## 5. របៀបប្រើ

1. បើក `admin.html` → Login → បន្ថែមទំនិញ (រូប + ស្តុក + តម្លៃ)
2. បើក `index.html` → អតិថិជនជ្រើសទំនិញ → បញ្ជាទិញ
3. ការបញ្ជាទិញនឹងចូល Firestore + ផ្ញើទៅ Telegram

សំណួរ ឬចង់កែបន្ថែម សូមប្រាប់ខ្ញុំ!
