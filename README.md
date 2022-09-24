# Twitter

## 1. Тема и целевая аудитория
### 1.1. Тема
[Twitter](https://twitter.com/) — американский сервис микроблогов и социальная сеть, в которой пользователи публикуют сообщения, известные как «твиты», и взаимодействуют с ними.

### 1.2. Целевая аудитория


| Целевая аудитория| Мир|
|-|-|
| Количество посетителей сайта в месяц [[1]](https://www.similarweb.com/ru/website/twitter.com/#traffic)| 7,1 млрд   |
| Количество посетителей сайта в день [[2]](https://datareportal.com/essential-twitter-stats#:~:text=2021%20report%2C%20published)| 238 млн |
| Среднее количество страниц за визит [[1]](https://www.similarweb.com/ru/website/twitter.com/#traffic)| 10,36 |
| Среднее время просмотра [[1]](https://www.similarweb.com/ru/website/twitter.com/#traffic)| 10м 57с  |

| Распределение пользователей по миру (топ-5)|млн|
|-----------------------------------|-----------|
| Соединенные Штаты Америки | 76,9 |
| Япония | 58,95 |
| Индия | 23,6 |
| Бразилия | 19,05 |
| Великобритания | 18,4 |

### 1.3. MVP
Минимальные требования для MVP:
* Регистрация и авторизация пользователя
* Публикация твита
* Проставление лайков
* Отправка сообщения
* Просмотр ленты (твитов)

## 2. Расчет нагрузки
### 2.1. Продуктовые метрики

#### Сводная таблица количества действий пользователя по типам в день
| Тип запроса | Среднее количество |
|-|-|
| Регистрация и авторизация | 178 тыс |
| Публикация твита [[3]](https://www.internetlivestats.com/) | 840 млн |
| Проставление лайков | 952 млн |
| Отправка сообщения | х |
| Просмотр ленты | 7,14 млрд (30 постов * 238 млн) |

### 2.2. Технические метрики
#### 2.2.1 Размер хранения
Основываясь на данных из статьи [[4]], опубликованной в 2020 году, можно сказать, что за 2020 год было накоплено порядка 4,3 Пб данных. Добавляя данные из статьи [[5]](https://www.renolon.com/number-of-tweets-per-day/), можно посчитать, сколько весит в среднем один твит вместе с вложениями.
* Средний размер твита: За 2020 год был создан 821 миллион твитов - что составляет 4,3 Петабайта. Тогда, один твит занимает: `4,3 * 10^6 / 821 = 5,23 Мб`
* Посчитаем количество твитов, суммарно накопленных за все время - 6,478 млрд
* Объем хранилища всех твитов за все время должен составлять: 6,478 * 10^9 * 5,23 = 34,46 Пб
* Остальные данные будут занимать незначительную память от общей памяти сервиса

#### 2.2.2 Сетевой трафик
Твиттер генерирует порядка 12 ТерраБайт новых данных в день [[6]](https://www.loginradius.com/blog/engineering/handling-cheapest-fuel-data/). Тогда, нагрузка в секунду составит: `12 000 / 24 * 60 * 60 = 0.138 Gb/sec` 

#### 2.2.3 RPS
##### 2.2.3.1 Регистрация и авторизация
Основываясь на приведенном графике роста количества новых пользователей на конец года [[7]](https://earthweb.com/how-many-people-use-twitter/), был проведен следующий расчет количества новых регистраций в день:
* Среднее количество новых пользователей за год: `(2 + 17 + 57 + 31 + 219) / 5 = 65,2 млн в год`
* Среднее количество новых пользователей в секунду: `65,2 / (60 * 60 * 24 * 365) = 2,06 rps`

Расчет авторизации:
* Среднее количество пользователей в день: 238 млн [[2]](https://datareportal.com/essential-twitter-stats#:~:text=2021%20report%2C%20published)
* Среднее количество авторизаций в секунду: `238 / (60 * 60 * 24) = 2754 rps`

Суммарное rps: `2754 + 2,06 = 2756,06`

##### 2.2.3.2 Публикация твита
* Общее количество твитов в день - 840 млн [[3]](https://www.internetlivestats.com/)
* Расчет RPS = `840 * 10^6 / 24 * 60 * 60 = 5787 req`

##### 2.2.3.3 Проставление лайков

Для расчета нагрузки по этому запрос был составлен [опрос [8]](https://docs.google.com/forms/d/e/1FAIpQLSe027RLUx7T4P-YPuxbT3Qa_szRFhHPIcNpuxhL_QzR0JeVuw/viewform?usp=sf_link), исходя из которого, можно определить среднее количество лайков пользователя в день:

Результаты опроса:

![image](https://user-images.githubusercontent.com/92160117/192096434-56219850-5878-44c6-b574-6a3ad5374a5a.png)

* Среднее количество лайков в день: `(2,5 * 0,813 + 8 * 0,125 + 15 * 0,063) = 4 лайка/день`
* Расчет RPS = `4 * 238 * 10^6 / (24 * 60 * 60) = 11018 rps`
##### 2.2.3.3 Отправка сообщения
Основываясь на выше опросе [8] можно определить, что люди очень редко пользуются сообщения в Твиттере, поэтому этот пункт в дальнейшем не будет учитываться.
##### 2.2.3.3 Просмотр ленты
В среднем, за один скролл прогружается порядка 10 страниц. Среднее время просмотра составляет 10м 57с  [[1]](https://www.similarweb.com/ru/website/twitter.com/#traffic). Исходя из личного опыта, время просмотра одного твита составляет 30 секунд. Следовательно, за время среднего просмотра пользователь успевает посмотреть 22 поста, значит, необходимо сделать 3 загрузки постов за время просмотра.
* Среднее количество загрузок постов в секунду суммарно: `30 * 238 * 10^6 / (24 * 60 * 60) = 82638 rps`

#### RPS по типам запросов
| Тип запроса | RPS |
|-|-|
| Регистрация и авторизация | 2756,06 |
| Публикация твита | 5787 |
| Проставление лайков | 11018 |
| Отправка сообщения | х |
| Просмотр ленты  | 82638 |
