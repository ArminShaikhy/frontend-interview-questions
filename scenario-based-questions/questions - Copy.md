### 1. یک کامپوننت مودال قابل استفاده مجدد در React طراحی کنید که از هر نقطه‌ای در برنامه قابل فعال‌سازی باشد.

**رویکرد**: استفاده از زمینه (Context) و پورتال‌ها.

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
          <div className="پس‌زمینه-مودال">{content}</div>,
          document.body
        )}
    </ModalContext.Provider>
  );
}

استفاده:
const { setContent } = useModal();
setContent(<محتوای_مودالم onClose={() => setContent(null)} />);



2. چگونه موقعیتی را مدیریت می‌کنید که یک کامپوننت React نیاز به دریافت داده از چندین API و ترکیب نتایج دارد؟
رویکرد:

استفاده از Promise.all یا async/await.
ترکیب نتایج در useEffect.

useEffect(() => {
  const fetchData = async () => {
    const [user, posts] = await Promise.all([
      fetch("/api/کاربر").then((res) => res.json()),
      fetch("/api/پست‌ها").then((res) => res.json()),
    ]);
    setData({ user, posts });
  };
  fetchData();
}, []);

امتیاز اضافی: افزودن حالت‌های بارگذاری/خطا و استفاده از AbortController برای پاک‌سازی.


3. فرض کنید وظیفه بازسازی یک کدبیس بزرگ و ضعیف نگهداری‌شده React به شما محول شده است. از کجا شروع می‌کنید؟
رویکرد:

ممیزی کدبیس: ساختار، لینتینگ، منطق تکراری، کد مرده.
معرفی لینتینگ/قالب‌بندی: ESLint، Prettier.
جداسازی کامپوننت‌های قابل استفاده مجدد در یک دایرکتوری shared/.
افزودن تست‌ها برای مسیرهای بحرانی قبل از بازسازی.
تبدیل کامپوننت‌های کلاسی به هوک‌ها در صورت امکان.
تایپ‌سازی تدریجی با TypeScript، شروع از ابزارها و کامپوننت‌های مشترک.



4. چگونه ویژگی‌ای را پیاده‌سازی می‌کنید که کاربر بتواند آیتم‌ها را در یک لیست در یک برنامه React بکشد و رها کند؟
ابزار: استفاده از @dnd-kit/core یا react-beautiful-dnd.
مثال با react-beautiful-dnd:
<DragDropContext onDragEnd={handleDragEnd}>
  <Droppable droppableId="لیست">
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



5. شما وظیفه هدایت یک تیم برای ساخت یک داشبورد با به‌روزرسانی‌های داده بلادرنگ در React را دارید. چگونه به آن نزدیک می‌شوید؟
معماری:

استفاده از WebSockets یا SSE برای به‌روزرسانی‌های بلادرنگ.
Redux/Context/Zustand برای مدیریت حالت مشترک.
نظرخواهی به‌عنوان پشتیبان در صورت نیاز.

استراتژی کامپوننت:

تقسیم ویجت‌ها به کامپوننت‌های مجزا.
محدود کردن به‌روزرسانی‌ها برای کاهش رندرها.
نمایش حالت‌های بارگذاری/اسکلتی.

زیرساخت:

استفاده از مدیر WebSocket و منطق اتصال مجدد.
محدود کردن نرخ به‌روزرسانی‌ها برای عملکرد.



6. چگونه موقعیتی را مدیریت می‌کنید که یک ذی‌نفع درخواستی برای ویژگی‌ای دارد که با اهداف عملکرد یا دسترسی‌پذیری در تضاد است؟
رویکرد:

همدلی: تأیید اهداف آن‌ها.
آموزش: به اشتراک گذاشتن تأثیر (مثل بارگذاری کند، نقض ناوبری کیبورد).
نمایش: ساخت یک POC کوچک برای نشان دادن معامله.
مصالحه: پیشنهاد جایگزینی که نیازهای تجاری و فنی را برآورده کند.
مستندسازی: ثبت تصمیمات و بازبینی در صورت نشان دادن داده‌ها ارزش تکرار را داشته باشد.



7. یک کامپوننت جدول داده قابل استفاده مجدد در React با ویژگی‌های مرتب‌سازی، فیلتر کردن و صفحه‌بندی طراحی کنید.
API سطح بالا:
<DataTable
  data={داده}
  columns={[
    { key: "نام", label: "نام", sortable: true },
    { key: "ایمیل", label: "ایمیل" },
  ]}
  pagination={{ pageSize: 10 }}
  onSort={(key, direction) => {}}
  onFilter={(query) => {}}
/>

ملاحظات پیاده‌سازی:

مدیریت صفحه‌بندی/مرتب‌سازی محلی در مقابل سمت سرور.
استفاده از مموایزیشن (useMemo) برای داده‌های مشتق‌شده.
استخراج رندرینگ ردیف از طریق پراپ رندر.



8. یک برنامه React که به ارث برده‌اید هیچ تستی ندارد و مستندات ضعیفی دارد. چگونه تلاش برای بهبود آن را هدایت می‌کنید؟
برنامه:

فهرست جریان‌های بحرانی (مثل ورود، پرداخت).
نوشتن تست‌های یکپارچگی ابتدا با استفاده از React Testing Library.
معرفی تست‌های واحد برای منطق ابزار و کامپوننت‌های مشترک.
استفاده از JSDoc و Storybook برای بهبود مستندات توسعه‌دهنده.
راه‌اندازی خط لوله CI با ردیابی پوشش و بررسی‌های لینت.
ایجاد یک راهنمای مشارکت برای اعمال مشارکت‌های تست‌محور.



9. چگونه یک API شخص ثالث با محدودیت نرخ را در یک برنامه React ادغام می‌کنید بدون اینکه تجربه کاربری را کاهش دهید؟
استراتژی‌ها:

محدود کردن یا تأخیر دادن فراخوانی‌ها (مثل استفاده از Lodash).
استفاده از کش (مثل SWR، React Query) برای جلوگیری از درخواست‌های تکراری.
بازگشت و امتحان مجدد در پاسخ‌های 429.
ارائه رابط کاربری خوش‌بینانه یا اسکلت‌های بارگذاری برای کاهش تأخیر ادراکی.
در صورت نیاز، پروکسی از طریق بک‌اند برای مدیریت بهتر محدودیت‌های نرخ به‌صورت جهانی.

const fetcher = async (url) => {
  const res = await fetch(url);
  if (res.status === 429) {
    await new Promise((r) => setTimeout(r, 1000)); // بازگشت ساده
    return fetcher(url);
  }
  return res.json();
};

