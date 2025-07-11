---
title: CI/CD و ابزارهای نظارتی
---

<link rel="stylesheet" href="{{ site.baseurl }}/assets/css/persian.css">

### 1. چگونه یک خط لوله CI/CD برای یک برنامه React طراحی می‌کنید تا حلقه‌های بازخورد سریع و استقرارهای قابل اعتماد را تضمین کند؟

- **CI:** بررسی کد (Lint) → تست‌های واحد → ساخت
- **CD:** استقرار به پیش‌نمایش (مثل Vercel/Netlify) → اجرای تست‌های E2E → ارتقاء به مرحله‌بندی/تولید
- استفاده از **GitHub Actions**، **CircleCI** یا **GitLab CI**.

```yaml
# نمونه GitHub Actions
jobs:
  build-test:
    steps:
      - run: npm ci
      - run: npm run lint && npm test
  deploy:
    steps:
      - run: npm run build
      - run: npm run deploy:staging
```

<br />

### 2. رویکرد شما برای خودکارسازی استقرار یک برنامه React به چندین محیط (مثل توسعه، مرحله‌بندی، تولید) چیست؟

- استفاده از **پیکربندی خاص محیط** (`.env.dev`، `.env.staging`).
- استفاده از شاخه‌ها یا تگ‌ها (مثل `main → prod`، `develop → dev`).
- اسکریپت‌های استقرار به‌صورت پویا زمینه محیط را می‌خوانند.

```bash
if [ "$ENV" == "staging" ]; then
  npm run deploy:staging
fi
```

<br />

### 3. چگونه تست‌های خودکار (واحد، یکپارچه‌سازی، E2E) را در یک خط لوله CI/CD برای پروژه React ادغام می‌کنید؟

- **تست‌های واحد:** Jest
- **یکپارچه‌سازی:** React Testing Library
- **E2E:** Cypress یا Playwright (بدون رابط کاربری در CI)

```yaml
steps:
  - run: npm test -- --coverage
  - run: npx cypress run
```

<br />

### 4. استراتژی شما برای مدیریت بازگشت به عقب (rollback) در خط لوله CI/CD وقتی استقرار در تولید شکست می‌خورد چیست؟

- استفاده از **استقرارهای غیرقابل تغییر** + **تگ‌های Git** یا **آرتیفکت‌های انتشار**.
- ذخیره آخرین ساخت موفق و خودکارسازی بازگشت:

```bash
aws s3 cp s3://backups/last-good-build ./ && npm run deploy
```

<br />

### 5. چگونه خط لوله CI/CD را برای کاهش زمان ساخت در یک پایگاه کد بزرگ React بهینه می‌کنید؟

- فعال‌سازی **ساخت‌های افزایشی** با TurboRepo یا Nx
- **کش کردن node_modules** و **آرتیفکت‌های ساخت**
- موازی‌سازی کارها:

```yaml
- uses: actions/cache@v3
  with:
    path: node_modules
```

<br />

### 6. رویکرد شما برای ایمن‌سازی خط لوله CI/CD برای یک برنامه React چیست؟

- چرخش و **رمزنگاری اسرار** (استفاده از GitHub Actions Secrets، Vault و غیره)
- **محدود کردن دسترسی نوشتن** به شاخه‌های اصلی
- استفاده از **تعهدات امضاشده**، بررسی وابستگی‌ها (مثل `npm audit`)

<br />

### 7. چگونه استقرارهای آبی-سبز یا کناری (canary) را در خط لوله CI/CD برای یک برنامه React پیاده‌سازی می‌کنید؟

- استفاده از **پرچم‌های ویژگی** + انتشار تدریجی (مثل LaunchDarkly)
- پشتیبانی در سطح پلتفرم (مثل پیش‌نمایش Vercel → ارتقاء)

```jsx
if (user.isInCanaryGroup) {
  return <NewComponent />
}
```

<br />

### 8. فرآیند شما برای ادغام تحلیل کد استاتیک و بررسی کد (linting) در خط لوله CI/CD چیست؟

- اجرای ESLint، TypeScript و Prettier در CI
- مسدود کردن درخواست‌های کشش (PR) در صورت تخلف:

```yaml
- run: npm run lint && npm run type-check
```

<br />

### 9. چگونه مدیریت وابستگی‌ها و نسخه‌بندی را در خط لوله CI/CD برای پروژه React مدیریت می‌کنید؟

- استفاده از **فایل قفل npm** (`package-lock.json`)
- خودکارسازی با **Dependabot**
- ردیابی تفاوت‌های بسته در CI

<br />

### 10. چگونه تیمی را برای پذیرش و نگهداری خط لوله CI/CD برای یک برنامه React آموزش می‌دهید؟

