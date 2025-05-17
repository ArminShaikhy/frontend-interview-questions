### 1. چگونه یک خط لوله CI/CD برای یک برنامه React طراحی می‌کنید تا حلقه‌های بازخورد سریع و استقرارهای قابل اعتماد را تضمین کند؟

- **CI:** لینت → تست‌های واحد → ساخت
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



2. رویکرد شما برای خودکارسازی استقرار یک برنامه React به محیط‌های مختلف (مثل توسعه، مرحله‌بندی، تولید) چیست؟

استفاده از پیکربندی خاص محیط (.env.dev، .env.staging).
استفاده از شاخه‌ها یا برچسب‌ها (مثل main → prod، develop → dev).
اسکریپت‌های استقرار به‌صورت پویا زمینه محیط را می‌خوانند.

if [ "$ENV" == "staging" ]; then
  npm run deploy:staging
fi



3. چگونه تست‌های خودکار (واحد، یکپارچه‌سازی، E2E) را در یک خط لوله CI/CD برای یک پروژه React ادغام می‌کنید؟

تست‌های واحد: Jest
یکپارچه‌سازی: React Testing Library
E2E: Cypress یا Playwright (بدون رابط کاربری در CI)

steps:
  - run: npm test -- --coverage
  - run: npx cypress run



4. استراتژی شما برای مدیریت بازگشت (rollback) در یک خط لوله CI/CD در صورت شکست استقرار در تولید چیست؟

استفاده از استقرارهای غیرقابل تغییر + برچسب‌های Git یا آرتیفکت‌های انتشار.
ذخیره آخرین ساخت موفق و خودکارسازی بازگشت:

aws s3 cp s3://backups/last-good-build ./ && npm run deploy



5. چگونه یک خط لوله CI/CD را برای کاهش زمان ساخت یک کدبیس بزرگ React بهینه می‌کنید؟

فعال‌سازی ساخت‌های افزایشی از طریق TurboRepo یا Nx
کش node_modules و آرتیفکت‌های ساخت
موازی‌سازی کارها:

- uses: actions/cache@v3
  with:
    path: node_modules



6. رویکرد شما برای ایمن‌سازی یک خط لوله CI/CD برای یک برنامه React چیست؟

چرخش و رمزگذاری اسرار (استفاده از GitHub Actions Secrets، Vault و غیره)
محدود کردن دسترسی نوشتن به شاخه‌های اصلی
استفاده از امضاهای تأییدشده، بررسی وابستگی‌ها (مثل npm audit)



7. چگونه استقرارهای آبی-سبز یا کناری را در یک خط لوله CI/CD برای یک برنامه React پیاده‌سازی می‌کنید؟

استفاده از پرچم‌های ویژگی + انتشار تدریجی (مثل LaunchDarkly)
پشتیبانی در سطح پلتفرم (مثل پیش‌نمایش Vercel → ارتقاء)

if (user.isInCanaryGroup) {
  return <NewComponent />
}



8. فرآیند شما برای ادغام تحلیل کد استاتیک و لینتینگ در یک خط لوله CI/CD چیست؟

اجرای ESLint، TypeScript و Prettier در CI
مسدود کردن PRها در صورت نقض:

- run: npm run lint && npm run type-check



9. چگونه مدیریت وابستگی‌ها و نسخه‌بندی را در یک خط لوله CI/CD برای یک پروژه React مدیریت می‌کنید؟

استفاده از فایل قفل npm (package-lock.json)
خودکارسازی با Dependabot
ردیابی تفاوت‌های بسته‌ها در CI



10. چگونه تیمی را برای پذیرش و نگهداری یک خط لوله CI/CD برای یک برنامه React آموزش می‌دهید؟

جلسات جفتی + مستندات
گردش‌های کاری بصری در PRها
ترویج دموهای داخلی + تشویق به مالکیت مشترک



11. چگونه یک سیستم نظارتی برای یک برنامه React در تولید طراحی می‌کنید تا عملکرد و خطاها را ردیابی کند؟

نظارت کلاینت: Sentry، LogRocket، Datadog RUM
ردیابی خطا: Try/catch + ثبت مرزها
عملکرد: گزارش از طریق Web Vitals API



12. رویکرد شما برای نظارت بر Core Web Vitals (مثل LCP، CLS، FID) در یک برنامه React پس از استقرار چیست؟

