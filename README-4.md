# Five layers - site

Три файла:

- `index.html` - лендинг
- `course.html` - кабинет курса (вход по почте, нужен Supabase)
- `SETUP.md` - как подключить Supabase

## Что заменить перед первой продажей

В `index.html`, блок оплаты:
- `TXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX` - адрес кошелька USDT TRC-20
- `paypal.me/YOURNAME` - твоя ссылка PayPal

В `course.html`, самый верх скрипта:
- `SUPABASE_URL` и `SUPABASE_ANON`

Кнопка Log in в шапке лендинга закомментирована. Найди `LOGIN: unhide` и убери комментарий, когда Supabase заработает.

## Локальный просмотр кабинета без входа

Открой `course.html#demo`.
