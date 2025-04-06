# Flutter Web Hosting for Deep Links Configuration

Этот проект представляет собой Flutter Web-приложение, развернутое на Firebase Hosting, основной целью которого является хостинг конфигурационных файлов для работы Deep Links (`assetlinks.json` и `apple-app-site-association`).

## Зачем хостить веб-приложение для Deep Links?

Для работы Deep Links необходимо, чтобы конфигурационные файлы (assetlinks.json и apple-app-site-association) были доступны на сервере. Их размещение на Firebase Hosting или другом веб-хостинге позволяет:

- Подтвердить право собственности на домен.
- Связать мобильное приложение с веб-сайтом.
- Обеспечить надёжную работу Universal Links на iOS и Android App Links.
- Избежать необходимости настраивать редиректы и промежуточные сервисы для обработки ссылок.

Использование Firebase Hosting — это удобный способ быстро развернуть эти файлы без необходимости настраивать собственный сервер.

## 1. Подготовка файла apple-app-site-association (для iOS)

В каталоге `public/.well-known/` находится файл `apple-app-site-association`, в котором необходимо изменить параметр `appID` на уникальный идентификатор вашего приложения, указав свои данные.

### Что такое `appID`?

`appID` — это уникальный идентификатор вашего приложения, который используется для связывания вашего веб-сайта с мобильным приложением через функциональность Universal Links (для iOS). Он состоит из двух частей:

1. **Apple Team ID** — уникальный идентификатор вашей команды разработчиков в Apple Developer Program.
2. **Bundle ID** — уникальный идентификатор вашего приложения на устройстве.

Для примера, если ваш `Apple Team ID` — `ABC123XYZ`, а `Bundle ID` — `com.example.myapp`, то ваш `appID` будет таким:
ABC123XYZ.com.example.myapp

### Измените параметр `appID` в файле `apple-app-site-association` на ваши данные

Пример файла:

```json
{
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "ABC123XYZ.com.example.myapp",
        "paths": ["*"]
      }
    ]
  }
}
```

## 2. Подготовка файла `assetlinks.json` (для Android)

В каталоге `public/.well-known/` создайте файл `assetlinks.json`, в котором необходимо указать настройки для вашего Android-приложения. Пример файла:

```json
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "com.example.myapp",
      "sha256_cert_fingerprints": [
        "A0:B1:C2:D3:E4:F5:G6:H7:I8:J9:K0:L1:M2:N3:O4:P5:Q6:R7:S8:T9:U0:V1:W2:X3:Y4:Z5"
      ]
    }
  }
]
```

### Что такое `package_name` и `sha256_cert_fingerprints`?

- **`package_name`** — это уникальный идентификатор вашего Android-приложения в системе Google Play. Он выглядит как строка в формате `com.example.myapp`. Например, если ваше приложение называется "MyApp", то его `package name` может быть `com.example.myapp`.

- **`sha256_cert_fingerprints`** — это уникальная подпись вашего Android-приложения, которая используется для подтверждения, что запросы с вашего приложения авторизованы. Подпись генерируется с помощью инструмента `keytool`, который позволяет создать SHA-256 подпись для вашего ключа подписи.

Пример с подписью SHA-256:

```
A0:B1:C2:D3:E4:F5:G6:H7:I8:J9:K0:L1:M2:N3:O4:P5:Q6:R7:S8:T9:U0:V1:W2:X3:Y4:Z5
```

### Замените данные:

- `com.example.myapp` на ваш **Package Name**.
- `A0:B1:C2...` на вашу настоящую **SHA-256 подпись**.

Если Firebase CLI уже установлен, вы можете сразу перейти к шагу 5.

## 3. Установка Firebase CLI

Если Firebase CLI не установлен, установите его с помощью команды:

```bash
curl -sL https://firebase.tools | bash
```

## 4. Аутентификация и проверка установки

### Авторизуйтесь в Firebase, выполнив команду:

```bash
firebase login
```
### После успешного входа убедитесь, что Firebase CLI имеет доступ к вашим проектам:

```bash
firebase projects:list
```

## 5. Инициализация проекта
### Подключите файлы локального проекта к Firebase, запустив команду из корневой директории проекта:

```bash
firebase init hosting
```

### Во время инициализации:

1. Выберите Firebase-проект для подключения к локальной директории.
2. Оставьте вариант по умолчанию для корневой директории статики, которая называется `public`. Это будет использоваться для размещения всех ваших статических файлов, таких как `index.html`.
3. Выберите конфигурацию сайта (например, одностраничное приложение, если необходимо).

## 6. Деплой проекта

Чтобы развернуть сайт на Firebase Hosting, выполните команду:

```bash
firebase deploy --only hosting
```

## После завершения развертывания

Firebase предоставит ссылку на опубликованный сайт.

## После успешного деплоя

Убедитесь, что `.json` файлы доступны по хостинговому URL:

- [https://your-domain.com/.well-known/assetlinks.json](https://your-domain.com/.well-known/assetlinks.json)
- [https://your-domain.com/.well-known/apple-app-site-association](https://your-domain.com/.well-known/apple-app-site-association)

## Заключение

Теперь ваше Flutter Web-приложение поддерживает Deep Linking и развернуто на Firebase Hosting! 🚀