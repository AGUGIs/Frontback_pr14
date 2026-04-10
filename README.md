# Практическое занятие №14 — Web App Manifest

## Цель работы

Доработать веб-приложение, добавив поддержку Web App Manifest для превращения сайта в прогрессивное веб-приложение (PWA) с возможностью установки на устройство.

## Что было сделано

### 1. Создан файл `manifest.json` — `client/public/manifest.json`

Манифест содержит все рекомендуемые поля:

| Поле | Значение | Назначение |
|------|----------|------------|
| `name` | КастрюляМаркет — Магазин кастрюль и посуды | Полное название приложения |
| `short_name` | КастрюляМаркет | Короткое название (отображается под иконкой) |
| `start_url` | `/` | URL, открываемый при запуске |
| `display` | `standalone` | Запуск в отдельном окне без интерфейса браузера |
| `background_color` | `#0b0f19` | Цвет фона при загрузке (splash screen) |
| `theme_color` | `#6366f1` | Цвет темы (адресная строка, панель задач) |
| `description` | Описание магазина | Текстовое описание приложения |
| `orientation` | `portrait-primary` | Портретная ориентация экрана |
| `icons` | 7 иконок (16–512px) | Набор иконок разных размеров |

### 2. Подготовлен набор иконок — `client/public/icons/`

Созданы PNG-иконки следующих размеров:

- `favicon-16x16.png` — для вкладки браузера
- `favicon-32x32.png` — для вкладки браузера (Retina)
- `favicon-48x48.png` — для панели задач
- `favicon-64x64.png` — для ярлыков
- `favicon-128x128.png` — для iOS (apple-touch-icon)
- `favicon-256x256.png` — для Android
- `favicon-512x512.png` — для splash screen (`purpose: maskable any`)
- `favicon.ico` — стандартный favicon

Все иконки выполнены в тематике приложения (кастрюля на оранжевом фоне).

### 3. Подключён манифест в HTML — `client/public/index.html`

В `<head>` файла `index.html` добавлены:

```html
<!-- Web App Manifest -->
<link rel="manifest" href="%PUBLIC_URL%/manifest.json" />

<!-- Favicon разных размеров -->
<link rel="icon" href="%PUBLIC_URL%/icons/favicon.ico" sizes="16x16" type="image/x-icon" />
<link rel="icon" type="image/png" sizes="32x32" href="%PUBLIC_URL%/icons/favicon-32x32.png" />
<link rel="icon" type="image/png" sizes="16x16" href="%PUBLIC_URL%/icons/favicon-16x16.png" />
```

### 4. Добавлены мета-теги для мобильных платформ

```html
<!-- Для Android -->
<meta name="mobile-web-app-capable" content="yes" />

<!-- Цвет темы для браузера -->
<meta name="theme-color" content="#6366f1" />

<!-- Для iOS — иконка на домашний экран -->
<link rel="apple-touch-icon" href="%PUBLIC_URL%/icons/favicon-128x128.png" />

<!-- Для iOS — стиль статус-бара -->
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```

Назначение каждого мета-тега:

- `mobile-web-app-capable` — сообщает Android, что приложение может работать как standalone
- `theme-color` — задаёт цвет адресной строки браузера и панели задач
- `apple-touch-icon` — иконка для добавления на домашний экран iOS
- `apple-mobile-web-app-capable` — разрешает запуск в полноэкранном режиме на iOS
- `apple-mobile-web-app-status-bar-style` — стиль статус-бара на iOS (полупрозрачный чёрный)

### 5. Обновлён Service Worker — `client/public/sw.js`

Версия кэша обновлена с `v1` на `v2`. В список кэшируемых ресурсов `ASSETS` добавлены:

- `/manifest.json`
- Все 7 иконок из папки `/icons/` и файл `favicon.ico`

При обновлении SW старый кэш `v1` автоматически удаляется в событии `activate`.

## Структура файлов, относящихся к ПР14

```
client/public/
├── manifest.json                  ← Web App Manifest
├── index.html                     ← Обновлён: <link rel="manifest"> + мета-теги
├── sw.js                          ← Обновлён: кэш v2, иконки и манифест в ASSETS
└── icons/
    ├── favicon.ico                ← ICO для браузера
    ├── favicon-16x16.png          ← Иконка 16×16
    ├── favicon-32x32.png          ← Иконка 32×32
    ├── favicon-48x48.png          ← Иконка 48×48
    ├── favicon-64x64.png          ← Иконка 64×64
    ├── favicon-128x128.png        ← Иконка 128×128 (apple-touch-icon)
    ├── favicon-256x256.png        ← Иконка 256×256
    └── favicon-512x512.png        ← Иконка 512×512 (maskable, splash screen)
```

## Как проверить работу

### Проверка манифеста в DevTools

1. Запустите сервер и клиент:
   ```bash
   # Терминал 1
   cd server && npm install && npm start

   # Терминал 2
   cd client && npm install && npm start
   ```

2. Откройте `http://localhost:3001` в Chrome.

3. Откройте DevTools (F12) → вкладка **Application** → раздел **Manifest**.

4. Убедитесь, что:
   - Все поля манифеста отображаются корректно (name, short_name, start_url и т.д.)
   - Все 7 иконок загружены и отображаются в разделе Icons
   - Нет ошибок и предупреждений

### Проверка установки на компьютер

1. В **Chrome**: в правой части адресной строки появится кнопка установки (значок «+» в круге или иконка монитора). Нажмите её — приложение установится и откроется в отдельном окне без элементов браузера.

2. В **Яндекс.Браузере**: меню → «Дополнительно» → «Добавить страницу на рабочий стол».

3. В **Edge**: меню → «Другие инструменты» → «Приложения» → «Установить этот сайт как приложение».

### Проверка кэширования

1. DevTools → **Application** → **Cache Storage**.
2. Убедитесь, что кэш `kastryula-cache-v2` содержит `manifest.json` и все иконки.

### Аудит Lighthouse

1. DevTools → вкладка **Lighthouse**.
2. Выберите категорию **Progressive Web App**.
3. Запустите аудит — при корректной настройке манифеста и Service Worker будут получены высокие баллы.

## Соответствие требованиям задания

| Требование | Выполнение |
|---|---|
| Создать `manifest.json` с полями name, short_name, start_url, display, background_color, theme_color, description, icons | ✅ Все поля заполнены |
| Подготовить набор иконок (минимум 3) в формате PNG | ✅ 7 иконок (16–512px) |
| Подключить манифест через `<link rel="manifest">` | ✅ В index.html |
| Добавить мета-теги для мобильных платформ | ✅ 5 мета-тегов (Android + iOS) |
| Обновить Service Worker для кэширования иконок и манифеста | ✅ sw.js обновлён до v2 |
| Протестировать установку приложения | ✅ Инструкция выше |
