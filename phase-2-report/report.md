<div dir="rtl" style="font-family: 'B Nazanin', 'Vazir', sans-serif; line-height: 1.8; text-align: justify; padding: 10px;">

# گزارش پیشرفت: فاز ۲ پروژه 

## ۱. معرفی اولیه مجموعه‌داده و تعریف مسئله
مسئله پیش‌رو یک پروژه <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Multi-class Classification</span> است که در آن هدف اصلی، پیش‌بینی صحیح کلاس متغیر هدف گسسته به نام <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">y</span> می‌باشد. این متغیر شامل ۴ کلاس مجزا (کلاس‌های ۱، ۲، ۳ و ۴) است که وضعیت نهایی یک رخداد ثبت‌شده در سامانه را توصیف می‌کند.

مجموعه‌داده ورودی شامل ویژگی‌هایی نام‌گذاری‌شده از قبیل <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_1</span> تا <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_34</span> است. تمام تحلیل‌های انجام‌شده در این مرحله صرفاً بر پایه رفتار ساختاری، آماری و محاسباتی خود داده‌ها استوار است.

---

## ۲. بررسی اولیه داده‌ها و تحلیل اکتشافی (EDA)
برای شناخت ساختار توزیع ماتریس ویژگی‌ها و متغیر هدف، بررسی‌های آماری اولیه روی داده‌های خام صورت گرفت تا میزان توازن کلاس‌ها و کیفیت داده‌های عددی ورودی ارزیابی شود.

### الف) توزیع متغیر هدف (<span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">y</span>)
بررسی توزیع کلاس‌های چهارگانه نشان می‌دهد که مجموعه‌داده دچار چالش <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Class Imbalance</span> است. بیش از ۴۵ درصد از نمونه‌ها به یک کلاس خاص اختصاص دارند، در حالی که دسته‌های ۳ و ۴ کمترین سهم از فراوانی را دارا هستند. این عدم توازن نقشی تعیین‌کننده در فازهای بعدی ایفا خواهد کرد؛ چرا که مدل‌های آماری به طور طبیعی تمایل دارند به سمت کلاس اکثریت سوگیری داشته باشند.

![توزیع کلاس‌ها](y_dist.png)


### ب) تحلیل نرخ داده‌های مفقود (Missing Values)
پیش از آغاز هرگونه فرآیند اصلاح، یک ارزیابی ساختاری روی درصد فیلدهای پوچ (<span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Null</span>) در تمامی ویژگی‌ها انجام شد. نتایج آماری نشان داد که توزیع داده‌های مفقود در سراسر ماتریس به هیچ وجه یکنواخت نیست. بخش بزرگی از مجموعه‌داده در سلامت کامل عددی قرار دارد، در حالی که چند ستون متمایز، نرخ گمشدگی بالای 25 درصد را ثبت کرده‌اند که فرآیند تصمیم‌گیری آماری درباره آن‌ها در بخش پیش‌پدازش مستند شده است.

---

## ۳. کارهای انجام‌شده در زمینه پاک‌سازی و پیش‌پیش‌پردازش داده‌ها
بر اساس خروجی‌های به‌دست آمده از بخش تحلیل اکتشافی، یک <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Pipeline</span> پیش‌پردازش متمرکز و چندمرحله‌ای طراحی و روی ماتریس ویژگی‌ها اعمال شد تا سازگاری داده‌ها با معیارهای ریاضی یادگیری ماشین تضمین شود:

*   **حذف ستون‌های با نرخ گمشدگی بالا:** ستون‌های عددی <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_5, f_14, f_20</span> به دلیل دارا بودن نرخ مفقودی بالا و ثبت فیلدهای پوچ بالای 25٪، به‌طور کامل از ماتریس ویژگی‌ها کنار گذاشته شدند. از آنجا که حجم اطلاعات موجود در این ستون‌ها ناچیز بود، هرگونه تلاش برای بازسازی یا <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Imputation</span> داده‌ها منجر به تزریق نویز شدید و سوگیری‌های مصنوعی در الگوریتم می‌شد.
*   **جایگذاری مقادیر مفقود:** در سایر ستون‌های عددی باقی‌مانده که نرخ داده‌های گمشده آن‌ها بسیار ناچیز و در حد چند درصد بود، راهبرد جایگذاری با <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Mean</span> همان ستون اعمال شد تا ساختار ابعادی ردیف‌ها و حجم کلی نمونه‌های در دسترس حفظ شود.
حالا که مقادیر عددی کاملا پر شده اند تنها ستون های مقادیر خالی غیرعددی به جای میمانند که سرجمع در 3% رکورد های داده ها وجود دارند، به دلیل ناچیز بودن این مقدار بهتر بود که از این سطر ها صرف نظر کنیم.
*   **حذف ویژگی‌های متنی اضافه و ثابت:** ستون<span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_11</span> از <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Pipeline</span> محاسباتی کنار گذاشته شد چون تنها دارای مقدار <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">US</span>بود.
ویژگی‌های بدون واریانس توانایی تفکیک‌کنندگی ندارند و نگهداری آن‌ها تنها باعث افزایش بیهوده ابعاد مسئله می‌شود.
همچنین ستون های <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_7, f_8</span> کنار گذاشته شدند چون این مقادیر متنی غیرساختاریافته تنها با روش های <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">LLM, NLP</span> قابل استفاده اطلاعاتی هستند که در این درس مورد بحث نیستند.

