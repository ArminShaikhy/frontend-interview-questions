### 1. طراحی یک کامپوننت مدال قابل استفاده مجدد در React که بتوان آن را از هر جای اپلیکیشن فعال کرد

**رویکرد**: استفاده از context و portal

```tsx
// ModalContext.tsx
const ModalContext = createContext(null);
export const useModal = () => useContext(ModalContext);

export function ModalProvider({ children }) {
  const [content, setContent] = useState(null);

  return (
    <ModalContext.Provider value={{ setContent }}>
      {children}
      {content &&
        ReactDOM.createPortal(
          <div className="modal-backdrop">{content}</div>,
          document.body
        )}
    </ModalContext.Provider>
  );
}
```

**نحوه استفاده**:

```tsx
const { setContent } = useModal();
setContent(<MyModalContent onClose={() => setContent(null)} />);
```

<br />

### 2. اگر یک کامپوننت React نیاز داشته باشد از چند API داده بگیرد و نتایج را ترکیب کند، چه کاری انجام می‌دهید؟

**رویکرد**:

- استفاده از `Promise.all` یا `async/await`
- ترکیب نتایج در `useEffect`

```tsx
useEffect(() => {
  const fetchData = async () => {
    const [user, posts] = await Promise.all([
      fetch("/api/user").then((res) => res.json()),
      fetch("/api/posts").then((res) => res.json()),
    ]);
    setData({ user, posts });
  };
  fetchData();
}, []);
```

**نکته اضافه**: مدیریت وضعیت بارگذاری/خطا و استفاده از `AbortController` برای پاکسازی

<br />

### 3. اگر مسئول بازسازی یک کد React قدیمی و نامرتب باشید، از کجا شروع می‌کنید؟

**رویکرد**:

1. بررسی کد: ساختار، lint، منطق تکراری، کد بدون استفاده
2. اضافه کردن lint و فرمت: ESLint، Prettier
3. جدا کردن کامپوننت‌های قابل استفاده مجدد در پوشه `shared/`
4. نوشتن تست برای مسیرهای حیاتی قبل از بازسازی
5. تبدیل کلاس‌ها به hook اگر امکان‌پذیر باشد
6. شروع به استفاده از TypeScript به صورت تدریجی، از بخش‌های عمومی و utils

<br />

### 4. چگونه قابلیت کشیدن و رها کردن (drag & drop) آیتم‌ها در یک لیست را در React پیاده‌سازی می‌کنید؟

**ابزار**: استفاده از `@dnd-kit/core` یا `react-beautiful-dnd`

**مثال با `react-beautiful-dnd`**:

```tsx
<DragDropContext onDragEnd={handleDragEnd}>
  <Droppable droppableId="list">
    {(provided) => (
      <ul {...provided.droppableProps} ref={provided.innerRef}>
        {items.map((item, index) => (
          <Draggable key={item.id} draggableId={item.id} index={index}>
            {(provided) => (
              <li
                ref={provided.innerRef}
                {...provided.draggableProps}
                {...provided.dragHandleProps}
              >
                {item.name}
              </li>
            )}
          </Draggable>
        ))}
        {provided.placeholder}
      </ul>
    )}
  </Droppable>
</DragDropContext>
```

<br />

### 5. اگر مسئول ساخت یک داشبورد با داده‌های به‌روزرسانی بلادرنگ در React باشید، چه رویکردی اتخاذ می‌کنید؟

**معماری**:

- استفاده از **WebSocket** یا **SSE** برای آپدیت بلادرنگ
- استفاده از **Redux/Context/Zustand** برای مدیریت وضعیت مشترک
- استفاده از polling در صورت نیاز

**استراتژی کامپوننت‌ها**:

- تقسیم ویجت‌ها به کامپوننت‌های جداگانه
- کاهش رندر مجدد با debounce کردن
- نمایش حالت بارگذاری یا اسکلت

**زیرساخت**:

- استفاده از مدیر WebSocket و منطق اتصال مجدد
- محدود کردن نرخ آپدیت برای بهبود عملکرد

<br />

### 6. اگر یک ذی‌نفع فیچری بخواهد که با اهداف عملکرد یا دسترسی‌پذیری در تضاد باشد، چه می‌کنید؟

**رویکرد**:

- **همدلی**: درک اهداف آن‌ها
- **آموزش**: توضیح تاثیر (مثلاً کندی بارگذاری یا مشکل در ناوبری با کیبورد)
- **نمایش**: ساخت یک نمونه اولیه کوچک برای نشان دادن تبادل
- **مصالحه**: پیشنهاد راه‌حلی که نیازهای فنی و کسب‌و‌کار را پوشش دهد
- **مستندسازی**: ثبت تصمیم و بازبینی در صورت تغییر داده‌ها

<br />

### 7. طراحی یک جدول داده قابل استفاده مجدد در React با قابلیت مرتب‌سازی، فیلتر و صفحه‌بندی

**API کلی**:

```tsx
<DataTable
  data={data}
  columns={[
    { key: "name", label: "Name", sortable: true },
    { key: "email", label: "Email" },
  ]}
  pagination={{ pageSize: 10 }}
  onSort={(key, direction) => {}}
  onFilter={(query) => {}}
/>
```

**نکات پیاده‌سازی**:

- مدیریت صفحه‌بندی و مرتب‌سازی محلی یا سمت سرور
- استفاده از `useMemo` برای داده‌های مشتق‌شده
- جدا کردن رندر ردیف با استفاده از render prop

<br />

### 8. اگر یک اپ React به شما واگذار شود که تست ندارد و مستندات ضعیف است، برای بهبود آن چه برنامه‌ای دارید؟

**برنامه**:

1. شناسایی مسیرهای حیاتی (مثلاً ورود، پرداخت)
2. ابتدا نوشتن تست‌های یکپارچه با React Testing Library
3. افزودن تست واحد برای منطق‌های کمکی و کامپوننت‌های مشترک
4. استفاده از JSDoc و Storybook برای مستندسازی بهتر
5. راه‌اندازی CI برای بررسی پوشش تست و lint
6. ایجاد راهنمای مشارکت برای اجباری کردن تست در هر مشارکت

<br />

### 9. چگونه یک API شخص ثالث با محدودیت نرخ را در یک اپلیکیشن React طوری ادغام می‌کنید که تجربه کاربر خراب نشود؟

**استراتژی‌ها**:

- محدودسازی یا تاخیر درخواست‌ها (مثلاً با استفاده از Lodash)
- استفاده از کش (مثل `SWR` یا `React Query`) برای جلوگیری از درخواست تکراری
- استفاده از backoff و retry هنگام دریافت وضعیت 429
- ارائه رابط کاربری خوشبینانه یا اسکلت بارگذاری برای کاهش زمان احساس‌شده
- در صورت نیاز، ارسال درخواست‌ها از طریق بک‌اند برای مدیریت بهتر محدودیت

```tsx
const fetcher = async (url) => {
  const res = await fetch(url);
  if (res.status === 429) {
    await new Promise((r) => setTimeout(r, 1000)); // simple backoff
    return fetcher(url);
  }
  return res.json();
};
```
