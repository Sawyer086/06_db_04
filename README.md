# Домашнее задание к занятию 4. «PostgreSQL» - Punin Sergey
Задача 1
Используя Docker, поднимите инстанс PostgreSQL (версию 13).

Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL, используя `psql`.

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

**Найдите и приведите** управляющие команды для:
- вывода списка БД,
- подключения к БД,
- вывода списка таблиц,
- вывода описания содержимого таблиц,
- выхода из psql.
  
## Задача 2
Используя `psql`, создайте БД `test_database`.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/virt-11/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.

Перейдите в управляющую консоль `psql` внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` с наибольшим средним значением размера элементов в байтах.
**Приведите в ответе** команду, которую вы использовали для вычисления, и полученный результат.

## Задача 3
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров ипоиск по ней занимает долгое время. Вам как успешному выпускнику курсов DevOps в Нетологии предложилипровести разбиение таблицы на 2: шардировать на orders_1 - price>499 и orders_2 - price<=499.

Предложите SQL-транзакцию для проведения этой операции.

Можно ли было изначально исключить ручное разбиение при проектировании таблицы orders?

## Задача 4
Используя утилиту `pg_dump`, создайте бекап БД `test_database`.Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

---

# Ответ: 
## Задача 1:

Команды:
```
sudo docker run --name postgres-13 -e POSTGRES_HOST_AUTH_METHOD=trust -v /var/docker/postgresql-13/db:/var/lib/postgresql/data -v /var/docker/postgresql-13/backup:/tmp/backup -d postgres:13
sudo docker exec -it postgres-13 psql -U postgres
```
![1](https://github.com/Sawyer086/06_db_04/blob/main/Screenshots/1.jpg)

вывод списка БД:
```
\l
```
подключения к БД:
```
\c
```
вывод списка таблиц:
```
\dt[S+]
```
вывод описания содержимого таблиц:
```
\d[S+]
```
выход из psql:
```
\q
```
## Задача 2:
Команды:
```
CREATE DATABASE test_database;
\c test_database
\i /tmp/backup/test_dump.sql
test_database=# ANALYZE VERBOSE public.orders;
test_database=# SELECT tablename, attname, avg_width FROM pg_stats WHERE tablename='orders' ORDER BY avg_width DESC LIMIT 1;
```
![2](https://github.com/Sawyer086/06_db_04/blob/main/Screenshots/2.jpg)



