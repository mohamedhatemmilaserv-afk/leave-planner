# إعداد GitHub والنشر

المستودع المقصود: `mohamedhatemmilaserv-afk/leave-planner` (خاص).

## نشر صفحة الموقع

1. من إعدادات المستودع افتح **Settings → Pages**.
2. اختر **GitHub Actions** ضمن **Build and deployment**.
3. ادفع التغييرات إلى `main`؛ سير العمل `.github/workflows/pages.yml` سينشر الموقع.
4. رابط الموقع المتوقع:
   `https://mohamedhatemmilaserv-afk.github.io/leave-planner/`

تأكد من أن خطة الحساب تسمح بنشر GitHub Pages من مستودع خاص. إذا لم تسمح، استخدم Firebase Hosting كما هو موضح في [FIREBASE_GUIDE.md](./FIREBASE_GUIDE.md).

الموقع المنشور عام، لكن محتوى قاعدة البيانات لا يظهر إلا بعد تسجيل الدخول بحساب Firebase مصرح به. لا تضع كلمات مرور أو بيانات اعتماد المستخدمين في المستودع.
