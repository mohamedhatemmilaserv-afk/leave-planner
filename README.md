# إدارة الورديات والإجازات | صيدليات المتحدة

تطبيق ويب عربي لإدارة جدول الموظفين والإجازات وتغطية الورديات الأسبوعية من السبت إلى الخميس. الواجهة ثابتة، وتُحفظ بيانات الفرع في Firebase Cloud Firestore مع تحديث مباشر بين الأجهزة.

## المزايا

- لوحة تغطية أسبوعية تعرض العجز في كل وردية.
- إدارة الموظفين والورديات وطلبات الإجازة.
- تصدير واستيراد النسخ الاحتياطية، وتصدير الجدول إلى CSV والطباعة.
- تسجيل دخول Firebase؛ لا يوجد تسجيل حسابات عام.
- مزامنة بيانات الفرع لحظياً عبر Firestore.
- واجهة عربية متجاوبة مع الوضعين الفاتح والداكن.

## تشغيل محلي

لتشغيل الموقع محلياً عبر Firebase Hosting:

```bash
npm run serve
```

يلزم إعداد Firebase قبل تسجيل الدخول. راجع [دليل إعداد Firebase](./FIREBASE_GUIDE.md).

## إعداد Firebase المطلوب

1. فعّل **Email/Password** في Firebase Authentication.
2. أنشئ حسابات العاملين المصرّح لهم من لوحة Firebase؛ التطبيق لا يتيح إنشاء حسابات ذاتياً.
3. أنشئ Cloud Firestore وانشر قواعد [firestore.rules](./firestore.rules). القراءة والكتابة مقصورة على المستخدمين المسجلين.
4. تحقق من إعدادات المشروع في [firebase-config.js](./firebase-config.js)، وأضف نطاق الاستضافة إلى Authorized domains في Firebase Authentication.

يُحفظ سجل الفرع في `branches/{branchId}` كمستند يحتوي حالة التطبيق كاملة. إذا كان مستند الفرع موجوداً، يحتفظ التطبيق ببياناته؛ أما قاعدة البيانات الجديدة فتبدأ بلا موظفين ويجب إضافة الفريق أو استيراد نسخة احتياطية.

> إعداد Firebase الظاهر في المتصفح ليس كلمة مرور. لا تفتح Firestore للعامة؛ قواعد الأمان والمصادقة هما ما يحمي بيانات الموظفين.

## نشر GitHub Pages

المستودع عام، وGitHub Pages مفعّل للنشر عبر GitHub Actions. يتولى سير العمل في `.github/workflows/pages.yml` نشر `index.html` عند الدفع إلى `main`.

رابط الموقع: <https://mohamedhatemmilaserv-afk.github.io/leave-planner/>

صفحة الموقع عامة، لكن الوصول إلى بيانات التطبيق يتطلب حساب Firebase مصرحاً به.

## الملفات الأساسية

- [index.html](./index.html) — التطبيق والواجهة.
- [firebase-config.js](./firebase-config.js) — إعداد Firebase العام للعميل.
- [firestore.rules](./firestore.rules) — قواعد وصول Firestore.
- [FIREBASE_GUIDE.md](./FIREBASE_GUIDE.md) — خطوات إعداد الحسابات وقاعدة البيانات والنشر.
