 # System Design социальной сети для путешественников

### Functional requirements:

- публикация постов из путешествий с фотографиями, небольшим описанием и привязкой к конкретному месту путешествия
- оценка и комментарии постов других путешественников
- подписка на других путешественников
- поиск популярных мест для путешествий и просмотр постов с этих мест в виде ТОПа мест по странам и городам
- общаться с другими путешественниками в личных сообщениях
- просматривать ленту других путешественников

### Non-functional requirements:

- 10 000 000 DAU
- Работает на территории СНГ
- Доступность 99,95%
- Максимальный размер фото в социальной сети 1280 х 1024 рх
- Максимальный вес изображения для поста — 1 МБ
- Длина поста - до 5120 символов (5 КБ)
- Длина комментария - до 512 символов (1 КБ)
- В пост можно вложить максимум 10 фотографий
- Суточное ограничение на количество постов от одного пользователя - 10
- Подписаться можно не более чем на 1000 путешественников
- Храним все данные все время
- Оповещение пользователя о событии по подписке асинхронное (до нескольких минут)
- Безопасное хранение пароля от учетной записи с двухфакторной аутентификацией и возможностью сброса пароля

#### Required memory:
##### Оценить сколько потребуется дисков для публикации / поиска / хранения постов путешественников на 1 год.
Допустим, что в среднем человек путешествует 2 раза в год.
За одно путешествие он делает до 5 постов.
Максимально один пост будет занимать 5 КБ + 10 фотографий (10 * 1 МБ).
У одного поста в среднем по 10 комментариев. (10 * 1 КБ)

MemoryOnPost_Text = 15 КБ;
MemoryOnPost_Image = 10 МБ;

MemoryOnAllPost_Text = 15 КБ * 5 постов * 2 раза в год * 10M DAU = 
    1.5 КБ * 10^9 = 1.5 MБ * 10^6 = 1.5 ГБ * 10^3 = 2 ТБ

Количество новых подключений пользователей за секунду ~= 120 (10M DAU / 86400 * 5 запросов/сек).
Допустим, лента загружается батчами по 25 постов.  
Трафик на чтение = MemoryOnPost_Text (15КБ) * 25 постов * 120 пользователей ~= 50МБ/сек.

Для хранения текста требуется 1 HDD (200 МБ/сек) 6 ТБ. Возьмем диск с запасом. 
В будущем можем сменить на SSD для увеличения пропускной способности.

MemoryOnAllPost_Image = 10 МБ * 5 постов * 2 раза в год * 10M DAU 
= 1 МБ * 10^9 = 1 ГБ * 10^6 = 1 ТБ * 10^3 = 1 ПБ.

Для хранения картинок требуется 1000/6 = 170 SSD по 6 ТБ.
