### 1. چگونه الگوی Singleton را در یک برنامه React پیاده‌سازی می‌کنید و چه زمانی از آن استفاده می‌کنید؟

**مفهوم:** اطمینان می‌دهد که تنها یک نمونه از یک کلاس یا ماژول وجود دارد.

**چه زمانی:** سرویس‌های مشترک (مثل تنظیمات، ثبت وقایع، کلاینت‌های API).

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

در React: در لایه‌های سرویس خارج از درخت کامپوننت استفاده می‌شود.


2. رویکرد شما برای استفاده از الگوی Factory در جاوااسکریپت برای ایجاد دینامیک کامپوننت‌های React چیست؟
مفهوم: ایجاد کامپوننت‌ها بر اساس نوع ورودی.
const componentFactory = (type) => {
  switch (type) {
    case 'text': return <TextInput />;
    case 'select': return <SelectInput />;
    default: return <DefaultInput />;
  }
};

چه زمانی: فرم‌های دینامیک یا داشبوردهای مبتنی بر تنظیمات رابط کاربری.


3. چگونه الگوی Builder را در یک برنامه React برای ساخت کامپوننت‌های پیچیده رابط کاربری اعمال می‌کنید؟
مفهوم: ساخت گام‌به‌گام یک عنصر رابط کاربری.
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

چه زمانی: کامپوننت‌های رابط کاربری با بخش‌های اختیاری زیاد (مثل مودال‌ها).


4. نقش الگوی Prototype در جاوااسکریپت چیست و چگونه می‌توان از آن در زمینه React استفاده کرد؟
مفهوم: به اشتراک گذاشتن رفتار از طریق پروتوتایپ‌ها به‌جای کلاس‌ها.
در React: به‌ندرت به‌صورت مستقیم استفاده می‌شود، اما در جاوااسکریپت زیربنایی است. برای گسترش اشیاء مفید است (مثل سیستم‌های رویداد سفارشی یا پلی‌فیل‌ها).
const proto = {
  greet() { return `سلام ${this.name}`; }
};
const user = Object.create(proto);
user.name = "علی";



5. چگونه الگوی Decorator را در یک برنامه React برای بهبود عملکرد کامپوننت پیاده‌سازی می‌کنید؟
مفهوم: افزودن رفتار بدون تغییر اصل.
مثال React (HOC):
const withLogger = (Component) => (props) => {
  console.log('رندر شده با پراپ‌ها:', props);
  return <Component {...props} />;
};

چه زمانی: نگرانی‌های متقاطع (احراز هویت، ثبت وقایع، معیارها).


6. رویکرد شما برای استفاده از الگوی Adapter در یک برنامه React برای ادغام با APIهای قدیمی یا کتابخانه‌های شخص ثالث چیست؟
مفهوم: تبدیل یک رابط به رابط دیگر.
const legacyApi = { fetchData: () => fetch('/old-endpoint') };
const apiAdapter = {
  getData: () => legacyApi.fetchData().then(res => res.json())
};

در React: بسته‌بندی APIهای شخص ثالث در ماژول‌های سرویس استاندارد.


7. چگونه الگوی Composite را در React برای مدیریت ساختارهای سلسله‌مراتبی کامپوننت (مثل نمای درختی) اعمال می‌کنید؟
مفهوم: رفتار یکسان با کامپوننت‌های منفرد و ترکیبی.
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



8. استراتژی شما برای استفاده از الگوی Facade در یک برنامه React برای ساده‌سازی تعاملات زیرساخت پیچیده چیست؟
مفهوم: ارائه یک رابط یکپارچه به زیرساخت‌های پیچیده.
// apiFacade.js
export const api = {
  getUser: () => fetch('/user'),
  getSettings: () => fetch('/settings')
};

کاربرد در React: ماژول‌های سرویس متمرکز برای منطق تمیز کامپوننت.


9. چگونه الگوی Proxy را در جاوااسکریپت برای یک برنامه React پیاده‌سازی می‌کنید تا دسترسی به منابع را کنترل کنید؟
مفهوم: کنترل دسترسی به یک شیء (مثل کش، اعتبارسنجی).
const api = new Proxy(fetch, {
  apply(target, thisArg, args) {
    console.log('در حال جلب:', args[0]);
    return target(...args);
  }
});

چه زمانی: ثبت وقایع، کش، بارگذاری تنبل.


10. رویکرد شما برای استفاده از الگوی Module در یک کدبیس React برای کپسوله‌سازی منطق چیست؟
مفهوم: کپسوله‌سازی منطق در یک واحد خودکفا.
// counterModule.js
let count = 0;
export const counter = {
  increment: () => ++count,
  get: () => count
};

کاربرد در React: منطق ابزار مشترک یا سرویس‌های حالتمند.


11. چگونه الگوی Observer را در یک برنامه React برای ارتباط مبتنی بر رویداد پیاده‌سازی می‌کنید؟
مفهوم: موضوع به ناظران در تغییر حالت اطلاع می‌دهد.
class EventBus {
  observers = [];
  subscribe(fn) { this.observers.push(fn); }
  notify(data) { this.observers.forEach(fn => fn(data)); }
}

