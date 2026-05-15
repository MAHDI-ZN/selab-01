# Selab01
Group Members:
Mehdi Mohammadi 400105239
Alireza Sohrabi400105047


# پاسخ سؤالات Git

## پوشه‌ی `.git` چیست؟ چه اطلاعاتی در آن ذخیره می‌شود؟ با چه دستوری ساخته می‌شود؟

پوشه‌ی `.git` هسته‌ی اصلی مخزن Git است و تمام اطلاعات مربوط به کنترل نسخه در آن نگهداری می‌شود. این پوشه معمولاً مخفی است و در ریشه‌ی پروژه قرار دارد.

اطلاعاتی که در `.git` ذخیره می‌شود شامل موارد زیر است:

- تاریخچه‌ی commit ها
- branch ها
- tag ها
- تنظیمات repository
- اطلاعات remote ها
- stash ها
- object ها و snapshot فایل‌ها

این پوشه با دستور زیر ساخته می‌شود:

```bash
git init
```

مثال:

```bash
mkdir my-project
cd my-project
git init
```

---

## منظور از atomic بودن در atomic commit و atomic pull-request چیست؟

منظور از atomic بودن این است که تغییرات مرتبط به صورت کامل و مستقل در یک واحد منطقی ثبت شوند.

### Atomic Commit

در atomic commit هر commit باید فقط یک هدف مشخص داشته باشد و تغییرات نامرتبط در آن قرار نگیرند.

مثال خوب:

- یک commit فقط برای «رفع باگ لاگین»
- commit دیگر فقط برای «تغییر ظاهر صفحه»

مثال بد:

- ترکیب رفع باگ، تغییر CSS و اضافه کردن feature جدید در یک commit

مزایا:

- rollback ساده‌تر
- review راحت‌تر
- فهم بهتر تاریخچه پروژه

### Atomic Pull Request

در atomic pull-request کل PR باید تنها یک feature یا تغییر مشخص را پوشش دهد.

مثال:

- PR فقط برای اضافه کردن سیستم احراز هویت
- نه همزمان احراز هویت + تغییر UI + refactor دیتابیس

---

## تفاوت دستورهای `fetch` و `pull` و `merge` و `rebase` و `cherry-pick`

### `git fetch`

تغییرات remote را دریافت می‌کند ولی branch فعلی را تغییر نمی‌دهد.

```bash
git fetch origin
```

---

### `git pull`

معادل:

```bash
git fetch + git merge
```

تغییرات را دریافت و مستقیماً با branch فعلی ادغام می‌کند.

```bash
git pull origin main
```

---

### `git merge`

دو branch را با حفظ تاریخچه ترکیب می‌کند.

```bash
git merge feature
```

اگر لازم باشد merge commit ساخته می‌شود.

---

### `git rebase`

commit های branch فعلی را روی branch دیگری بازنویسی می‌کند تا تاریخچه خطی‌تر شود.

```bash
git rebase main
```

مزیت:
- تاریخچه تمیزتر

عیب:
- بازنویسی تاریخچه انجام می‌دهد

---

### `git cherry-pick`

فقط یک commit خاص را از branch دیگر اعمال می‌کند.

```bash
git cherry-pick <commit-hash>
```

مثال:
انتقال فقط یک bug fix بدون merge کامل branch.

---

## تفاوت دستورهای `reset` و `revert` و `restore` و `switch` و `checkout`

### `git reset`

اشاره‌گر branch و stage را به commit قبلی برمی‌گرداند.

```bash
git reset --hard HEAD~1
```

ممکن است تاریخچه را تغییر دهد.

---

### `git revert`

اثر یک commit را با ساخت commit جدید خنثی می‌کند.

```bash
git revert <commit>
```

تاریخچه حفظ می‌شود.

---

### `git restore`

برای بازگرداندن فایل‌ها استفاده می‌شود.

```bash
git restore file.txt
```

یا:

```bash
git restore --staged file.txt
```

---

### `git switch`

برای جابه‌جایی بین branch ها:

```bash
git switch dev
```

یا ساخت branch جدید:

```bash
git switch -c feature
```

---

### `git checkout`

دستور قدیمی و چندمنظوره برای:
- تغییر branch
- بازگردانی فایل
- رفتن به commit خاص

مثال:

```bash
git checkout main
```

امروزه معمولاً `switch` و `restore` به جای آن توصیه می‌شوند.

---

## منظور از stage یا همان index چیست؟ دستور `stash` چه کاری انجام می‌دهد؟

### Stage / Index

stage ناحیه‌ای بین working directory و commit است که مشخص می‌کند چه تغییراتی وارد commit بعدی شوند.

مثال:

```bash
git add file.txt
```

فایل وارد stage می‌شود.

سپس:

```bash
git commit
```

فقط فایل‌های staged ثبت می‌شوند.

---

### `git stash`

تغییرات commit نشده را موقتاً ذخیره می‌کند تا working directory تمیز شود.

```bash
git stash
```

بازگردانی:

```bash
git stash pop
```

کاربرد:
- تغییر branch بدون commit کردن تغییرات فعلی

---

## مفهوم snapshot به چه معناست؟ ارتباط آن با commit چیست؟

در Git هر commit در واقع یک snapshot از وضعیت کل پروژه در آن لحظه است.

برخلاف برخی سیستم‌های کنترل نسخه که فقط تفاوت فایل‌ها را ذخیره می‌کنند، Git وضعیت کامل فایل‌ها را نگهداری می‌کند.

بنابراین:

- هر commit = یک snapshot
- اگر فایلی تغییر نکرده باشد، Git به snapshot قبلی اشاره می‌کند و دوباره آن را کپی نمی‌کند.

مثال:

```text
Commit A -> وضعیت اولیه پروژه
Commit B -> snapshot جدید بعد از تغییر فایل‌ها
Commit C -> snapshot جدید دیگر
```

این ساختار باعث سرعت و یکپارچگی بالای Git می‌شود.

---

## تفاوت‌های `local repository` و `remote repository`

### Local Repository

مخزن موجود روی سیستم شخصی کاربر است.

ویژگی‌ها:
- شامل تمام تاریخچه پروژه
- بدون اینترنت قابل استفاده است
- commit ها ابتدا در local repository ثبت می‌شوند

مثال:

```bash
git commit
```

---

### Remote Repository

نسخه‌ای از repository روی سرور یا سرویس‌هایی مانند GitHub است.

ویژگی‌ها:
- برای همکاری تیمی استفاده می‌شود
- بین افراد synchronize می‌شود
- معمولاً با `push` و `pull` به‌روزرسانی می‌شود

مثال:

```bash
git push origin main
```

نمونه remote repository:
- GitHub
- GitLab
- Bitbucket
