---
title: ویژگی‌ها و معماری پیشرفته
---

<link rel="stylesheet" href="{{ site.baseurl }}/assets/css/persian.css">

### 1. چطور در یک برنامه React رندر سمت سرور (SSR) پیاده‌سازی می‌کنید؟

**روش** : استفاده از فریم‌ورکی مثل **Next.js** که تنظیمات SSR را ساده می‌کند.

**مفهوم** :

- SSR کامپوننت‌های React را در سمت سرور به HTML تبدیل می‌کند.
- این باعث بهبود **سرعت بارگذاری اولیه** ، **SEO** و **زمان تعامل کاربر** می‌شود.

  **مثال Next.js** :

```tsx
// pages/index.tsx
export default function Home({ user }) {
  return <div>Hello, {user.name}</div>;
}

export async function getServerSideProps() {
  const res = await fetch("https://api.example.com/user");
  const user = await res.json();

  return { props: { user } };
}
```

**SSR سفارشی** (با استفاده از `ReactDOMServer`):

```tsx
const html = ReactDOMServer.renderToString(<App />);
```

**چه زمانی استفاده کنیم؟** : صفحات عمومی که نیاز به SEO یا بارگذاری سریع اولیه دارند (مثل صفحات لندینگ یا فروشگاه‌های اینترنتی).

<br />

### 2. تفاوت کامپوننت‌های کنترل‌شده و کنترل‌نشده در React چیست؟ و چه زمانی از هرکدام استفاده می‌کنید؟

**کنترل‌شده** : وضعیت فرم توسط React با `useState` مدیریت می‌شود.

```tsx
const [value, setValue] = useState("");
<input value={value} onChange={(e) => setValue(e.target.value)} />;
```

**کنترل‌نشده** : وضعیت فرم توسط DOM با استفاده از `ref` کنترل می‌شود.

```tsx
const inputRef = useRef();
<input ref={inputRef} />;
```

**چه زمانی استفاده کنیم؟** :

- **کنترل‌شده** : وقتی اعتبارسنجی لحظه‌ای، فیلدهای شرطی، یا منطق پیچیده در فرم نیاز دارید.
- **کنترل‌نشده** : در فرم‌های ساده، آپلود فایل، یا سناریوهایی که بهینه‌سازی عملکرد مهم است.

<br />

### 3. چگونه در React با Error Boundaryها برخورد می‌کنید؟ آیا می‌توانید یک کامپوننت ساده بنویسید؟

**مفهوم** :

Error Boundaryها خطاهای **رندرینگ** ، **متدهای lifecycle** یا **سازنده‌ها** در کامپوننت‌های فرزند را می‌گیرند — ولی خطاهای event handler یا کد async را نه.

**مثال** :

```tsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // Log error to monitoring service
    console.error(error, info);
  }

  render() {
    return this.state.hasError ? (
      <h1>Something went wrong.</h1>
    ) : (
      this.props.children
    );
  }
}

// Usage
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>;
```

**چه زمانی استفاده کنیم؟** : دور کامپوننت‌های بزرگ، صفحات، یا importهای داینامیک.

<br />

### 4. چطور یک برنامه بزرگ React با صدها کامپوننت را به شکل مقیاس‌پذیر و قابل نگهداری طراحی می‌کنید؟

**اصول کلیدی** :

- **ساختار پوشه‌ای ماژولار** بر اساس دامنه یا قابلیت:
  ```
  /features/user
  /features/dashboard
  /shared/components
  ```
- **کامپوننت‌های اتمیک** و الگوهای قابل استفاده مجدد برای UI (مثل دکمه‌ها، ورودی‌ها).
- **ایمنی نوعی (Type Safety)** با استفاده از TypeScript.
- **مدیریت وضعیت** در سطح مناسب: وضعیت محلی → Context → وضعیت سراسری (مثل Redux یا Zustand).
- **تقسیم کد (Code Splitting)** با React.lazy یا dynamic imports در Next.js.
- **استانداردهای linting/فرمت‌بندی/تست** با ابزارهایی مثل ESLint، Prettier و Jest.
- **مستندسازی و Storybook** برای کشف کامپوننت‌ها و هماهنگی با سیستم طراحی.

<br />

### 5. الگوی event delegation چیست و React چگونه در سیستم event مصنوعی خود از آن استفاده می‌کند؟

**Event Delegation** : یک handler (مثلاً در `document`) رویدادهای بسیاری از عناصر فرزند را با استفاده از event bubbling دریافت می‌کند.

**رویدادهای مصنوعی React** :

- React تنها یک listener کلی برای هر نوع رویداد به **ریشه‌ی DOM** وصل می‌کند (نه برای هر عنصر جداگانه).
- رویدادها را در قالب یک **SyntheticEvent** می‌پیچد تا در همه مرورگرها یکسان رفتار کند.
- React رویدادها را از طریق **درخت virtual DOM** حبابی می‌کند، نه DOM واقعی.

  **مثال** :

```tsx
function App() {
  return <button onClick={() => console.log("clicked")}>Click Me</button>;
}
```

در پشت صحنه، این `onClick` توسط یک listener کلی روی ریشه کنترل می‌شود که باعث **عملکرد بهتر** و **صرفه‌جویی در حافظه** می‌شود.
