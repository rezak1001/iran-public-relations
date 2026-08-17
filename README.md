# مارکت‌پلیس پلاگین‌های روابط عمومی ایران

این مخزن یک **مارکت‌پلیس پلاگین** برای Claude است. کاربران آدرس این مخزن را یک بار
اضافه می‌کنند و از آن پس، هر تغییری که شما اینجا منتشر کنید، برایشان به‌روزرسانی
می‌شود.

## پلاگین موجود

### `iran-public-relations` — روابط عمومی ایران

پنج اسکیل تخصصی که منطق حرفه‌ای جهانی روابط عمومی را بر بستر واقعیت ایران
(تقویم و مناسبت‌ها، نقشه رسانه‌ای، مسیرهای تأیید، تشریفات و خطوط قرمز) اجرا
می‌کنند.

| اسکیل | کِی فعال می‌شود |
|---|---|
| `press-conference-planner` | نشست خبری، کنفرانس مطبوعاتی، دعوت از رسانه‌ها، رونمایی خبری |
| `iran-exhibition-pr` | نمایشگاه، غرفه، غرفه‌سازی، جانمایی، پیش‌ثبت‌نام، بودجه نمایشگاه |
| `event-planner` | افتتاحیه، همایش، سمینار، وبینار، جشن سازمانی، رویداد برندی و اجتماعی |
| `sponsorship-planner` | حمایت مالی و اسپانسرشیپ از رویداد دیگران، فعال‌سازی، بازگشت سرمایه |
| `media-visit-planner` | بازدید رسانه‌ای، تور خبرنگاران از کارخانه، پروژه یا سایت عملیاتی |

---

## نصب — برای کاربران

### در Cowork یا اپ دسکتاپ Claude

۱. منوی **Customize** را باز کنید
۲. تب **Plugins**
۳. در بخش Personal plugins روی دکمه **+** بزنید
۴. گزینه **Add from a repository** را انتخاب کنید
۵. آدرس این مخزن را وارد کنید:

```
https://github.com/rezak1001/iran-public-relations
```

۶. پلاگین **iran-public-relations** را از فهرست نصب کنید

### در Claude Code

```bash
/plugin marketplace add rezak1001/iran-public-relations
/plugin install iran-public-relations@iran-public-relations
```

اگر پس از نصب پیام `Run /reload-plugins to activate` دیدید، همان دستور را اجرا
کنید.

---

## به‌روزرسانی — برای نگهدارنده

کاربران **نیازی به نصب مجدد ندارند**. برای انتشار نسخه جدید:

۱. تغییرات را در `plugins/iran-public-relations/` اعمال کنید
۲. **شماره نسخه را در هر دو فایل بالا ببرید** — این مرحله حیاتی است:
   - `plugins/iran-public-relations/.claude-plugin/plugin.json` → فیلد `version`
   - `.claude-plugin/marketplace.json` → فیلد `version` همان پلاگین
۳. تغییرات را push کنید

> ⚠️ **رشته `version` سیگنال به‌روزرسانی است.** اگر آن را بالا نبرید، کاربران
> نسخه کش‌شده قدیمی را نگه می‌دارند حتی اگر شما push کرده باشید.

قاعده شماره‌گذاری (semver): اصلاح جزئی و رفع اشتباه → `0.1.1` · افزودن اسکیل یا
محتوای جدید → `0.2.0` · تغییر ساختاری که سازگاری را می‌شکند → `1.0.0`.

### افزودن اسکیل جدید

فقط پوشه‌اش را زیر `plugins/iran-public-relations/skills/` بسازید و نسخه را بالا ببرید. کاربران
هیچ کار جداگانه‌ای نمی‌کنند — اسکیل جدید خودش می‌آید.

### به‌روزرسانی — برای کاربران

به‌روزرسانی در پس‌زمینه انجام می‌شود. برای اجبار به بررسی فوری:

```bash
/plugin marketplace update
/plugin update iran-public-relations
```

---

## ساختار مخزن

```
.
├── .claude-plugin/
│   └── marketplace.json          ← کاتالوگ مارکت‌پلیس
└── plugins/
    └── iran-public-relations/
        ├── .claude-plugin/
        │   └── plugin.json       ← مانیفست پلاگین (شماره نسخه اینجاست)
        ├── README.md
        └── skills/
            ├── press-conference-planner/
            ├── iran-exhibition-pr/
            ├── event-planner/
            ├── sponsorship-planner/
            └── media-visit-planner/
```

هر اسکیل شامل `SKILL.md`، پوشه `references/` برای محتوای تفصیلی، پوشه `assets/`
برای قالب‌های آماده، و پوشه `shared/` است.

### درباره پوشه `shared/`

دو فایل مرجع مشترک — `iran-context.md` (تقویم، نقشه رسانه‌ای، تشریفات، خطوط قرمز)
و `sector-profiles.md` (شش پروفایل سازمانی) — عمداً در **هر پنج اسکیل تکرار
شده‌اند**.

دلیلش فنی است: هنگام نصب، پوشه پلاگین کپی می‌شود و ارجاع به فایل‌های بیرون پوشه
اسکیل (مثل `../../shared/`) کار نمی‌کند.

**نکته نگهداری:** اگر محتوای این دو فایل را تغییر دادید، تغییر را در هر پنج پوشه
اعمال کنید. اسکریپت کمکی:

```bash
for s in iran-exhibition-pr event-planner sponsorship-planner media-visit-planner; do
  cp plugins/iran-public-relations/skills/press-conference-planner/shared/*.md \
     plugins/iran-public-relations/skills/$s/shared/
done
```

(نسخه اسکیل نشست خبری را به‌عنوان مرجع ویرایش کنید و بقیه را از رویش کپی کنید.)
