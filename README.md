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
3. Официальной информации о затратах дискового пространства на каждого пользователя нет, поэтому были выделены основные сущности такие как пост, комментарий и голосование. Положим, что в среднем пользователь создаёт 3 поста, 20 комментариев и 150 голосований в месяц. Пост включает в себя текст в среднем на 500-1000 символов, что в худшем случае занимает 1 кБ, иногда включающий вложения, такие как фото или gif-изображения, видео обычно прикрепляются из сторонних источников. Изображения, отображающиеся на клиенте весят порядка 80 КБ, размер средней gif примем за 350 КБ, а видео за 300 МБ, так как длина видео ограничена 15 минутами, но примем, что на видео уходит 50 МБ. Учитывая тренды ведения постов и историю, примем, что фото присутствует у 50% постов, gif - у 10%, видео - у 3%.  Примем, что комментарий занимает 0.2 КБ, а оценка 0.1 КБ (запись в базу).

Итоговый объём в месяц:
|Активность|Вес единицы|Количество|Итоговый объём|
| -------- | --------- | -------- | ------------ |
|Пост|1КБ + 80КБ * 0.5 + 350КБ * 0.1 + 50МБ * 0.03|3|~4МБ|
|Комментарий|0.2КБ|20|4КБ|
|Голосование|0.1КБ|150|15КБ|

Итого каждый пользователь в месяц занимает около 4.2 МБ. Если средний пользователь пользуется Reddit уже 10 лет, то он занимает 4.2*12*10 = 504 МБ.

4. Согласно [2], зарегистрированные пользователи проводят 20 минут на площадке, допустим, за 20 минут он в среднем просматривает 15 постов.

Среднее количество действий пользователя по типам в день (единиц в день):
|Действие|Количество|
| ------ | -------- |
| Создание поста | 0.1 |
| Создание сообщества | ~0 |
| Посещение сообщетсва | 1 |
| Комментирование | 0.6 |
| Оценка | 3 |
| Выдача ленты контента | 3 (Популярное, сообщество, подписки) |
| Поиск постов | 0.5 |
| Регистрация | ~0 |
| Загрузка постов | ~100 |
| Загрузка комментариев | ~40 (5 постов) |

Итого пользователь загружает порядка 400МБ в сутки

Технические метрики:
1. Существенными блоками данных являются аккаунты пользователей и перечисленные ранее посты, комментарии и оценки, есть также посты в сообществе, где в сутки может поститься порядка десяти постов. Положим, что количество постов в день равно одному. При этом количество сообществ - 1 млн [3]. Расчёты велись исходя из того, что в среднем за всё время MAU состовляло 1 млрд. Аккаунт содеаржит аватар (80 КБ), описание, подписки, положим, что вся информация о пользователе требует в стреднем 90 КБ.
| Блок | Штук | Вес | Всего |
| ---- | ---- | --- | ----- |
| Посты | (3 * 1e9 + 30 * 1e6) * (2024-2005) | 4 МБ | ~220.000 ТБ |
| Комментарии | 20 * 1e9 | 0.2КБ | ~4 ТБ |
| Оценки | 150 * 1e9 | 0.1КБ | ~15 ТБ |
| Аккаунты | 1e9 | 90КБ | ~90ТБ |
2. Сетевой трафик
| Тип | Нагрузка для одного пользователя | Общая |
| Загрузка постов | 100 * 4МБ | 23.651 ТБ |
| Комментарии | (0.6 + 40) * 0.2КБ | ~0.5 ТБ |
| Оценка | 3 * 0.1КБ | 0.02 ТБ |

Суммарный суточный - 24ТБ/сутки. Пиковое потребление - 5% от суточной нагрузки (5% суточных пользователей пользуются одновременно) 62e6 * 5% * 400МБ / 24 / 3600 ~ 14 ГБ/c.

3. RPS. Загрузка постов считается не как отдельный запрос, а как сопутствующий загрузке ленты, поиска и сообщества. Всего в сутки запросов = avg * 24 * 3600, пиковый в секунду - всего запросов * 5% / Время пика (20 минут). В секунду = средний * 5% * 24 * 3600 / 1200 = средний * 3,6
| Блок | На одного пользователя в сутки | Итого в секунду средний | Итого в секунду пиковый |
| ---- | ---- | --- | --- |
| Загрузить посты | 3 | ~2150 | 7.740 | 
| Загрузить комментарии | 4 | ~2870 | 10.332 |
| Поиск | 0.5 | ~360 | 1.296 |
| Создание поста | 0.1 | ~70 | 252 |
| Загрузить сообщество | 1 | ~700 | 2.520 |
| Комментировать | 0.6 | ~420 | 1.512 | 
Итого: ~6.600 средний, и ~23.760 пиковый

Список источников:
1. [Reddit User Base & Growth Statistics: How Many People Use Reddit? (2024)](https://www.bankmycell.com/blog/number-of-reddit-users/)
2. [Reddit User Age, Gender, & Demographics (2024)](https://explodingtopics.com/blog/reddit-users)
3. [How many subreddits are on Reddit?](https://wegotthiscovered.com/social-media/how-many-subreddits-are-on-reddit/)
