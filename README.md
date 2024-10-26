# lesson_8

1 Созда pg
```
sudo apt update && sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y && sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && sudo apt-get update && sudo DEBIAN_FRONTEND=noninteractive apt -y install postgresql-16 unzip atop iotop
```
2 Создал таблицу продаж
```
CREATE TABLE sales (
    id SERIAL PRIMARY KEY,
    month INT NOT NULL,
    sales_amount DECIMAL(10, 2) NOT NULL
);
CREATE TABLE
```
```
INSERT INTO sales (month, sales_amount)
VALUES
    (1, 1000.00),
    (2, 1200.00),
    (3, 1500.00),
    (4, 1300.00),
    (5, 1600.00),
    (6, 1400.00),
    (7, 1700.00),
    (8, 1800.00),
    (9, 2000.00),
    (10, 1900.00),
    (11, 2100.00),
    (12, 2200.00);
INSERT 0 12
```
2 Создал функию
```
CREATE OR REPLACE FUNCTION get_quarter(month INTEGER)
RETURNS INTEGER AS $$
BEGIN
    IF month BETWEEN 1 AND 4 THEN
        RETURN 1;  -- Первая треть (январь - апрель)
    ELSIF month BETWEEN 5 AND 8 THEN
        RETURN 2;  -- Вторая треть (май - август)
    ELSIF month BETWEEN 9 AND 12 THEN
        RETURN 3;  -- Третья треть (сентябрь - декабрь)
    ELSE
        RAISE EXCEPTION 'Month must be between 1 and 12';
    END IF;
END;
```
С использованием CASE
```
CREATE OR REPLACE FUNCTION get_quarter(month INTEGER)
RETURNS INTEGER AS $$
BEGIN
    IF month < 1 OR month > 12 THEN
        RAISE EXCEPTION 'Month must be between 1 and 12';
    END IF;

    RETURN CASE
        WHEN month BETWEEN 1 AND 4 THEN 1  -- Первая треть
        WHEN month BETWEEN 5 AND 8 THEN 2  -- Вторая треть 
        WHEN month BETWEEN 9 AND 12 THEN 3  -- Третья треть 
    END;
END;
$$ LANGUAGE plpgsql;
CREATE FUNCTION
```

