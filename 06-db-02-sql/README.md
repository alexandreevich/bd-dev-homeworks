# Домашнее задание к занятию 2. «SQL»

## Введение

Перед выполнением задания вы можете ознакомиться с 
[дополнительными материалами](https://github.com/netology-code/virt-homeworks/blob/virt-11/additional/README.md).

## Задача 1

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose-манифест.

## Ответ 1
sudo docker volume create pgdata
sudo docker volume create pgbackup

sudo docker run -d --name dockerpg -e TZ=UTC -p 30432:5432 -e POSTGRES_PASSWORD=mypass -v pgdata:/var/lib/postgresql/data -v pgbackup:/tmp/backups ubuntu/postgres:12-20.04_edge
![Снимок экрана 2024-02-27 в 19 27 23](https://github.com/alexandreevich/bd-dev-homeworks/assets/109306886/f1edd560-f141-4edd-9ce5-da8350ef1e9e)





## Задача 2

В БД из задачи 1: 

- создайте пользователя test-admin-user и БД test_db;
- в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);
- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;
- создайте пользователя test-simple-user;
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.

Таблица orders:

- id (serial primary key);
- наименование (string);
- цена (integer).

Таблица clients:

- id (serial primary key);
- фамилия (string);
- страна проживания (string, index);
- заказ (foreign key orders).

Приведите:

- итоговый список БД после выполнения пунктов выше;
- описание таблиц (describe);
- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;
- список пользователей с правами над таблицами test_db.


## Ответ 2 
![Снимок экрана 2024-02-27 в 19 42 32](https://github.com/alexandreevich/bd-dev-homeworks/assets/109306886/a06af9fd-a139-408b-8a03-11f962e2ac42)
![Снимок экрана 2024-02-27 в 19 42 43](https://github.com/alexandreevich/bd-dev-homeworks/assets/109306886/da88e736-00f6-4006-9bb8-c8ab3fc6ce28)





## Задача 3

Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Используя SQL-синтаксис:
- вычислите количество записей для каждой таблицы.

Приведите в ответе:

    - запросы,
    - результаты их выполнения.




## Ответ 3
![Снимок экрана 2024-02-27 в 19 43 38](https://github.com/alexandreevich/bd-dev-homeworks/assets/109306886/0008d46f-2f11-4ebc-b8fb-c1abe2c104bc)




## Задача 4

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys, свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения этих операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса.
 
Подсказка: используйте директиву `UPDATE`.


## Ответ 4
![Снимок экрана 2024-02-27 в 19 44 47](https://github.com/alexandreevich/bd-dev-homeworks/assets/109306886/70e91e63-2754-4ea8-9436-49c3ec6d8fdd)




## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните, что значат полученные значения.


## Ответ 5

![Снимок экрана 2024-02-27 в 19 47 21](https://github.com/alexandreevich/bd-dev-homeworks/assets/109306886/bfc00b61-0cc1-4b91-8d8d-b93ecc39de77)
Seq Scan - показывает, что при выполнении запроса последовательно считывается каждая запись таблицы. Rows - примерное количество строк, возвращаемых на каждом этапе плана запроса. Width - предполагаемый средний размер строк в байтах. Startup Cost, Total Cost - ориентировочная стоимость выполнения запроса состоящая из двух частей: стартовая стоимость и общая стоимость. Чем ниже стоимость, тем более эффективным будет запрос. Relation Name - имя таблицы над котороый производится анализ. Filter - каждая запись сравнивается с условием "заказ" IS NOT NULL, если условие выполняется, запись вводится в результат, если нет, то > > отбрасывается.



## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).

Остановите контейнер с PostgreSQL, но не удаляйте volumes.

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 


## Ответ 6

![Снимок экрана 2024-02-27 в 20 21 16](https://github.com/alexandreevich/bd-dev-homeworks/assets/109306886/78a9f98c-a42a-4ddf-87a1-f26b8e8a91aa)

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---

