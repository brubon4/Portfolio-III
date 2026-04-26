# Portfolio-III — контекст проекта

## Что это
Портфолио Аминского Сергея (BI-аналитик). Один файл: `index.html`. Никаких фреймворков — чистый HTML/CSS/JS.

## Деплой
- Репозиторий: `brubon4/Portfolio-III`
- Живой сайт: `https://brubon4.github.io/Portfolio-III/`
- GitHub Pages деплоит из ветки `main`
- Рабочая ветка: `claude/continue-portfolio-dev-9LYhG` (мержить в `main` после каждого изменения)

## Git-workflow
- Разрабатывать в ветке `claude/continue-portfolio-dev-9LYhG`
- Коммитить → пушить → мержить в `main` → пушить `main`
- При мерже всегда делать `git pull origin main --no-rebase` перед `git push`, если remote опережает
- После пуша GitHub Pages обновляется за 5–10 минут
- Для проверки без кеша: инкогнито или Ctrl+Shift+R

## Структура файла index.html
Один большой HTML-файл (~1000 строк). Разделы сверху вниз:
1. `<head>` — Google Analytics 4 (G-Y0SSRZTCEV), CSS
2. `<nav>` — навигация + кнопки RU/EN; логотип — ссылка на `#home`
3. `.hero` — главный экран (canvas с частицами, scanline, счётчики)
4. `.about` — обо мне + фото
5. `.experience` — опыт работы (6 мест, хронология)
6. `.skills` — навыки (3 колонки + AI-группа на полную ширину)
7. `.analytics` — стек + блок-схемы проектов (SVG с переключателем)
8. `.ai-tools` — AI-инструменты (6 карточек)
9. `.objects` — ключевые объекты
10. `.education` — образование
11. `.contact` — контакты + форма Formspree
12. `<footer>`

## Фотографии
- **Герой (главный экран):** `AE4F4DD9-C82F-4185-9553-523C86769451.png` (в корне)
- **About (обо мне):** `uploads/Я на стройке фон СИЛЬНАЯ ретушь v3.png`
- Старые файлы `photo_hero.jpg` и `uploads/EC5D65A9-87C7-431A-AAC1-FFC829E0DD31.png` остаются в репозитории, но не используются

## CSS-переменные
```css
--bg: oklch(9% 0.012 250)      /* основной фон */
--bg2: oklch(13% 0.012 250)
--bg3: oklch(17% 0.012 250)
--accent: oklch(62% 0.16 210)  /* голубой акцент */
--accent2: oklch(70% 0.13 55)  /* янтарный */
--font: 'Inter', ...
```

## Переключатель языка RU/EN
- Кнопки `.lang-btn` в nav, справа от ссылок
- На мобильном: `position: absolute; top: 18px; right: 24px`
- Все переводимые элементы имеют атрибут `data-i18n="ключ"`
- Плейсхолдеры инпутов формы: `data-ph-ru` / `data-ph-en`
- Переводы хранятся в двух JS-объектах `window._T` (в конце `<body>`):
  - Первый блок: nav, hero, about, experience (ключи exp1..exp6)
  - Второй блок: skills, analytics, ai-tools, objects, education, contact, footer
- Функция `applyLang(lang)` — переключает всё включая SVG-диаграммы и плейсхолдеры
- Язык сохраняется в `localStorage`

## Блок-схемы проектов (`.analytics`)
- RU: `uploads/portfolio_projects_v4.svg` (680×1160px)
- EN: `uploads/portfolio_projects_v4_en.svg` (полный перевод)
- `<image id="svg-img-1">` и `<image id="svg-img-2">` — href меняется при смене языка
- Переключатель `.proj-switch-btn` показывает/скрывает `.svg-proj` div-ы
- Проект 1: `viewBox="0 0 680 568"`, Проект 2: `viewBox="0 568 680 592"`
- На десктопе (≥901px): `.svg-proj { width: 50%; margin: 0 auto; }`

## Контакты
- Telegram: `https://t.me/Serg_310`
- Email: `art56000@yandex.ru`
- GitHub: `https://github.com/brubon4`
- Форма обратной связи: Formspree `https://formspree.io/f/mgorqvab`
  - Поля: имя, email, сообщение
  - AJAX-отправка (страница не перезагружается)
  - Сообщения приходят на почту владельца

## Google Analytics 4
- Measurement ID: `G-Y0SSRZTCEV`
- Скрипт вставлен в `<head>` (стандартный gtag.js)
- Поток настроен на `https://brubon4.github.io`

## Анимации и эффекты
- `@keyframes shimmer` — sweep слева направо (`.hero-tag::after`, `.skill-tag::after`, `.section-tag::after`)
- `@keyframes shimmer-rare` — с длинной паузой (14s) — для тегов навыков
- `@keyframes scandown` — горизонтальная линия сверху вниз по герою
- `@keyframes pulse` — мигание зелёной точки `.avail-dot` в контактах
- JS: случайные задержки `--sd` для шиммера (Math.random * 4s)
- IntersectionObserver — scroll-reveal для карточек
- Canvas — частицы с соединительными линиями в герое
- Nav: скрывается при скролле вниз (`nav-hidden`), появляется при скролле вверх

## Мобильная версия (≤900px)
- `section { padding: 36px 20px; }`
- Nav: колонка, lang-switch `position: absolute; top: 18px; right: 24px` (НЕ `position: relative` на nav — сломает fixed!)
- Опыт работы — горизонтальный скролл с точками-индикаторами (`.exp-dots`)
- Блок-схемы — полная ширина
- Форма: `.cf-row { flex-direction: column; }`

## Известные нюансы
- `@keyframes shimmer` определён в `<style>` блоке в конце `<body>` (не в `<head>`)
- `.contact-avail` содержит два span: `<span class="avail-dot">` (зелёная точка) и `<span data-i18n>` (текст). CSS таргетит только `.avail-dot`, не все span
- SVG блок-схемы загружены через `<image href="...">` — текст внутри недоступен через DOM, поэтому сделан отдельный EN-файл
- При мерже веток возможен конфликт в index.html — проверять фото героя после каждого мержа