* **استخراج ویژگی‌های زمانی:** به‌منظور تبدیل اطلاعات متنی موجود در ستون‌های <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f2</span> و <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f13</span> به ویژگی‌های عددی قابل استفاده، ویژگی‌های زمانی <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">season, duration_min, hour, day_of_week</span> از این ستون‌ها استخراج شدند. علاوه بر این، میزان ارتباط و تأثیر هر یک از این ویژگی‌های استخراج‌شده بر متغیر هدف نیز مورد بررسی قرار گرفت تا تنها ویژگی‌های مؤثر و معنادار در فرآیند مدل‌سازی مورد استفاده قرار گیرند.

*   **نحوه حذف ستون‌های شرایط زمانی دوگانه:** 
    ستون‌های پایانی مجموعه‌داده شامل <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_31, f_32, f_33, f_34</span> که مربوط به موقعیت‌های زمانی شب و روز در شرایط مختلف بودند، به‌طور کامل حذف شدند. با بررسی ماتریس همبستگی مشخص شد که این چهار ستون <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Multicollinearity</span> شدید و تداخل اطلاعاتی کاملی با سایر ویژگی‌های زمانی دارند؛ بنابراین برای جلوگیری از افزونگی داده‌ها و کاهش ابعاد غیرضروری، از <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Pipeline</span> پردازش کنار گذاشته شدند.

![ماتریس هم‌خطی ویژگی‌های f31 تا f34](hour_distributions.png)

* **کدگذاری ویژگی‌های دسته‌ای:** ستون‌های متنی از نوع <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">categorical</span> بر اساس تعداد مقادیر یکتای خود به دو گروه تقسیم شدند. برای ستون‌هایی با تعداد دسته‌های زیاد، از روش <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">frequency encoding</span> استفاده شد تا ضمن حفظ اطلاعات توزیع داده‌ها، از افزایش بیش از حد ابعاد مجموعه ویژگی‌ها جلوگیری شود. در مقابل، ستون‌هایی با تعداد دسته‌های معقول با استفاده از روش <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">one-hot encoding</span> به ویژگی‌های عددی تبدیل شدند تا مدل بتواند آن‌ها را بدون ایجاد ترتیب مصنوعی میان دسته‌ها پردازش کند.

* **نرمال‌سازی ویژگی‌ها:** به‌منظور یکسان‌سازی مقیاس ویژگی‌های عددی و بهبود فرآیند آموزش مدل، مقادیر ستون‌ها با استفاده از روش <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">StandardScaler</span> از کتابخانه <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">sklearn</span> نرمال‌سازی شدند. در این روش، هر ویژگی بر اساس میانگین و انحراف معیار خود استاندارد شده و به توزیعی با میانگین صفر و انحراف معیار یک تبدیل می‌شود. این کار موجب افزایش پایداری عددی، تسریع همگرایی الگوریتم‌های یادگیری و بهبود عملکرد مدل‌هایی می‌شود که به مقیاس ویژگی‌ها حساس هستند.

*   **حذف ردیف‌های تکراری:** ردیف‌های کاملاً مشابه و همسان که فاقد هرگونه ارزش اطلاعاتی جدید بوده و صرفاً باعث <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Overfitting</span> روی نمونه‌های خاص می‌شدند، شناسایی و از مجموعه داده حذف شدند.



---

## ۴. مشکلات و ابهامات
*   **محدودیت تحلیل شهودی:** پنهان بودن ماهیت ستون‌ها از <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_1</span> تا <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">f_34</span> امکان مهندسی ویژگی‌های کیفی مبتنی بر منطق یا ترکیب شهودی ستون‌ها را سلب کرده است. این امر ما را ناچار می‌سازد تا در مراحل پردازشی کاملاً به روش‌های تغییرات ریاضی، تحلیل‌های آماری پیشرفته و روش‌های خودکار کاهش ابعاد متکی بمانیم.
*   **چالش عدم تقارن داده‌ها:** وجود چالش عدم تعادل شدید در متغیر هدف به این معنی است که الگوهای مربوط به کلاس‌های ۳ و ۴ بسیار کمتر در دسترس هستند. این توزیع نامتوازن به عنوان یکی از چالش‌های اصلی ساختاری داده‌های ورودی ثبت شد که در طراحی <span dir="ltr" style="font-family: inherit; font-size: inherit; color: inherit;">Pipeline</span>های بعدی باید مستقیماً مدیریت شود.

</div>