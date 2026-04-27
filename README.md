# 🎯 Online Quiz — Laravel Trivia Application

> A full-stack web application built with Laravel that lets users test their knowledge across multiple trivia categories with a timed quiz experience.

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Setup & Installation](#-setup--installation)
- [Running the Application](#-running-the-application)
- [API Integration](#-api-integration)
- [Categories Implementation](#-categories-implementation)
- [Design Decisions](#-design-decisions)
- [Debugging Tips](#-debugging-tips)
- [Future Improvements](#-future-improvements)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔐 Authentication | User registration and login system |
| 📂 Category Selection | Choose from multiple trivia categories |
| ❓ Dynamic Questions | 50 multiple-choice questions fetched from API |
| ⏱️ Timed Quiz | 30-second countdown timer per question |
| 📊 Results Page | Score, percentage, and Pass/Fail verdict |
| 🔁 Replay Support | Replay the same quiz or pick a new category |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | PHP 8.x — Laravel Framework |
| Database | SQLite (default) / MySQL |
| Frontend | Laravel Blade + Custom CSS |
| HTTP Client | Laravel HTTP Client (Guzzle) |
| External API | [The Trivia API](https://the-trivia-api.com/v2/) |

---

## 📂 Project Structure

```
online-quiz/
├── app/
│   ├── Enums/
│   │   └── QuizCategory.php          # Static category definitions
│   ├── Http/
│   │   └── Controllers/              # Route controllers
│   └── Services/
│       ├── TriviaApiClient.php        # Handles API communication
│       └── Quiz/
│           └── QuizService.php        # Business logic layer
├── config/
│   ├── quiz.php                       # Quiz app configuration
│   └── services.php                   # API credentials & base URL
├── database/
│   └── migrations/                    # Database schema
├── resources/
│   └── views/                         # Blade templates
├── routes/
│   └── web.php                        # Application routes
└── .env.example                       # Environment template
```

**Key Files:**

- `TriviaApiClient.php` — Centralized API request handling with error management
- `QuizService.php` — Quiz orchestration and business logic
- `QuizCategory.php` — Enum-based, static category definitions
- `config/quiz.php` — Question count and quiz settings
- `config/services.php` — External API base URL and key

---

## ⚙️ Configuration

Copy `.env.example` to `.env` and update the following values:

```env
# External Trivia API
TRIVIA_API_URL=https://the-trivia-api.com/v2/
TRIVIA_API_KEY=          # Optional — leave blank for free tier

# Quiz Settings
QUIZ_QUESTION_COUNT=50   # Number of questions per session

# Database (choose one)
DB_CONNECTION=sqlite     # Default
# DB_CONNECTION=mysql
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=online_quiz
# DB_USERNAME=root
# DB_PASSWORD=
```

---

## 🚀 Setup & Installation

> **Prerequisites:** PHP 8.x, Composer, Node.js & npm

### Step 1 — Extract the project

```bash
unzip online-quiz.zip
cd online-quiz
```

### Step 2 — Install dependencies

```bash
composer install
npm install
npm run build
```

### Step 3 — Configure environment

```bash
cp .env.example .env
# Edit .env with your database and API settings
```

### Step 4 — Generate application key

```bash
php artisan key:generate
```

### Step 5 — Run database migrations

```bash
php artisan migrate
```

### Step 6 — Clear and cache config

```bash
php artisan config:clear
php artisan config:cache
```

### Step 7 — Start the development server

```bash
php artisan serve
```

---

## ▶️ Running the Application

1. Open your browser and navigate to: **http://127.0.0.1:8000**
2. Register a new account or log in
3. Select a trivia category
4. Answer 50 questions within 30 seconds each
5. View your final score and result

---

## 🔌 API Integration

This application integrates with **[The Trivia API](https://the-trivia-api.com/v2/)** for dynamic question fetching.

**Architecture:**

```
Request → TriviaApiClient → External API
              ↓
         QuizService        ← Business Logic
              ↓
         Controller → Blade View
```

**Key design choices:**
- API credentials are injected via `config/services.php` (not hardcoded)
- `TriviaApiClient` is a dedicated service class — single responsibility
- Error handling is centralized within the client layer
- Clean separation between the HTTP layer and business logic

---

## 🧠 Categories Implementation

Categories are defined statically using `App\Enums\QuizCategory` rather than fetched from the API.

**Why static enums?**

| Reason | Detail |
|---|---|
| API restriction | The category endpoint requires a paid subscription |
| No external dependency | App works fully without API access for categories |
| Performance | No extra API call on category selection page |

> **Note:** Questions are still fetched dynamically from the API — only category listings are static.

---

## 💡 Design Decisions

| Decision | Rationale |
|---|---|
| Service layer pattern | Keeps controllers thin; separates concerns cleanly |
| Config-driven architecture | Environment-specific values stay out of code |
| Enum-based categories | Type-safe, IDE-friendly, and avoids magic strings |
| Laravel HTTP Client | Built-in retry, timeout, and error handling |
| Rate limiting | Prevents API abuse and improves security |

---

## 🔍 Debugging Tips

**API not returning questions?**
```bash
php artisan config:clear
php artisan config:cache
# Verify TRIVIA_API_URL and TRIVIA_API_KEY in .env
# Restart the server: php artisan serve
```

**Database errors?**
```bash
php artisan migrate:fresh    # Reset and re-run all migrations
php artisan migrate:status   # Check migration state
```

**General issues?**
```bash
php artisan cache:clear
php artisan view:clear
php artisan route:clear
```

---

## ⚡ Future Improvements

- [ ] Admin panel for managing questions and categories
- [ ] Global leaderboard system
- [ ] Difficulty level selection (Easy / Medium / Hard)
- [ ] API response caching to reduce external calls
- [ ] Unit and feature test coverage
- [ ] Progressive Web App (PWA) support

---

## 📦 ZIP Exclusions

To keep the archive size small, the following are excluded and must be regenerated after extraction:

```
/vendor              → composer install
/node_modules        → npm install
```

---

## 🙌 Summary

This project demonstrates clean Laravel architecture with:

- **Service layer pattern** for maintainable API integration
- **Config-driven design** for environment flexibility
- **Enum-based category management** for type safety
- **Separation of concerns** across controllers, services, and views
