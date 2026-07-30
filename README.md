# Менеджер модов Baldur's Gate 3 (Русская локализация)

[![Sponsor](https://img.shields.io/badge/Sponsor-%E2%9D%A4%EF%B8%8F-red)](https://github.com/sponsors/laurenwilcox)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-%E2%98%95-yellow)](https://ko-fi.com/laurenwilcox)

Менеджер модов для [Baldur's Gate 3](https://store.steampowered.com/app/1086940/Baldurs_Gate_3/) с полным русским интерфейсом.

**Это русская локализация официального** [LaughingLeader/BG3ModManager](https://github.com/LaughingLeader/BG3ModManager). Оригинальный репозиторий — единственный официальный источник.

## 🇷🇺 Русификация

- **249 переведённых строк:** меню, тултипы, диалоги, алерты
- **Полная поддержка русского интерфейса:** все пункты меню, горячие клавиши, окна настроек
- **Основа:** коммит fc35358 (18 мая 2025)
- **Сборка:** одной командой через `setup_bg3_ru.ps1`

## ⚙️ Установка

### Требования

- Windows 10/11
- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) или Build Tools 2022
- Git

### Быстрый старт (рекомендуется)

Скачай `setup_bg3_ru.ps1` и запусти:

```powershell
powershell -ExecutionPolicy Bypass -File setup_bg3_ru.ps1
```

Скрипт автоматически:
1. Клонирует репозиторий
2. Качает утилиты gplex/gppg с Amazon S3
3. Применяет патч русификации
4. Собирает проект через MSBuild
5. Очищает кэш горячих клавиш

Результат: `Desktop\BG3_ru\BG3ModManager\bin\Release\BG3ModManager.exe`

### Ручная сборка

```powershell
git clone https://github.com/ТВ_ЛОГИН/BG3ModManager-Russian.git
cd BG3ModManager
git checkout fc35358
git submodule update --init --recursive

# Скачай gplex/gppg (см. оригинальный README)
# Примени патч
git apply russify-full-fc35358.patch

# Собери
msbuild BG3ModManager.sln /p:Configuration=Release /p:GeneratePackageOnBuild=false /m
```

## 🎮 Использование

1. **Запусти программу:**
   ```
   bin\Release\BG3ModManager.exe
   ```

2. **Первый запуск — укажи пути:**
   - Settings → Preferences
   - Game Executable Path → `Baldur's Gate 3\bin\x64\bg3.exe`
   - Game Data Path → `Baldur's Gate 3\Data`

3. **Организуй моды:**
   - Перетаскивай моды в список "Активные моды"
   - Порядок важен! Зависимости должны быть выше зависимых
   - Проверяй красные треугольники — это значит не хватает чего-то

4. **Экспортируй в игру:**
   - File → Export Order to Game
   - Или нажми первую зелёную кнопку в тулбаре

5. **Запусти игру и наслаждайся!**

## 📋 Основные функции

- 🎚️ **Перестановка модов** — drag-and-drop интерфейс, множественный выбор
- 📝 **Сохранение порядков** — экспорт в JSON для обмена с друзьями
- 📊 **Экспорт в текст** — красивые таблицы для документов и скриншотов
- 🔎 **Фильтрация** — поиск по названию, автору, типу мода
- 📦 **Импорт/экспорт архивов** — делись полным сетапом модов
- 🎯 **Импорт из сохранений** — возьми порядок из завершённого сохранения
- 🔗 **Быстрые ссылки** — кнопки на все папки: моды, мастерская, логи, настройки
- 🌙 **Светлая и тёмная тема** — выбирай по вкусу

## ⚙️ Функции для авторов модов

- 🗜️ **Распаковка модов** — извлеки .pak одной кнопкой
- 📌 **Копирование UUID/FolderName** — для скрипт-экстендера
- 🏷️ **Пользовательские теги** — задавай теги в `meta.lsx` — они отобразятся
- 📦 **Генератор версий** — инструмент для правильной нумерации версий

## 📁 Структура проекта

```
localization/
  ├── russify-full-fc35358.patch      # Суммарный патч (XAML + C# + меню)
  ├── setup_bg3_ru.ps1                # One-click сборка с нуля
  ├── translations.json               # Память переводов
  ├── extract_strings.py              # Извлечение строк из исходников
  ├── build_translations.py           # Валидация и сборка translations.json
  ├── apply_translations.py           # Применение переводов к файлам
  └── translate_appkeys.py            # Перевод меню (AppKeys.cs)

src/
  ├── GUI/                            # WPF интерфейс (переведён)
  ├── Core/                           # Ядро (LSLib wrapper)
  └── Toolbox/                        # Утилиты

External/
  ├── lslib/                          # LSLib (для чтения pak-файлов)
  └── CrossSpeak/                     # Озвучка текста
```

## 🔄 Обновления апстрима

Когда автор выпустит новую версию:

```powershell
cd BG3ModManager
git fetch upstream
git rebase upstream/master ru

# Если конфликты — редактируешь вручную, потом:
git rebase --continue

# Пересоздаёшь переводы (если чего-то добавилось):
python localization/extract_strings.py .
python localization/build_translations.py
python localization/translate_appkeys.py .
python localization/apply_translations.py .

# Собираешь
msbuild BG3ModManager.sln /p:Configuration=Release /p:GeneratePackageOnBuild=false /m

# Пушишь
git push origin ru
```

## ⚠️ Важно

- **Не кладь моды в подпапки** `%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods` — это сбросит твой порядок
- **Убедись, что Game Data Path указан правильно** — куда лежат Gustav.pak и остальные файлы
- **Выбери кампанию** — игра должна иметь экспортированный порядок для Main (Public)
- **Если modsettings.lsx сбрасывается** — один из твоих модов крашится; см. логи

## 💝 Поддержка проекта

Если тебе нравится менеджер и ты хочешь помочь развитию русской локализации:

- ⭐ **[GitHub Sponsors](https://github.com/sponsors/laurenwilcox)** — регулярная поддержка на GitHub
- ☕ **[Ko-fi (Buy Me a Coffee)](https://ko-fi.com/LaughingLeader)** — отправить кофе разработчику оригинального проекта
- 🐙 **GitHub Star** — звезда репо помогает видимости

**Спасибо за поддержку! 🙏**

## 🔗 Ссылки

- [Оригинальный репозиторий](https://github.com/LaughingLeader/BG3ModManager)
- [Список изменений](https://github.com/LaughingLeader/BG3ModManager/wiki/Changelog)
- [Дискорд сообщества](https://discord.gg/j5gp6MD)
- [Поддержать разработчика (Ko-fi)](https://ko-fi.com/LaughingLeader)

## 📝 Благодарности

- **LaughingLeader** — менеджер модов и его активная поддержка
- **Norbyte** — [LSLib](https://github.com/Norbyte/lslib) для работы с pak-файлами
- **Larian Studios** — Baldur's Gate 3

## 📄 Лицензия

MIT (как оригинальный проект). Смотри файл [LICENSE](LICENSE).

---

**Русская локализация:** основана на коммите fc35358, полностью совместима с оригиналом. Для обновления до свежей версии LaughingLeader используй команды выше.

Приятной игры! 🐉⚔️
