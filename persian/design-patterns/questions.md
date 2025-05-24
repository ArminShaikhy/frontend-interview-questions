### 1. چگونه الگوی Singleton را در یک برنامه React پیاده‌سازی می‌کنید و چه زمانی از آن استفاده می‌کنید؟

**مفهوم:** اطمینان می‌دهد که تنها یک نمونه از یک کلاس یا ماژول وجود داشته باشد.

**چه زمانی:** سرویس‌های اشتراکی (مثل پیکربندی، لاگ‌گیری، کلاینت‌های API).

```tsx
// singleton.js
let instance;
class Logger {
  constructor() {
    if (instance) return instance;
    instance = this;
  }
  log(msg) {
    console.log(msg);
  }
}
export const logger = new Logger();
```

**در React:** در لایه‌های سرویس خارج از درخت کامپوننت استفاده می‌شود.

<br />

### 2. رویکرد شما برای استفاده از الگوی Factory در جاوااسکریپت برای ایجاد دینامیک کامپوننت‌های React چیست؟

**مفهوم:** ایجاد کامپوننت‌ها بر اساس نوع ورودی.

```jsx
const componentFactory = (type) => {
  switch (type) {
    case 'text': return <TextInput />;
    case 'select': return <SelectInput />;
    default: return <DefaultInput />;
  }
};
```

**چه زمانی:** فرم‌های دینامیک یا داشبوردهای مبتنی بر پیکربندی.

<br />

### 3. چگونه الگوی Builder را در یک برنامه React برای ساخت کامپوننت‌های پیچیده UI اعمال می‌کنید؟

**مفهوم:** ساخت گام‌به‌گام یک عنصر رابط کاربری.

```jsx
class CardBuilder {
  constructor() {
    this.card = {};
  }
  setTitle(title) { this.card.title = title; return this; }
  setContent(content) { this.card.content = content; return this; }
  build() {
    return <Card {...this.card} />;
  }
}
```

**چه زمانی:** کامپوننت‌های رابط کاربری با بخش‌های اختیاری زیاد (مثل مدال‌ها).

<br />

### 4. نقش الگوی Prototype در جاوااسکریپت چیست و چگونه می‌توان آن را در زمینه React استفاده کرد؟

**مفهوم:** اشتراک رفتار از طریق پروتوتایپ‌ها به جای کلاس‌ها.

**در React:** به‌ندرت به‌صورت مستقیم استفاده می‌شود، اما در جاوااسکریپت زیربنایی است. برای گسترش اشیا (مثل سیستم‌های رویداد سفارشی یا پلی‌فیل‌ها) مفید است.

```jsx
const proto = {
  greet() { return `Hello ${this.name}`; }
};
const user = Object.create(proto);
user.name = "Alice";
```

<br />

### 5. چگونه الگوی Decorator را در یک برنامه React برای بهبود عملکرد کامپوننت پیاده‌سازی می‌کنید؟

**مفهوم:** افزودن رفتار بدون تغییر اصل.

**مثال React (HOC):**

```jsx
const withLogger = (Component) => (props) => {
  console.log('Rendered with props:', props);
  return <Component {...props} />;
};
```

**چه زمانی:** نگرانی‌های متقاطع (احراز هویت، لاگ‌گیری، معیارها).

<br />

### 6. رویکرد شما برای استفاده از الگوی Adapter در یک برنامه React برای ادغام با APIهای قدیمی یا کتابخانه‌های شخص ثالث چیست؟

**مفهوم:** ترجمه یک رابط به رابط دیگر.

```jsx
const legacyApi = { fetchData: () => fetch('/old-endpoint') };
const apiAdapter = {
  getData: () => legacyApi.fetchData().then(res => res.json())
};
```

**در React:** بسته‌بندی APIهای شخص ثالث در ماژول‌های سرویس استاندارد.

<br />

### 7. چگونه الگوی Composite را در React برای مدیریت ساختارهای سلسله‌مراتبی کامپوننت (مثل نمای درختی) اعمال می‌کنید؟

**مفهوم:** رفتار یکسان با کامپوننت‌های تکی و ترکیبی.

```jsx
const TreeNode = ({ node }) => (
  <li>
    {node.label}
    {node.children && (
      <ul>
        {node.children.map((child) => (
          <TreeNode node={child} />
        ))}
      </ul>
    )}
  </li>
);
```

<br />

### 8. استراتژی شما برای استفاده از الگوی Facade در یک برنامه React برای ساده‌سازی تعاملات زیرساختی پیچیده چیست؟

**مفهوم:** ارائه یک رابط یکپارچه برای زیرساخت‌های پیچیده.

```jsx
// apiFacade.js
export const api = {
  getUser: () => fetch('/user'),
  getSettings: () => fetch('/settings')
};
```

**کاربرد در React:** ماژول‌های سرویس متمرکز برای منطق تمیز کامپوننت.

<br />

### 9. چگونه الگوی Proxy را در جاوااسکریپت برای یک برنامه React برای کنترل دسترسی به منابع پیاده‌سازی می‌کنید؟

**مفهوم:** کنترل دسترسی به یک شیء (مثل کشینگ، اعتبارسنجی).

```jsx
const api = new Proxy(fetch, {
  apply(target, thisArg, args) {
    console.log('Fetching:', args[0]);
    return target(...args);
  }
});
```

**چه زمانی:** لاگ‌گیری، کشینگ، بارگذاری تنبل.

<br />

### 10. رویکرد شما برای استفاده از الگوی Module در یک پایگاه کد React برای محصورسازی منطق چیست؟

**مفهوم:** محصورسازی منطق در یک واحد خودکفا.

```jsx
// counterModule.js
let count = 0;
export const counter = {
  increment: () => ++count,
  get: () => count
};
```