استفاده از بسته web-vitals برای ارسال معیارها به یک بک‌اند یا ارائه‌دهنده تجزیه‌وتحلیل:

import { onCLS, onFID, onLCP } from 'web-vitals';
onCLS(sendToAnalytics); onLCP(sendToAnalytics); onFID(sendToAnalytics);



13. چگونه هشدارهای بلادرنگ را برای یک 프로그램 React تنظیم می‌کنید تا خطاهای اجرایی یا افت عملکرد را تشخیص دهد؟

ادغام ابزارهایی مانند Sentry یا Datadog با هشدارهای Slack/Webhook
تنظیم آستانه‌های هشدار (مثل نرخ خطای بیش از ۱٪)



14. استراتژی شما برای نظارت بر تأخیر و دسترسی API در یک برنامه React که به بک‌اند وابسته است چیست؟

استفاده از APIهای زمان‌بندی frontend (performance.now())
افزودن سربرگ‌های ردیابی (مثل x-request-id)
گزارش کندی:

const start = performance.now();
fetch('/api/data').finally(() => {
  const duration = performance.now() - start;
  if (duration > 1000) logSlowAPI('/api/data', duration);
});



15. چگونه از ابزارهای نظارتی برای ردیابی سلامت خود خط لوله CI/CD (مثل نرخ موفقیت ساخت، زمان‌های استقرار) استفاده می‌کنید؟

استفاده از معیارهای داخلی GitHub Actions / CircleCI
نظارت بر:
نرخ‌های موفقیت/شکست
میانگین زمان ساخت و استقرار
تست‌های ناپایدار





16. رویکرد شما برای ثبت وقایع در یک برنامه React چیست و چگونه آن را با ابزارهای نظارتی ادغام می‌کنید؟

استفاده از انتزاع کنسول:

export const log = (msg) => {
  if (process.env.NODE_ENV === "production") {
    sendToLoggingService(msg);
  } else {
    console.log(msg);
  }
};


ادغام با Sentry breadcrumbs یا Datadog logs.



17. چگونه ردیابی توزیع‌شده را در یک برنامه React با بک‌اند میکروسرویس پیاده‌سازی می‌کنید؟

تزریق سربرگ‌های ردیابی از بک‌اند
استفاده از OpenTelemetry یا Datadog RUM
انتشار شناسه‌های همبستگی:

fetch("/api", {
  headers: { "x-trace-id": currentTraceId },
});



18. چگونه استفاده از منابع (مثل CPU، حافظه) را در یک برنامه React مستقر شده روی پلتفرم بدون سرور نظارت می‌کنید؟

استفاده از ابزارهای بومی پلتفرم (مثل Vercel Analytics)
گزارش استفاده از حافظه سمت کلاینت در صورت نیاز:

if (performance.memory) {
  log(performance.memory.usedJSHeapSize);
}



19. چگونه از ابزارهای نظارتی برای پشتیبانی از تست A/B یا انتشار پرچم‌های ویژگی در یک برنامه React استفاده می‌کنید؟

ردیابی استفاده از طریق رویدادهای سفارشی (مثل Segment، Amplitude)
برچسب‌گذاری جلسات با پرچم‌های فعال:

analytics.track("feature_used", { variant: "B" });



20. چگونه تیمی را برای استفاده مؤثر از ابزارهای نظارتی برای یک برنامه React در تولید آموزش می‌دهید؟

ایجاد داشبورد برای دید
اجرای تمرین‌های حالت شکست
چرخش مالکیت هشدارها
مستندسازی SOPها برای دسته‌بندی/تشدید


### Notes
- The Markdown file (`cicd-monitoring.md`) contains the complete Persian translation of the provided content.
- The structure, headings, and code snippets are preserved, with Persian translations for all non-code text.
- Technical terms (e.g., CI/CD, GitHub Actions, Sentry, Core Web Vitals) are kept in English, as is standard in Persian technical documentation, to maintain clarity and consistency.
- Code snippets remain unchanged, as they are in JavaScript/JSX/YAML/Bash, but any user-facing text within code (e.g., comments, log messages) is translated to Persian where applicable.
- If you need the file to be sent in a specific way (e.g., as a downloadable file, email attachment, or otherwise), please let me know, and I can provide further assistance.

Let me know if you need additional files translated, combined, or any other actions!