- جلسات جفتی + مستندات
- جریان‌های کاری بصری در درخواست‌های کشش
- ترویج دموهای داخلی + تشویق به مالکیت مشترک

<br />

### 11. چگونه یک سیستم نظارتی برای یک برنامه React در تولید طراحی می‌کنید تا عملکرد و خطاها را ردیابی کند؟

- **نظارت کلاینت:** Sentry، LogRocket، Datadog RUM
- **ردیابی خطا:** Try/catch + لاگ‌گیری مرزی
- **عملکرد:** گزارش از طریق Web Vitals API

<br />

### 12. چگونه Core Web Vitals (مثل LCP، CLS، FID) را در یک برنامه React پس از استقرار نظارت می‌کنید؟

- استفاده از بسته [`web-vitals`](https://github.com/GoogleChrome/web-vitals) برای ارسال معیارها به یک سرور یا ارائه‌دهنده تحلیل:

```jsx
import { onCLS, onFID, onLCP } from 'web-vitals';
onCLS(sendToAnalytics); onLCP(sendToAnalytics); onFID(sendToAnalytics);
```

<br />

### 13. چگونه هشدارهای بلادرنگ برای یک برنامه React تنظیم می‌کنید تا خطاهای اجرایی یا کاهش عملکرد را شناسایی کند؟

- ادغام ابزارهایی مثل Sentry یا Datadog با هشدارهای Slack/Webhook
- تنظیم آستانه‌های هشدار (مثل نرخ خطای بیش از 1%)

<br />

### 14. استراتژی شما برای نظارت بر تأخیر و در دسترس بودن API در یک برنامه React که به یک بک‌اند وابسته است چیست؟

- استفاده از APIهای زمان‌بندی فرانت‌اند (`performance.now()`)
- افزودن سربرگ‌های ردیابی (مثل `x-request-id`)
- گزارش کندی:

```jsx
const start = performance.now();
fetch('/api/data').finally(() => {
  const duration = performance.now() - start;
  if (duration > 1000) logSlowAPI('/api/data', duration);
});
```

<br />

### 15. چگونه از ابزارهای نظارتی برای ردیابی سلامت خود خط لوله CI/CD (مثل نرخ موفقیت ساخت، زمان‌های استقرار) استفاده می‌کنید؟

- استفاده از معیارهای داخلی GitHub Actions / CircleCI
- نظارت بر:
  - نرخ‌های موفقیت/شکست
  - میانگین زمان ساخت و استقرار
  - تست‌های ناپایدار

<br />

### 16. رویکرد شما برای لاگ‌گیری در یک برنامه React چیست و چگونه آن را با ابزارهای نظارتی ادغام می‌کنید؟

- استفاده از **انتزاع کنسول**:

```jsx
export const log = (msg) => {
  if (process.env.NODE_ENV === "production") {
    sendToLoggingService(msg);
  } else {
    console.log(msg);
  }
};
```

- ادغام با **Sentry breadcrumbs** یا **Datadog logs**.

<br />

### 17. چگونه ردیابی توزیع‌شده را در یک برنامه React با بک‌اند میکروسرویس پیاده‌سازی می‌کنید؟

- تزریق سربرگ‌های ردیابی از بک‌اند
- استفاده از **OpenTelemetry** یا **Datadog RUM**
- انتشار شناسه‌های همبستگی:

```jsx
fetch("/api", {
  headers: { "x-trace-id": currentTraceId },
});
```

<br />

### 18. فرآیند شما برای نظارت بر مصرف منابع (مثل CPU، حافظه) در یک برنامه React مستقر در پلتفرم بدون سرور چیست؟

- استفاده از ابزارهای بومی پلتفرم (مثل Vercel Analytics)
- گزارش مصرف حافظه سمت کلاینت در صورت نیاز:

```jsx
if (performance.memory) {
  log(performance.memory.usedJSHeapSize);
}
```

<br />

### 19. چگونه از ابزارهای نظارتی برای پشتیبانی از تست A/B یا انتشار پرچم‌های ویژگی در یک برنامه React استفاده می‌کنید؟

- ردیابی استفاده از طریق رویدادهای سفارشی (مثل Segment، Amplitude)
- تگ کردن جلسات با پرچم‌های فعال:

```jsx
analytics.track("feature_used", { variant: "B" });
```

<br />

### 20. چگونه تیمی را برای استفاده مؤثر از ابزارهای نظارتی برای یک برنامه React در تولید آموزش می‌دهید؟

- ایجاد داشبورد برای دیده شدن
- اجرای **تمرین‌های حالت شکست**
- چرخش مالکیت هشدارها
- مستندسازی روش‌های استاندارد برای دسته‌بندی/تشدید