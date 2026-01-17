# Шахматы по сети: план и спецификация (черновик)

## 1. Цели продукта
Сервис онлайн-шахмат с социальными функциями:
- Игра 1v1 в реальном времени.
- Текстовый чат во время партии.
- Профиль пользователя (фото, описание, рейтинг, история партий).
- Система знакомств: запрос контакта после игры.
- Рейтинг и статистика: Эло, история игр, достижения.
- Опционально: голосовой чат (WebRTC).

## 2. MVP (минимально жизнеспособная версия)
### Этап 1 — “Играть можно”
- Регистрация и авторизация (email + пароль).
- Создание/поиск соперника.
- Реалтайм-игра (ходы, таймер, завершение партии).
- Сохранение результата партии.

### Этап 2 — “Общение”
- Чат в партии (текстовый).
- Публичный профиль пользователя.
- История партий.

### Этап 3 — “Социальность”
- Эло-рейтинг.
- Запросы в контакты после игры.
- Базовая статистика и достижения.

### Этап 4 — “Голос” (опционально)
- WebRTC голосовой чат.
- Модерация/блокировки.

## 3. Архитектура (высокий уровень)
- Frontend: React/Next.js.
- Backend API: Node.js (NestJS) или Go.
- Realtime: WebSocket (Socket.IO/WS).
- БД: PostgreSQL.
- Кэш и очереди: Redis.
- Авторизация: JWT + refresh токены, далее OAuth.
- Голос: WebRTC P2P.

## 4. Основные домены и сущности
### Пользователь и профиль
- User: id, email, password_hash, created_at.
- Profile: user_id, display_name, avatar_url, bio, country.

### Игра
- Game: id, white_user_id, black_user_id, status, result, created_at, ended_at, time_control.
- Move: id, game_id, move_number, uci, fen, created_at.
- ChatMessage: id, game_id, user_id, message, created_at.

### Рейтинг и статистика
- Rating: user_id, rating, updated_at.
- RatingHistory: id, user_id, game_id, delta, rating_after, created_at.
- Achievement: id, code, title, description.
- UserAchievement: user_id, achievement_id, unlocked_at.

### Социальные связи
- ContactRequest: id, from_user_id, to_user_id, status, created_at.
- Contact: user_id, contact_user_id, created_at.

## 5. Основные API (пример)
### Auth
- POST /api/auth/register
- POST /api/auth/login
- POST /api/auth/refresh

### Игры
- POST /api/games (создать игру/матчмейкинг)
- GET /api/games/:id
- POST /api/games/:id/move
- POST /api/games/:id/resign
- GET /api/games/history

### Чат
- WS /ws/games/:id/chat

### Профили
- GET /api/profiles/:id
- PATCH /api/profiles/:id

### Социальные связи
- POST /api/contacts/request
- POST /api/contacts/accept
- GET /api/contacts

## 6. Реалтайм-события (WebSocket)
- game.move
- game.state
- game.chat.message
- presence.online/offline

## 7. Приоритеты безопасности и модерации
- Ограничение на частоту сообщений (rate limiting).
- Модерация чата (простые фильтры, репорты).
- Блокировки пользователей.

## 8. Следующие шаги
1. Подтвердить MVP.
2. Уточнить стек и инфраструктуру.
3. Согласовать UX-сценарии (регистрация, матч, профиль, запрос контакта).
4. Составить ТЗ по API и БД.
5. Начать с прототипа: регистрация + матчмейкинг + реалтайм-ходы.
