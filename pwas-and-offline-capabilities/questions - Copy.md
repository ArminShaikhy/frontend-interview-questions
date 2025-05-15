### 1. چگونه یک برنامه React را برای رعایت استانداردهای اپلیکیشن وب پیش‌رونده (PWA) طراحی می‌کنید؟

**پاسخ:**
من با اطمینان از رعایت معیارهای اصلی PWA شروع می‌کنم:

- **رابط کاربری پاسخ‌گو**: طراحی‌شده با رویکرد موبایل‌اول با استفاده از چارچوب‌های CSS یا کوئری‌های رسانه‌ای.
- **HTTPS**: برای سرویس‌ورکرها و امنیت ضروری است.
- **مانیفست اپلیکیشن وب**: متادیتای برنامه (نام، آیکون‌ها، رنگ تم) را تعریف می‌کند.
- **سرویس‌ورکر**: کش و پشتیبانی آفلاین را فعال می‌کند.

در `create-react-app`، بسیاری از موارد پیش‌فرض آماده است. من `manifest.json` را سفارشی کرده و سرویس‌ورکر را ثبت می‌کنم:

```jsx
// index.js
import * as serviceWorkerRegistration from './serviceWorkerRegistration';
serviceWorkerRegistration.register();

همچنین از Lighthouse برای ممیزی و تأیید انطباق استفاده می‌کنم.


2. رویکرد شما برای پیاده‌سازی کش آفلاین در یک برنامه React با استفاده از سرویس‌ورکرها چیست؟
پاسخ:من از یک سرویس‌ورکر برای کش دارایی‌های ضروری (HTML، CSS، JS) و به‌صورت اختیاری پاسخ‌های API استفاده می‌کنم:
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open('static-v1').then(cache =>
      cache.addAll(['/index.html', '/main.css', '/main.js'])
    )
  );
});

برای کش API:
self.addEventListener('fetch', event => {
  if (event.request.url.includes('/api/')) {
    event.respondWith(
      fetch(event.request).catch(() => caches.match(event.request))
    );
  }
});

این اطمینان می‌دهد که پوسته برنامه و برخی داده‌های پویا به‌صورت آفلاین در دسترس هستند.


3. چگونه اعلان‌های فشاری را در یک PWA React ادغام می‌کنید؟
پاسخ:

درخواست مجوز با استفاده از API اعلان‌ها.
ثبت برای فشار با API فشار و سرویس‌ورکر.
اشتراک در سرویس فشار (مثل Firebase Cloud Messaging یا سرور VAPID).
مدیریت فشار در سرویس‌ورکر:

self.addEventListener('push', event => {
  const data = event.data.json();
  self.registration.showNotification(data.title, {
    body: data.body,
    icon: '/icons/icon-192.png'
  });
});

در سمت React، منطق اشتراک را ادغام کرده و آن را به بک‌اند برای ذخیره‌سازی ارسال می‌کنم.


4. استراتژی شما برای اطمینان از عملکرد یک برنامه React به‌صورت آفلاین با داده‌های پویا چیست؟
پاسخ:

کش داده‌های API پویا با استفاده از IndexedDB (از طریق کتابخانه‌هایی مثل idb) یا localStorage.
استفاده از استراتژی "ابتدا کش، سپس شبکه" یا "شبکه با بازگشت به کش".
نگهداری یک صف از عملیات نوشتن (مثل ارسال فرم‌ها) و همگام‌سازی آن‌ها هنگام آنلاین شدن.

مثال:
// بازگشت به کش در صورت شکست
const getData = async () => {
  try {
    const res = await fetch('/api/داده');
    const json = await res.json();
    localStorage.setItem('داده_کش', JSON.stringify(json));
    return json;
  } catch {
    return JSON.parse(localStorage.getItem('داده_کش'));
  }
};



5. چگونه قابلیت‌های آفلاین یک PWA React را در دستگاه‌ها و مرورگرهای مختلف آزمایش می‌کنید؟
پاسخ:

استفاده از Chrome DevTools → تب Application → Service Workers → شبیه‌سازی آفلاین.
انجام ممیزی‌های Lighthouse برای پشتیبانی آفلاین.
غیرفعال کردن دستی Wi-Fi و آزمایش جریان‌های بحرانی.
استفاده از BrowserStack یا دستگاه‌های واقعی برای تست بین‌مرورگری.
آزمایش شروع سرد (کاملاً آفلاین) و شروع گرم (جلسه کش‌شده).



6. رویکرد شما برای بهینه‌سازی قابلیت نصب یک PWA React (مثل فایل‌های مانیفست) چیست؟
پاسخ:

اطمینان از معتبر بودن manifest.json و لینک شدن در index.html:

<link rel="manifest" href="/manifest.json" />


شامل فیلدهای مورد نیاز: name، short_name، start_url، display، icons و theme_color.

{
  "name": "برنامه من",
  "short_name": "برنامه",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}


نمایش اعلان‌های نصب با استفاده از رویداد beforeinstallprompt.



7. چگونه مدیریت حالت را در یک PWA React طی اختلالات شبکه مدیریت می‌کنید؟
پاسخ:

استفاده از استراتژی حالت محلی‌اول با کتابخانه‌هایی مثل Zustand، Redux Persist یا Dexie.js برای همگام‌سازی با IndexedDB.
تشخیص آنلاین/آفلاین با استفاده از navigator.onLine یا window.addEventListener('online').
صف‌بندی اقدامات انجام‌شده در حالت آفلاین و پردازش مجدد آن‌ها هنگام اتصال مجدد.

مثال:
window.addEventListener('آنلاین', () => {
  // پردازش اقدامات در صف
});

هدف، یکنواختی نهایی بدون مسدود کردن رابط کاربری است.


8. فرآیند شما برای اشکال‌زدایی مشکلات سرویس‌ورکر در یک برنامه React چیست؟
پاسخ:

باز کردن DevTools → Application → Service Workers برای بررسی چرخه حیات.
استفاده از console.log() داخل سرویس‌ورکر و بررسی DevTools → Console → Service Worker.
اعتبارسنجی فعال شدن رویدادهای fetch و install.
پاک‌سازی کش‌های قدیمی در طول ارتقاء نسخه.
استفاده از Lighthouse برای شناسایی استراتژی‌های کش شکسته یا مشکلات ثبت.

self.addEventListener('activate', event => {
  // پاک‌سازی نسخه‌های قدیمی کش
});



9. چگونه تأثیر عملکرد ویژگی‌های PWA را در یک برنامه React اندازه‌گیری می‌کنید؟
پاسخ:

استفاده از Lighthouse برای اندازه‌گیری عملکرد، انطباق PWA و زمان تعاملی (TTI).
نظارت بر زمان‌بندی سرویس‌ورکر با API Performance.
ردیابی نرخ موفقیت کش و بازگشت درخواست‌های شبکه با استفاده از لاگ‌گیری سفارشی یا تحلیل‌ها (مثل Sentry، LogRocket).
استفاده از web-vitals برای ردیابی معیارهایی مثل FCP، LCP و CLS.



10. چگونه یک تیم را برای ساخت و نگهداری یک PWA با React آموزش می‌دهید؟
پاسخ:

ایجاد یک سند مشترک یا ویکی داخلی با استانداردهای PWA، الگوها و نمونه‌های کد.
برگزاری کارگاه‌های زنده یا راهنمایی‌ها درباره:
سرویس‌ورکرها
استراتژی‌های کش
تجربه کاربری مناسب آفلاین


استفاده از Lighthouse به‌عنوان چک‌لیست.
ارائه مخازن قالب یا بویلرپلیت‌ها.
بررسی کدهای خاص PWA در PRها برای اطمینان از بهترین روش‌ها.



