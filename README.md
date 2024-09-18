# highload

## [Reddit](https://www.reddit.com/)

### 1. Тема и целевая аудитория

**Reddit** является популярным сайтом, на котором пользователи могут создавать посты, участвовать в обсуждениях, подписываться на каналы. Ближайшие аналоги Reddit'а - Lemmy, Quora, 4chan[1].
Ежедневная аудитория Reddit'а состовляет 70 миллионов, при этом большая часть активных пользователей - 47% (26.4 млн) жители США [2].

Распределение пользователей по странам:
| Страна  | Процентное соотношение |
|---------|-----------------------:|
|США  |	47.8%                      |
|Великобритания  |	7.6%           |
|Канада  |	7.6%                   |
|Австралия    |	7.6%               |
|Германия    |	7.6%               |
|Остальные    |	29.9%              |

Распределение пользователей по возрастам:
| Возраст | Процентное соотношение |
|---------|-----------------------:|
|18 – 29  |	36%                    |
|30 – 49  |	22%                    |
|50 – 64  |	10%                    |
|65+      |	3%                     |

MVP функционал:
1. Создание и управление постами.
2. Сообщества
3. Комментарии.
4. Система голосования/оценки.
5. Выдача ленты контента.
6. Поиск постов.
7. Регистрация.

Ключевые продуктовые решения:
1. Система голосования, влияющая на порядок отображения.
2. Генерирование ленты с учетом предпочтений пользователя.
3. Уведомление пользователя о новых постах в сообщетсвах, ответах.