کاربرد در React: انتشار/اشتراک برای اشتراک حالت در میکروفرانت‌اندها یا تب‌ها.


12. رویکرد شما برای اعمال الگوی Strategy در React برای تغییر دینامیک بین الگوریتم‌ها یا رفتارها چیست؟
مفهوم: تعویض الگوریتم‌ها/استراتژی‌ها در زمان اجرا.
const sortStrategies = {
  byName: (a, b) => a.name.localeCompare(b.name),
  byAge: (a, b) => a.age - b.age
};
const sortData = (data, strategy) => data.sort(sortStrategies[strategy]);

کاربرد در React: منطق دینامیک رابط کاربری (فیلترها، چیدمان‌ها، اعتبارسنجی‌ها).


13. چگونه الگوی Command را در یک برنامه React برای کپسوله‌سازی اقدامات (مثل عملکرد بازگشت/تکرار) استفاده می‌کنید؟
مفهوم: کپسوله‌سازی دستورات کاربر (بازگشت/تکرار).
class Command {
  execute() {}
  undo() {}
}
class AddItemCommand extends Command {
  execute() { /* افزودن */ }
  undo() { /* حذف */ }
}

کاربرد در React: سازندگان فرم، ویرایشگرهای بوم.


14. استراتژی شما برای پیاده‌سازی الگوی Mediator در یک برنامه React برای هماهنگی چندین کامپوننت چیست؟
مفهوم: شیء مرکزی ارتباط را هماهنگ می‌کند.
const mediator = {
  notify: (sender, event) => {
    if (event === 'save') console.log(`${sender} ذخیره را فعال کرد`);
  }
};

کاربرد در React: سیستم‌های دیالوگ یا داشبوردهای کم‌پیوسته.


15. چگونه الگوی Chain of Responsibility را در یک برنامه React برای مدیریت وظایف متوالی اعمال می‌کنید؟
مفهوم: انتقال درخواست در یک زنجیره تا زمانی که مدیریت شود.
const handlerA = (req, next) => req.type === 'A' ? 'مدیریت A' : next(req);
const handlerB = (req) => req.type === 'B' ? 'مدیریت B' : 'مدیریت‌نشده';

const chain = (req) => handlerA(req, handlerB);

کاربرد در React: منطق رابط کاربری مشابه میدلور، زنجیره‌های اعتبارسنجی فرم.


16. چگونه از الگوی Higher-Order Component (HOC) در React استفاده می‌کنید و مزایا و معایب آن چیست؟
مفهوم: تابعی که رفتار به کامپوننت اضافه می‌کند.
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading ? <Spinner /> : <Component {...rest} />;

مزایا: قابلیت استفاده مجدد، جداسازی نگرانی‌ها
معایب: برخورد پراپ‌ها، پیچیدگی تودرتو


17. رویکرد شما برای پیاده‌سازی الگوی Render Props در React برای اشتراک منطق بین کامپوننت‌ها چیست؟
مفهوم: اشتراک منطق از طریق تابع به‌عنوان فرزند.
<MouseTracker render={({ x, y }) => <Cursor x={x} y={y} />} />

کاربرد: منطق قابل استفاده مجدد بدون مشکلات HOC.


18. چگونه الگوی Provider (مثل Context API) را در React برای حالت یا تنظیمات جهانی طراحی می‌کنید؟
مفهوم: استفاده از Context React برای تزریق مقادیر جهانی.
const ThemeContext = React.createContext();

const ThemeProvider = ({ children }) => {
  const [theme] = useState("تاریک");
  return <ThemeContext.Provider value={theme}>{children}</ThemeContext.Provider>;
};

کاربرد: احراز هویت، تم‌سازی، تنظیمات، محلی‌سازی.


19. استراتژی شما برای اعمال الگوی Container/Presentational در یک برنامه React چیست؟
مفهوم: جداسازی منطق (کانتینر) از نمایش (نمایشی).
// کانتینر
const UserContainer = () => {
  const [user, setUser] = useState(null);
  return <UserProfile user={user} />;
};

// نمایشی
const UserProfile = ({ user }) => <div>{user?.name}</div>;

کاربرد: تست، استفاده مجدد، جداسازی نگرانی‌ها.


20. چگونه تیمی را برای انتخاب و پیاده‌سازی الگوهای طراحی در یک کدبیس React آموزش می‌دهید؟

استفاده از مثال‌های واقعی و بررسی‌های کد.
مستندسازی الگوهای قابل استفاده مجدد در یک راهنمای مشترک یا سیستم طراحی.
تشویق به بحث‌های الگو در بررسی‌های کد.
استفاده از برنامه‌نویسی جفتی برای تقویت تفکر طراحی.

هدف: کمک به توسعه‌دهندگان برای انتخاب الگوها بر اساس نیت، نه صرفاً آشنایی.```
