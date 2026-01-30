# Django Google OAuth — полный проект

Готовый минимальный проект Django с поддержкой входа через Google (OAuth2) с использованием **django-allauth**.

---

## Структура проекта

```
django_google_oauth/
├── README.md
├── requirements.txt
├── .env.example
├── manage.py
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── core/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   └── templates/
│       └── core/
│           ├── base.html
│           └── home.html
└── templates/
    └── account/
        └── login.html
```

---

## Установка и запуск

### 1. Создайте виртуальное окружение

```bash
python -m venv venv
source venv/bin/activate    # на Windows: venv\Scripts\activate
```

### 2. Установите зависимости

```bash
pip install -r requirements.txt
```

### 3. Настройте переменные окружения

Скопируйте `.env.example` в `.env` и заполните необходимые данные:

```bash
cp .env.example .env
```

Отредактируйте `.env`:

```env
SECRET_KEY=your-generated-secret-key-here
DEBUG=True
ALLOWED_HOSTS=127.0.0.1,localhost
SOCIAL_AUTH_GOOGLE_CLIENT_ID=your-client-id.apps.googleusercontent.com
SOCIAL_AUTH_GOOGLE_SECRET=your-client-secret
```

### 4. Настройте Google OAuth credentials

1. Перейдите в [Google Cloud Console](https://console.cloud.google.com/)
2. Создайте новый проект или выберите существующий
3. Включите Google+ API
4. Перейдите в **APIs & Services** → **Credentials**
5. Нажмите **Create Credentials** → **OAuth client ID**
6. Выберите тип приложения: **Web application**
7. Добавьте авторизованные redirect URIs:
   - `http://localhost:8000/accounts/google/login/callback/`
   - `http://127.0.0.1:8000/accounts/google/login/callback/`
8. Скопируйте **Client ID** и **Client Secret** в файл `.env`

### 5. Примените миграции

```bash
python manage.py migrate
```

### 6. Создайте суперпользователя

```bash
python manage.py createsuperuser
```

### 7. Настройте Social Application в админке

Запустите сервер:

```bash
python manage.py runserver
```

Перейдите в админку: `http://localhost:8000/admin/`

1. Войдите с учетными данными суперпользователя
2. Перейдите в **Sites** → выберите `example.com` и измените:
   - Domain name: `localhost:8000`
   - Display name: `localhost`
3. Перейдите в **Social applications** → **Add social application**
4. Заполните форму:
   - **Provider**: Google
   - **Name**: Google OAuth
   - **Client id**: (скопируйте из `.env`)
   - **Secret key**: (скопируйте из `.env`)
   - **Sites**: выберите `localhost:8000`
5. Сохраните

### 8. Протестируйте вход

Откройте `http://localhost:8000/` и нажмите **Login with Google**

---

## Типичные проблемы

### Ошибка: Site matching query does not exist

**Решение**: Убедитесь, что в админке создан Site с ID=1 (проверьте `SITE_ID = 1` в `settings.py`)

### Ошибка: redirect_uri_mismatch

**Решение**: Проверьте, что в Google Cloud Console добавлены правильные Redirect URIs:
- `http://localhost:8000/accounts/google/login/callback/`

### Кнопка "Login with Google" не работает

**Решение**: Убедитесь, что Social Application создано в админке и привязано к правильному Site

---

## Дополнительные возможности

Этот проект можно расширить:

- ✅ Добавить Dockerfile и docker-compose
- ✅ Интеграция с React/Next.js через JWT
- ✅ Улучшенный дизайн страницы входа
- ✅ Поддержка других провайдеров (Facebook, GitHub)
- ✅ Профиль пользователя с дополнительными полями

---

## Лицензия

MIT License - свободно используйте для своих проектов!
