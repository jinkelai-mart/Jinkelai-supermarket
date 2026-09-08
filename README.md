# Jinkelai Shop — Deploy លើ Vercel + Firebase

គម្រោងនេះជាហាងអនឡាញ (Static Website) ភ្ជាប់ទៅ **Firebase Firestore + Authentication** និង **Telegram Bot**.

## ឯកសារត្រឹមត្រូវ

```
jinkelai-shop/
├── index.html                     ← ទំព័រហាង (អតិថិជន)
├── jinkelai-panel-x7k9m.html      ← ទំព័រ Admin (ឈ្មោះលាក់)
├── logo.png
├── robots.txt
├── vercel.json
└── README.md
```

> 🔒 **Admin ប្រើឈ្មោះវែង** ដើម្បីកាត់បន្ថយហានិភ័យ  
> មិនមាន route `/admin` ងាយស្រួលទេ។

---

## 1. ត្រៀម Firebase

### Firestore Rules (ណែនាំ)
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /products/{docId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /orders/{docId} {
      allow read, write: if true;
    }
    match /settings/{docId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

### Authentication
- បើក **Email/Password** នៅ Firebase Console
- បង្កើត User សម្រាប់ Admin

---

## 2. Deploy លើ Vercel

1. Import repo ពី GitHub
2. Framework Preset: **Other**
3. Deploy

### URL បន្ទាប់ពី Deploy
- **ហាង**: `https://your-project.vercel.app`
- **Admin**: `https://your-project.vercel.app/jinkelai-panel-x7k9m.html`

> រក្សា URL Admin នេះឲ្យសម្ងាត់ កុំផ្សព្វផ្សាយ។

---

## 3. ការសម្អាត GitHub

លុបឯកសារដែលមិនចាំបាច់ចេញ:
- ❌ `admin.html` (បើមាន)

ទុកតែឯកសារខាងលើ។

---

## 4. របៀបប្រើ

1. បើក URL Admin → Login តាម Firebase Auth
2. បន្ថែមទំនិញ / គ្រប់គ្រង order
3. អតិថិជនប្រើទំព័រហាងធម្មតា
