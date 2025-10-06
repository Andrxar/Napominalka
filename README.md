команды для запуска на Codespace:
#!/bin/bash
npx expo start --tunnel


================================================

Отлично! 😎 Давай сделаем **супер-подробную пошаговую инструкцию**, чтобы собрать APK из твоего проекта natively.dev через EAS Build, **без Android Studio и без лишних мучений**.

---

# 📌 Полная инструкция: сборка APK через EAS Build

Конечно! Вот полностью переписанная и обновлённая **пошаговая инструкция по сборке APK через EAS Build**, с учётом всех исправлений `app.json` и `eas.json`, а также последних нюансов:

---

📌 **Полная инструкция: сборка APK через EAS Build**

---

### 1️⃣ Создание аккаунта Expo / EAS

1. Зайди на [https://expo.dev](https://expo.dev).
2. Нажми **Sign Up** → создай аккаунт через **GitHub**, **Google** или **Email**.
3. Подтверди Email.
4. Запомни Email и пароль — они понадобятся для EAS CLI.

⚡ **Примечание:** не нужно загружать код проекта на сайт. Сборка будет идти из локального проекта или Codespaces.

---

### 2️⃣ Установка EAS CLI

В терминале твоего проекта (в Codespaces или локально) выполни:

```bash
npm install -g eas-cli
```

Проверка установки:

```bash
eas --version
```

Должна появиться версия CLI, например `12.0.0`.

---

### 3️⃣ Логин в EAS CLI

Выполни:

```bash
eas login
```

Введи Email и пароль от аккаунта Expo.
Если всё верно → CLI готов к сборке.

---

### 4️⃣ Подготовка проекта к сборке

1. Убедись, что в проекте есть `app.json`.
2. Измени его так, чтобы профиль **production** собирал APK.

Полный пример исправленного `app.json`:

```json
{
  "expo": {
    "name": "Natively",
    "slug": "Natively",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/images/natively-dark.png",
    "userInterfaceStyle": "light",
    "newArchEnabled": true,
    "platforms": ["android"],
    "splash": {
      "image": "./assets/images/natively-dark.png",
      "resizeMode": "contain",
      "backgroundColor": "#000000"
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/images/natively-dark.png",
        "backgroundColor": "#000000"
      },
      "edgeToEdgeEnabled": true,
      "package": "com.anonymous.Natively",
      "permissions": [
        "RECEIVE_BOOT_COMPLETED",
        "VIBRATE",
        "WAKE_LOCK",
        "SYSTEM_ALERT_WINDOW",
        "USE_FULL_SCREEN_INTENT",
        "SCHEDULE_EXACT_ALARM",
        "USE_EXACT_ALARM"
      ]
    },
    "plugins": [
      "expo-font",
      "expo-router",
      [
        "expo-notifications",
        {
          "icon": "./assets/images/natively-dark.png",
          "color": "#4A90E2",
          "defaultChannel": "reminders",
          "sounds": ["./assets/sounds/notification.wav"]
        }
      ]
    ],
    "scheme": "natively",
    "experiments": {
      "typedRoutes": true
    },
    "extra": {
      "router": {}
    }
  }
}
```

---

### 5️⃣ Настройка `eas.json`

Исправленный `eas.json` для сборки:

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal",
      "autoIncrement": true
    },
    "preview": {
      "distribution": "internal",
      "autoIncrement": true
    },
    "production": {
      "autoIncrement": true,
      "android": {
        "buildType": "apk"
      }
    }
  }
}
```

✅ **Комментарий:**

* `development` и `preview` — для тестирования через Expo Go.
* `production` — генерирует готовый APK.
* Нет запрещённых полей `platform`.

---

### 6️⃣ Инициализация EAS Build в проекте

В терминале проекта:

```bash
eas build:configure
```

1. CLI проверит `app.json`.
2. Спрашивает: **Use managed workflow?** → выбираем **Yes**, т.к. проект natively.dev использует Expo Managed Workflow.
3. EAS создаст файл `eas.json` или обновит существующий.

---

### 7️⃣ Запуск сборки APK

В терминале:

```bash
eas build --platform android --profile production
```

1. CLI может спросить про **app version source** → выбираем **remote** (рекомендуется).
2. Начнётся облачная сборка.
3. В терминале появится ссылка на билд:

```
Build started. Track the build at:
https://expo.dev/accounts/<username>/projects/Natively/builds/<id>
```

Открой ссылку в браузере, чтобы отслеживать процесс.
⏱ Время сборки: обычно 5–15 минут, зависит от нагрузки сервера.

---

### 8️⃣ Скачивание APK

После завершения сборки на странице билда появится кнопка **Download APK**.
Кликаешь → APK скачивается на компьютер.

⚡ **Примечание:** APK уже готов к установке на телефон, Android Studio не нужен.

---

### 9️⃣ Установка APK на телефон

1. Перенеси APK на телефон (Google Drive, Telegram, почта и т.д.).
2. Включи установку приложений из неизвестных источников (если Android 8+).
3. Установи APK и запусти приложение.

---

### 🔟 Последующие изменения в коде

1. Меняешь код в Codespaces или локально.
2. Сохраняешь (Ctrl+S или автосохранение).
3. Для мгновенного просмотра изменений на телефоне → используешь **Expo Go** и:

```bash
npx expo start --tunnel
```

4. Для нового APK → повторяешь шаги **7–8** (сборка через EAS Build).

---

### ✅ Полезные советы

* Для быстрых тестов можно использовать профиль **preview**:

```bash
eas build --platform android --profile preview
```

* Это даст тестовый APK без полной подготовки production.
* EAS Build автоматически инкрементирует версии (`versionCode`).
* Все плагины natively.dev (expo-notifications, expo-router) работают в облачном билде.

---

Если хочешь, я могу прямо составить **готовый шаблон `.gitignore` и команд для Codespaces**, чтобы после закрытия контейнера твой проект не терял изменения и сразу был готов к сборке.

Хочешь, чтобы я это сделал?
