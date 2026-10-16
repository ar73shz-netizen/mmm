# Rasa used phone

اپلیکیشن وب (PWA) فارسی و راست‌چین برای ثبت ورود/خروج گوشی، موجودی، فیلتر و گزارش سود ماهانه و سالانه با تاریخ شمسی.

## ۱) انتشار روی GitHub
1. یک مخزن جدید بسازید و همه فایل‌های این پوشه را (با پوشه `.github`) در شاخه `main` آپلود کنید.
2. در مخزن: **Settings → Pages → Source: GitHub Actions** را انتخاب کنید.
3. بعد از چند دقیقه آدرس `https://USERNAME.github.io/REPO/` فعال می‌شود.

## ۲) تبدیل به اپلیکیشن
- **ساده‌ترین راه (PWA):** آدرس بالا را در کروم گوشی باز کنید → منو → *Add to Home screen / نصب برنامه*. در آیفون: Safari → Share → Add to Home Screen.
- **فایل APK:** آدرس سایت را در https://www.pwabuilder.com وارد کنید و بسته Android را بگیرید.
- **Capacitor:** `npm i @capacitor/core @capacitor/cli @capacitor/android` ، سپس `npx cap init` (webDir = `.`) ، `npx cap add android` ، `npx cap open android`.

## ۳) فعال‌سازی حالت آنلاین (چند کارمند، همگام‌سازی لحظه‌ای)
بدون این مرحله برنامه به صورت محلی (روی همان دستگاه) کار می‌کند.
1. در https://console.firebase.google.com یک پروژه بسازید.
2. **Build → Authentication → Sign-in method → Email/Password** را فعال کنید و در تب Users برای هر کارمند یک ایمیل و رمز بسازید.
3. **Build → Firestore Database → Create database**. سپس در تب Rules این قوانین را بگذارید و Publish کنید:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```
4. **Project settings → Your apps → Web (</>)** را بزنید و `firebaseConfig` را کپی کنید.
5. در برنامه: **تنظیمات → همگام‌سازی آنلاین** آن را بچسبانید و «اتصال» بزنید. همین کار را روی دستگاه هر کارمند انجام دهید.
6. در Authentication → Settings → Authorized domains، دامنه `USERNAME.github.io` را اضافه کنید.

با ورود، خروج یا ویرایش هر گوشی، بقیه کارکنان فوراً اعلان می‌بینند.

## ۴) پشتیبان‌گیری
**تنظیمات → دانلود پشتیبان** یک فایل JSON می‌دهد. بعد از هر آپدیت یا روی دستگاه جدید، با **بازیابی از فایل** اطلاعات برمی‌گردد. قبل از آپدیت حتماً پشتیبان بگیرید.

## ۵) ساخت APK اندروید با GitHub
فایل `.github/workflows/apk.yml` را در **Actions → Build Android APK → Run workflow** اجرا کنید. بعد از سبز شدن، در صفحه Summary همان اجرا و بخش **Artifacts** فایل `rasa-used-phone-apk` را دانلود کنید (داخل zip فایل `app-debug.apk` است). فایل‌های `icon-1024.png` و `icon-fg-1024.png` برای ساخت لوگوی برنامه لازم‌اند و باید در ریشه مخزن باشند.
