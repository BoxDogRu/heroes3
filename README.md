# Heroes III Telegram Bot
Да, да - о старом, о добром, о вечном. Бот по мотивам игры Heroes of Might and Magic III.
Адрес в Telegram: [@heroes_game_bot](https://t.me/heroes_game_bot).

### Содержание 
1. Энциклопедия по игре Heroes of Might and Magic III: Hota.
   - Герои. Доступна имитация прокачки героев согласно игровым вероятностям.
   - Магия.
   - Города. Только картинки и культовые мелодии.
   - Существа.
   - Артефакты. С учетом игровых вероятностей смоделировано получение артефактов в игровых объектах.
2. GPT-эксперименты. 
   1. Генерация историй про героев игры: см. "Герои/<Класс>/<Герой>/GPT Story"
   2. Моделирование персонажей: см. "Герои/GPT-моделирование персонажей" - эксперимент с семантическим промтингом.
      Мы взяли 12 архетипов - сущностных характеристик любого героя, смешали их с 46-ю расами и 20-ю классами из игры Heroes III Hota.
      Получился прототип сервиса моделирования персонажа.
      Текущая схема:
      - Этап 1. Определение архетипа. В качестве фундамента моделирования характера персонажа взята архетипическая концепция от Маргарет Марк и Кэрол Пирсон. Немного подробнее про [теорию управления знаниями](https://brainmod.ru/magazine/business/archetypal-marketing/). Подход универсальный, не зависит от конкретной компьютерной игры - мы выбираем сущностные характеристики героя. Подход может использоваться в любом моделировании, в первоисточнике - авторы строят бизнес-бренды, в нашем боте мы пробуем использовать данный подход для создания ярких персонажей игры.
        - В боте определение архетипа реализовано в формате теста. Ограничиваем максимумом тремя архетипами в одном профиле персонажа: позиционные "желание" и "текущее состояние", плюс дополнительная модель поведения (минимальное влияние).  
      - Этап 2. Выбор класс, расы, пола персонажа. После определения архетипического ядра персонажа, мы продолжаем моделирование персонажа уже в мире Heroes of Might and Magic - для начала, определяем подходящий класс героя, расу, и пол персонажа
      - Этап 3 (текущий). GPT. Комбинируем и экспериментируем в создании промтов, получению интересных, согласованных результатов. Пока ясно, что можно лучше :) Отсутствует реализация героев перешедших на "тёмную сторону силы" - когда "тень архетипа" побеждает (злые замки и расы), использование игровых объектов в генерации.
      - Этап 4 (как идея). Хронотоп. Детализация героической истории персонажа, разворачивание истории во времени, добавление элементов написания сценариев, смысловой раскадровки. Вариации на тему [Saga](https://writeonsaga.com/demos).
      

## Tech Stack
Расширяемый тематический микросервис с высокой производительностью для Telegram Bot.
Используется база данных Postgres и ORM. Интеграция с Yandex GPT.
#### Исходное решение: [Telegram Bot Microservice](https://github.com/raisultan/tg-bot-service). 
Библиотеки обновлены до актуальных версий.
  - FastAPI 0.111.0
  - Telethon 1.35.0
  - AsyncI
  - Docker & docker-compose
#### Добавлено к исходному решению:
- Postgres & Peewee - база знаний, сохранение результатов и генераций.
- [Yandex GPT](https://yandex.cloud/ru/docs/foundation-models/quickstart/yandexgpt) с авторизацией через [JWT-токены](https://yandex.cloud/ru/docs/iam/operations/iam-token/create-for-sa#via-jwt).

  

## Features

### Receiver Service
- Прием обновлений [Telethon / MTProto] - получение обновлений от любых действий пользователя, направленных на Telegram Bot/Account. Аналогично серверу Webhook или долгому опросу, но быстрее и стабильнее за счет использования протокола MTProto, который является родным для Telegram. [Подробнее об этом](https://docs.telethon.dev/en/latest/concepts/botapi-vs-mtproto.html#advantages-of-mtproto-over-bot-api).
- Универсальный базовый обработчик событий.

### Sender Service
- Async API [FastAPI] - получение запросов от других служб.
- Hero API Yandex GPT. Swagger:  http://localhost:8001/docs
  - Endpoints (дополнительно к запросам из бота):
    - `/api/v1/hero/{hero_id}/{topic}` - сгенерировать историю про героя в Yandex GPT.


## Как использовать

```shell
docker-compose build
docker-compose up
```


## Todo:
- [✔] ~~Размещение проекта на сервере~~
- [  ] Дальнейшие работы над ботом не запланированы. По всем вопросам и предложениям пишите на brainmod@yandex.ru