Список источников:
1. [19 best sites like Reddit 2024](https://rigorousthemes.com/blog/best-reddit-alternatives/)
2. [14+ Reddit Statistics For 2024](https://www.demandsage.com/reddit-statistics/).

### 2. Расчет нагрузки

Продуктовые метрики:
1. Месячная аудитория (MAU) - 2.306 миллиарда [1],
2. Дневная аудитория (DAU) - 62.24 миллиона [1],
3. Для пользователей не предусмотрено выделение личного пространства на диске.
4. Согласно различным источникам [2] [3] [4], зарегистрированные пользователи в среднем проводят 20 минут на площадке, при этом просматривая 2.84 страницы [4]. Одна страница содержит 25 новостей (Пример запроса, выдающего [ленту](https://www.reddit.com/svc/shreddit/feeds/popular-feed)). В среднем лента весит 25 МБ, то есть 1 пост (с медиаконтентом) весит 1 МБ. За год публикуется порядка 2-ух миллиардов комментариев [3], то есть в среднем пользователь в месяц публиукет 0.09 комментарий, в день это 0.003. Согласно [5] каждый месяц создаётся около 11 млн постов, это в среднем 0,00016 постов на пользователя в день. Согласно [3] в год регистрируется порядка 50 миллиардов голосований, это 1,8 оценок в день на пользователя. Комментарии также загружаются страницами. В страницу комментариев входят также пользователи и их данные (аватар, имя). Предположим, что пользователь в день также загружает 3 страницы комментариев, средний вес которой 13 КБ (измерено по запросу), вес одного комментария в среднем 0.5 КБ, производит 0.5 запросов поиска и 1 раз посещает сообщество в день, а также подписывается (или отписывается) на сообшество 1 раз в месяц. 

Среднее количество действий пользователя по типам в день (единиц в день):
|Действие|Количество в день|
| ------ | -------- |
| Создание поста | 0.00016 |
| Создание сообщества | ~0 |
| Посещение сообщетсва | 1 |
| Подписка/отписка на сообщество | 0.03 |
| Комментирование | 0.003 |
| Оценка | 1.8 |
| Выдача ленты контента | 2.84 |
| Поиск постов | 0.5 |
| Регистрация | ~0 |
| Загрузка постов | 71 |
| Загрузка комментариев | 3 страницы (~60 комментариев) |

Технические метрики:
1. Существенными блоками данных являются аккаунты пользователей и перечисленные ранее посты, комментарии и оценки. Аккаунт содеаржит аватар (80 КБ), описание, подписки, положим, что вся информация о пользователе требует в стреднем 90 КБ. Положим, что в среднем текст поста состовляет 500 знаков, фото занимает 50 КБ, gif - 200 КБ, видео ~750 КБ

| Блок | Штук | Вес | Всего |
| ---- | ---- | --- | ----- |
| Посты (всего) | 11 000 000 [^5] * 12 * (2024-2005) = 2 508 000 000| 1 МБ | ~2 394 ТБ |
| Текст поста | ~ количесвто постов | 0,5 КБ | ~1.2 ТБ |
| Фото поста | ~0.6 постов | ~85 КБ | ~120 ТБ |
| GIF поста | ~0.4 постов | 512 КБ | ~478.8 ТБ |
| Видео поста | ~0.1 постов | 7.5 МБ | ~1 800 ТБ |
| Комментарии | 2e9 * (2024-2005) | 0.25КБ | ~9 000 ТБ |
| Оценки | 50e9 * (2024-2005) | 0.1КБ | ~90 000 ТБ |
| Аккаунты | 0.5e9[2] | 90КБ | ~45ТБ |

2. Сетевой трафик

| Тип | Нагрузка для одного пользователя | Общая (сутки) | Сердняя (в секунду) | Пиковая (x5) |
| ---- | ---- | --- | --- |
| Загрузка ленты постов | 2.84 * 25 * 1МБ | 4 200 ТБ | 0.05 ТБ/секунду | 0.25 ТБ/секунду |
| Создать пост          | 0.00016 * 1МБ   | 0.01 ТБ  | 1.2e-7 ТБ/секунду | 6e-7 ТБ/секунду |
| Подписка на сообщество| 0.003 * 0.1КБ   | 0.00002 ТБ| 2.3e-10 ТБ/секунду | 1.1e-9 ТБ/секунду |
| Загрузить комментарии | 3 * 60 * 0.25КБ | ~2.6 ТБ  | 3e-5 ТБ/секудну | 15e-5 ТБ/секунду |
| Оценка                | 1.8 * 0.1КБ     | 0.01 ТБ  | 1.2e-7 ТБ/секудну | 6e-7 ТБ/секунду |

Суммарная нагрузка в сутки - 4 203 ТБ, в секунду - 0.05 ТБ, пиковая в секунду - 0.25 ТБ

3. RPS.

| Блок | На одного пользователя в сутки | Итого в секунду средний | Итого в секунду пиковый (x5) |
| ---- | ---- | --- | --- |
| Загрузка ленты постов | 2.84 | ~2 000 | 10 000 | 
| Загрузить комментарии | 3 | ~2 200 | 11 000 |
| Поиск | 0.5 | ~360 | 1 800 |
| Создание поста | 0.00016 | ~0.12 | 0.6 |
| Загрузить сообщество | 1 | ~720 | 3 600 |
| Комментировать | 0.003 | ~2.2 | 11 | 
|Оценить | 1.8 | ~1 300 | 6 500 |
|Итого| 9,2 | ~6 600| ~33 000 |

Список источников:
1. [Reddit User Base & Growth Statistics: How Many People Use Reddit? (2024)](https://www.bankmycell.com/blog/number-of-reddit-users/)
2. [Reddit User Age, Gender, & Demographics (2024)](https://explodingtopics.com/blog/reddit-users)
3. [Significant Reddit Statistics 2024: Facts & Figures, Usage Statistics](https://bytegain.com/reddit-statistics/)
4. [Latest Reddit Statistics: The SECRET Social Media Platform](https://www.ileeline.com/reddit-statistics/)
5. [20+ Riveting Reddit Statistics [2023]](https://www.zippia.com/advice/reddit-statistics/)
6. [How many subreddits are on Reddit?](https://wegotthiscovered.com/social-media/how-many-subreddits-are-on-reddit/)
