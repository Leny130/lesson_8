# lesson_8

1 Созда pg
```
sudo apt update && sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y && sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && sudo apt-get update && sudo DEBIAN_FRONTEND=noninteractive apt -y install postgresql-16 unzip atop iotop
```
2 Создал таблицу продаж
```
create table sales(
 id serial,
 fk_warehouse int,
 kolvo int,
 salesdate date default now());

insert into sales(fk_warehouse, kolvo,salesdate) values (1, 10,'20230915'), (2, 5,'2023-09-14');
insert into sales(fk_warehouse, kolvo,salesdate) values (1, 10,'20230115'), (2, 5,'2023-05-14');
insert into sales(fk_warehouse, kolvo,salesdate) values (3, 20,'20231211'), (4, 50,'2023-09-12');
```
Создал функцию
```
CREATE OR REPLACE FUNCTION test_if(data date) RETURNS int AS $$
BEGIN
    RETURN CASE 
        WHEN EXTRACT(MONTH FROM data) < 5 THEN 1
        WHEN EXTRACT(MONTH FROM data) < 9 THEN 2
        ELSE 3
    END;
END;
$$ LANGUAGE plpgsql;
```

Проверяю

```
SELECT 
    id,
    fk_warehouse,
    kolvo,
    salesdate,
    CASE 
        WHEN EXTRACT(MONTH FROM salesdate) < 5 THEN 'Q1'
        WHEN EXTRACT(MONTH FROM salesdate) < 9 THEN 'Q2'
        ELSE 'Q3'
    END AS quarter
FROM sales;
```
```
id	fk_warehouse	kolvo	salesdate	quarter
1	1	10	2023-09-15	Q3
2	2	5	2023-09-14	Q3
3	1	10	2023-01-15	Q1
4	2	5	2023-05-14	Q2
5	3	20	2023-12-11	Q3
6	4	50	2023-09-12	Q3
```