**کاربرد در React:** منطق ابزار اشتراکی یا سرویس‌های دارای وضعیت.

<br />

### 11. چگونه الگوی Observer را در یک برنامه React برای ارتباط مبتنی بر رویداد پیاده‌سازی می‌کنید؟

**مفهوم:** یک سوژه به مشاهده‌کنندگان در زمان تغییر وضعیت اطلاع می‌دهد.

```jsx
class EventBus {
  observers = [];
  subscribe(fn) { this.observers.push(fn); }
  notify(data) { this.observers.forEach(fn => fn(data)); }
}
```

**کاربرد در React:** انتشار/اشتراک برای اشتراک وضعیت در میکروفرانت‌اند یا تب‌ها.

<br />

### 12. رویکرد شما برای اعمال الگوی Strategy در React برای تغییر دینامیک بین الگوریتم‌ها یا رفتارها چیست؟

**مفهوم:** تعویض الگوریتم‌ها/استراتژی‌ها در زمان اجرا.

```jsx
const sortStrategies = {
  byName: (a, b) => a.name.localeCompare(b.name),
  byAge: (a, b) => a.age - b.age
};
const sortData = (data, strategy) => data.sort(sortStrategies[strategy]);
```

**کاربرد در React:** منطق رابط کاربری دینامیک (فیلترها، طرح‌بندی‌ها، اعتبارسنجی‌ها).

<br />

### 13. چگونه الگوی Command را در یک برنامه React برای محصورسازی اقدامات (مثل عملکرد لغو/بازگردانی) استفاده می‌کنید؟

**مفهوم:** محصورسازی دستورات کاربر (لغو/بازگردانی).

```jsx
class Command {
  execute() {}
  undo() {}
}
class AddItemCommand extends Command {
  execute() { /* add */ }
  undo() { /* remove */ }
}
```

**کاربرد در React:** سازنده‌های فرم، ویرایشگرهای بوم.

<br />

### 14. استراتژی شما برای پیاده‌سازی الگوی Mediator در یک برنامه React برای هماهنگی چندین کامپوننت چیست؟

**مفهوم:** شیء مرکزی ارتباطات را هماهنگ می‌کند.

```jsx
const mediator = {
  notify: (sender, event) => {
    if (event === 'save') console.log(`${sender} triggered save`);
  }
};
```

**کاربرد در React:** سیستم‌های گفت‌وگو یا داشبوردهای با اتصال آزاد.

<br />

### 15. چگونه الگوی Chain of Responsibility را در یک برنامه React برای مدیریت وظایف متوالی اعمال می‌کنید؟

**مفهوم:** انتقال درخواست در یک زنجیره تا زمانی که مدیریت شود.

```jsx
const handlerA = (req, next) => req.type === 'A' ? 'Handled A' : next(req);
const handlerB = (req) => req.type === 'B' ? 'Handled B' : 'Unhandled';

const chain = (req) => handlerA(req, handlerB);
```

**کاربرد در React:** منطق رابط کاربری شبیه میان‌افزار، زنجیره‌های اعتبارسنجی فرم.

<br />

### 16. چگونه الگوی Higher-Order Component (HOC) را در React استفاده می‌کنید و مزایا و معایب آن چیست؟

**مفهوم:** تابعی که رفتار را به یک کامپوننت اضافه می‌کند.

```jsx
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading ? <Spinner /> : <Component {...rest} />;
```

**مزایا:** قابلیت استفاده مجدد، جداسازی نگرانی‌ها

**معایب:** تداخل پراپ‌ها، پیچیدگی تودرتو

<br />

### 17. رویکرد شما برای پیاده‌سازی الگوی Render Props در React برای اشتراک منطق بین کامپوننت‌ها چیست؟

**مفهوم:** اشتراک منطق از طریق تابع به‌عنوان فرزند.

```jsx
<MouseTracker render={({ x, y }) => <Cursor x={x} y={y} />} />
```

**کاربرد:** منطق قابل استفاده مجدد بدون مشکلات HOC.

<br />

### 18. چگونه الگوی Provider (مثل Context API) را در React برای وضعیت یا پیکربندی جهانی طراحی می‌کنید؟

**مفهوم:** استفاده از Context React برای تزریق مقادیر جهانی.

```jsx
const ThemeContext = React.createContext();

const ThemeProvider = ({ children }) => {
  const [theme] = useState("dark");
  return <ThemeContext.Provider value={theme}>{children}</ThemeContext.Provider>;
};
```

**کاربرد:** احراز هویت، تم‌دهی، پیکربندی، محلی‌سازی.

<br />

### 19. استراتژی شما برای اعمال الگوی Container/Presentational در یک برنامه React چیست؟

**مفهوم:** جداسازی منطق (ظرف) از نمایش (نمایش‌دهنده).

```jsx
// Container
const UserContainer = () => {
  const [user, setUser] = useState(null);
  return <UserProfile user={user} />;
};

// Presentational
const UserProfile = ({ user }) => <div>{user?.name}</div>;
```

**کاربرد:** تست، استفاده مجدد، جداسازی نگرانی‌ها.

<br />

### 20. چگونه تیمی را برای انتخاب و پیاده‌سازی الگوهای طراحی در یک پایگاه کد React آموزش می‌دهید؟

- استفاده از **مثال‌های واقعی** و **بررسی‌های کد**.
- مستندسازی الگوهای قابل استفاده مجدد در یک **راهنمای مشترک یا سیستم طراحی**.
- تشویق به **بحث‌های الگو در بررسی‌های کد**.
- استفاده از **برنامه‌نویسی جفتی** برای تقویت تفکر طراحی.

**هدف:** کمک به توسعه‌دهندگان برای انتخاب الگوها بر اساس نیت، نه فقط آشنایی.