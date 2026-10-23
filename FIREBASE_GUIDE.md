# إعداد Firebase | إدارة الورديات والإجازات

يستخدم التطبيق Firebase Authentication لتسجيل الدخول وCloud Firestore لحفظ بيانات الفرع ومزامنتها. صفحة GitHub Pages نفسها عامة؛ لذلك يجب إكمال خطوات المصادقة والقواعد قبل مشاركة الرابط.

## 1. تفعيل تسجيل الدخول

1. افتح [Firebase Console](https://console.firebase.google.com/) واختر المشروع `leave-planner-155bf`.
2. اذهب إلى **Authentication → Sign-in method** وفعّل **Email/Password**.
3. من **Authentication → Settings → Authorized domains** أضف نطاق الموقع المنشور، مثلاً:
   `mohamedhatemmilaserv-afk.github.io`
4. من **Authentication → Users** أنشئ حساباً لكل شخص مصرح له. لا يوجد تسجيل حساب ذاتي داخل التطبيق.
5. شارك بيانات الدخول عبر قناة آمنة، وليس داخل مستودع GitHub.

## 2. تأمين Cloud Firestore

تأكد من إنشاء قاعدة بيانات Cloud Firestore، ثم انشر قواعد المستودع. القواعد تمنع القراءة والكتابة من دون جلسة Firebase صالحة:

```powershell
npx firebase-tools login
npx firebase-tools deploy --only firestore:rules
```

المشروع مرتبط افتراضياً بـ `leave-planner-155bf` عبر `.firebaserc`. راجع [firestore.rules](./firestore.rules) قبل النشر، ولا تستبدل شرط تسجيل الدخول بقواعد الاختبار أو `allow read, write: if true`.

## 3. إعداد التطبيق

إعداد الويب الحالي موجود في [firebase-config.js](./firebase-config.js). قيم Firebase التي تبدأ بـ API key وApp ID هي إعدادات عميل عامة وليست كلمات مرور؛ حماية البيانات تعتمد على Authentication وقواعد Firestore.

يمكن تغيير إعدادات المشروع من التطبيق بعد تسجيل الدخول. تُحفظ هذه التغييرات في المتصفح الحالي، أما النشر لجميع المستخدمين فيتطلب تحديث `firebase-config.js` ورفع التعديل إلى المستودع.

## 4. بنية البيانات والسلوك

- يحفظ التطبيق حالة الفرع في المستند `branches/{branchId}`.
- تضم الحالة الموظفين والورديات والإجازات والحدود الدنيا للتغطية.
- عند وجود مستند الفرع، يحمّله التطبيق كما هو؛ لا تُستبدل بياناته ببيانات ابتدائية.
- عند عدم وجود المستند، ينشئ التطبيق حالة فارغة. أضف الموظفين يدوياً أو استورد نسخة JSON احتياطية.
- تُحفظ التعديلات في Firestore وتظهر للمستخدمين المسجلين الآخرين عبر الاستماع اللحظي.
- يحتفظ Firestore بذاكرة مؤقتة على الجهاز للعمل دون اتصال، وقد يحتاج تفعيل التخزين غير المتصل في إعدادات المتصفح.

> البيانات القديمة التي كانت مضمنة مباشرة في ملفات HTML أزيلت حتى لا تُكشف من مصدر صفحة عامة. إذا كانت بيانات الفريق موجودة بالفعل في مستند Firestore، فستبقى كما هي. خذ نسخة احتياطية قبل تغيير القواعد أو المشروع.

## 5. النشر على GitHub Pages

سير العمل في `.github/workflows/pages.yml` ينشر ملفات الموقع تلقائياً عند الدفع إلى فرع `main`.

1. افتح المستودع على GitHub ثم **Settings → Pages**.
2. اختر **GitHub Actions** كمصدر للنشر.
3. انتظر نجاح سير العمل `Deploy GitHub Pages`.
4. افتح `https://mohamedhatemmilaserv-afk.github.io/leave-planner/` وسجّل الدخول بحساب Firebase المصرّح به.

إذا لم تسمح خطة GitHub بنشر Pages من مستودع خاص، استخدم Firebase Hosting:

```powershell
npx firebase-tools deploy --only hosting
```

نشر ملفات الاستضافة لا ينشئ حسابات Firebase Authentication ولا يفتح قواعد Firestore تلقائياً؛ يجب إعداد الحسابات والقواعد أولاً.
