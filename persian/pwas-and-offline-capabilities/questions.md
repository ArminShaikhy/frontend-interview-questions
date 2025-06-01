---
title: PWA ها و قابلیت‌های آفلاین
---

<link rel="stylesheet" href="{{ site.baseurl }}/assets/css/persian.css">

### 1. چطور یک برنامه React طراحی می‌کنید که با استانداردهای PWA مطابقت داشته باشه؟

**پاسخ:**  
من مطمئن می‌شم که شرایط اصلی PWA رعایت شده:

- **رابط کاربری ریسپانسیو** : طراحی موبایل-اول با CSS یا media query.
- **HTTPS** : الزامی برای فعال‌سازی service worker و امنیت.
- **Web App Manifest** : مشخصات اپ مثل اسم، آیکون، رنگ.
- **Service Worker** : برای کش و پشتیبانی آفلاین.

در `create-react-app` خیلیاش آماده‌ست. من `manifest.json` رو شخصی‌سازی می‌کنم و service worker رو رجیستر می‌کنم:

```
// index.js
import * as serviceWorkerRegistration from './serviceWorkerRegistration';
serviceWorkerRegistration.register();
```

همچنین از Lighthouse برای بررسی و تایید استفاده می‌کنم.

<br />

### 2. چطور از service worker برای کش کردن آفلاین در React استفاده می‌کنید؟

**پاسخ:**  
از یک service worker استفاده می‌کنم تا فایل‌های اصلی (HTML, CSS, JS) و در صورت نیاز API ها رو کش کنم:

```
// نصب اولیه و کش فایل‌ها
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open('static-v1').then(cache =>
      cache.addAll(['/index.html', '/main.css', '/main.js'])
    )
  );
});
```

برای کش کردن API:

```
// واکشی با fallback به کش
self.addEventListener('fetch', event => {
  if (event.request.url.includes('/api/')) {
    event.respondWith(
      fetch(event.request).catch(() => caches.match(event.request))
    );
  }
});
```

<br />

### 3. چطور نوتیفیکیشن پوش رو توی PWA با React پیاده‌سازی می‌کنید؟

**پاسخ:**  

1. گرفتن اجازه با Notifications API.
2. ثبت در Push API و service worker.
3. سابسکرایب به سرویس پوش مثل FCM یا VAPID.
4. هندل کردن پوش در service worker:

```
// گوش دادن به نوتیفیکیشن جدید
self.addEventListener('push', event => {
  const data = event.data.json();
  self.registration.showNotification(data.title, {
    body: data.body,
    icon: '/icons/icon-192.png'
  });
});
```

در React من سابسکریپشن رو ذخیره می‌کنم و به backend می‌فرستم.

<br />

### 4. چطور مطمئن می‌شید که React app شما در حالت آفلاین با داده داینامیک هم کار می‌کنه؟

**پاسخ:**  

- کش کردن داده API با IndexedDB (مثل `idb`) یا localStorage.
- استفاده از استراتژی "cache then network" یا "network fallback to cache".
- نگهداری صف برای عملیات نوشتاری و سینک کردن بعد از وصل شدن.

```
// استفاده از کش هنگام قطع بودن شبکه
const getData = async () => {
  try {
    const res = await fetch('/api/data');
    const json = await res.json();
    localStorage.setItem('cachedData', JSON.stringify(json));
    return json;
  } catch {
    return JSON.parse(localStorage.getItem('cachedData'));
  }
};
```

<br />

### 5. چطور قابلیت آفلاین اپ React PWA رو روی دستگاه‌ها و مرورگرهای مختلف تست می‌کنید؟

**پاسخ:**  

- استفاده از **Chrome DevTools** → بخش Application → حالت آفلاین.
- بررسی با Lighthouse برای تایید.
- خاموش کردن وای‌فای برای تست دستی.
- استفاده از **BrowserStack** یا دستگاه واقعی.
- تست حالت‌های مختلف: cold start (کاملا آفلاین) و warm start (با کش).

<br />

### 6. چطور نصب‌پذیری (Installability) اپ React PWA رو بهینه می‌کنید؟

**پاسخ:**  

- بررسی و اتصال فایل `manifest.json` در `index.html`:

```html
<link rel="manifest" href="/manifest.json" />
```

- داشتن فیلدهای لازم:

```json
{
  "name": "My App",
  "short_name": "App",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

- نمایش prompt نصب با `beforeinstallprompt`.

<br />

### 7. در هنگام قطعی شبکه، چطور مدیریت وضعیت (state management) در React PWA انجام می‌دید؟

**پاسخ:**  

- استفاده از استراتژی local-first با Zustand، Redux Persist یا Dexie.js.
- بررسی وضعیت اتصال با `navigator.onLine`.
- صف کردن عملیات و اجرای آن بعد از reconnect.

```
// بعد از اتصال دوباره، صف رو اجرا کن
window.addEventListener('online', () => {
  // اجرا عملیات در صف
});
```

<br />

### 8. چطور مشکلات مربوط به service worker رو در React دیباگ می‌کنید؟

**پاسخ:**  

1. بررسی در **DevTools → Application → Service Workers**.
2. لاگ گرفتن داخل service worker با `console.log()`.
3. چک کردن اینکه `fetch` و `install` اجرا می‌شن.
4. پاک کردن کش‌های قدیمی هنگام نسخه جدید.
5. استفاده از **Lighthouse** برای بررسی دقیق‌تر.

```
// پاکسازی کش قدیمی
self.addEventListener('activate', event => {
  // حذف نسخه‌های قبلی
});
```

<br />

### 9. چطور تاثیر ویژگی‌های PWA روی عملکرد React app رو اندازه‌گیری می‌کنید؟

**پاسخ:**  

- استفاده از **Lighthouse** برای بررسی TTI، PWA، و performance.
- نظارت بر زمان‌های service worker با Performance API.
- آنالیز cache hit rate و fallbackها با ابزارهایی مثل Sentry یا LogRocket.
- استفاده از `web-vitals` برای متریک‌هایی مثل FCP، LCP، CLS.

<br />

### 10. چطور تیم رو برای توسعه و نگهداری React PWA آموزش می‌دید؟

**پاسخ:**  

- ساخت **مستندات یا ویکی داخلی** با الگوها و best practice ها.
- برگزاری **ورکشاپ زنده** درباره:
  - service worker
  - استراتژی کش
  - تجربه کاربری آفلاین‌پذیر
- استفاده از **Lighthouse** به عنوان چک‌لیست.
- ارائه boilerplate یا ریپوی آماده.
- بررسی کدهای مرتبط با PWA در PR ها.

<br />