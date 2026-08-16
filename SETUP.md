# Подключение админки (Supabase)

Три шага, минут пятнадцать. Оба файла (`index.html`, `course.html`) кладёшь рядом.

## 1. Создать проект

supabase.com → New project. Регион ближе к тебе, пароль базы сохрани.

## 2. Создать таблицу доступа

Открой SQL Editor и выполни:

```sql
create table access_list (
  email text primary key,
  granted_at timestamptz default now(),
  note text
);

alter table access_list enable row level security;

create policy "read own row"
on access_list for select
to authenticated
using (lower(email) = lower(auth.jwt() ->> 'email'));
```

Политика важна: без неё либо никто не увидит свою строку, либо все увидят чужие.

## 3. Вставить ключи

Project Settings → API. Копируешь `Project URL` и `anon public` key, вставляешь в начало `course.html`:

```js
const SUPABASE_URL  = "https://xxxx.supabase.co";
const SUPABASE_ANON = "eyJhbGciOi...";
```

anon-ключ публичный, его видно в коде страницы, это нормально. Доступ защищает политика RLS, а не секретность ключа. `service_role` ключ на сайт не клади никогда.

## 4. Настроить ссылку для входа

Authentication → URL Configuration:
- Site URL: адрес твоего сайта
- Redirect URLs: добавь `https://твойдомен/course.html`

Иначе письмо придёт, но ссылка вернёт ошибку.

## Как выдавать доступ после оплаты

Table Editor → access_list → Insert row → почта покупателя. Всё.

Отзыв доступа — удалить строку.

## Что важно понимать

Письма на бесплатном тарифе Supabase шлёт со своего SMTP и лимитом порядка нескольких штук в час. На старте хватит. Когда пойдёт поток, подключаешь Resend или Postmark в Authentication → SMTP Settings.

Проверка доступа идёт на стороне сервера через RLS, так что подделать её из браузера нельзя.

Курс лежит текстом внутри `course.html`. Кто оплатил, тот может открыть исходник страницы и скопировать содержимое. Полностью это лечится только подгрузкой уроков из базы. Если захочешь, переделаем: уроки в таблицу `lessons`, политика на чтение только для тех, кто в `access_list`.

## Локальная проверка без ключей

Открой `course.html#demo` — кабинет откроется без входа, чтобы посмотреть вёрстку.
