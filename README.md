# 12.4 "Реляционные базы данных: SQL. Часть 2" - НиконоровДА FOPS-6

## Задание 1.

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей и выведите в результат следующую информацию:

* фамилия и имя сотрудника из этого магазина,
* город нахождения магазина,
* количество пользователей, закрепленных в этом магазине.

```SQL
SELECT CONCAT(s.first_name,'',s.last_name) as ФИО, c.city, count(c2.customer_id) as Количество FROM staff s JOIN address a ON s.address_id = a.address_id JOIN city c ON a.city_id = c.city_id JOIN store s2 ON s2.store_id = s.store_id JOIN customer c2 ON s2.store_id = c2.store_id GROUP BY s.first_name, s.last_name, c.city HAVING Количество > 300;
```

![alt text](https://github.com/mxssclxck/hw-12.04/blob/main/img/1.png)

## Задание 2.

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```SQL
SELECT COUNT(f.`length`) AS Число_фильмов FROM film f WHERE f.length > (SELECT AVG(f2.`length`) FROM film f2);
```

![alt text](https://github.com/mxssclxck/hw-12.04/blob/main/img/2.png)

## Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```SQL
SELECT DATE_FORMAT(p.payment_date,'%Y-%M') AS Месяц, (SUM(p.amount)) AS Сумма_в_месяц, COUNT(p.rental_id) AS Количество_аренд FROM payment p GROUP BY Месяц ORDER BY Сумма_в_месяц DESC LIMIT 1;
```

![alt text](https://github.com/mxssclxck/hw-12.04/blob/main/img/3.png)

## Задание 4

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```SQL
SELECT CONCAT (s.first_name, '',s.last_name) AS ФИО, COUNT(p.amount) AS Продажи, CASE WHEN COUNT(p.amount) > 8000 THEN 'Да' WHEN COUNT(p.amount) < 8000 THEN 'Нет'
END AS Премия, SUM(p.amount) AS Выручка FROM payment p JOIN staff s ON s.staff_id = p.staff_id WHERE p.amount > 0 GROUP BY ФИО ORDER BY Продажи DESC;
```

![alt text](https://github.com/mxssclxck/hw-12.04/blob/main/img/4.png)

## Задание 5

Найдите фильмы, которые ни разу не брали в аренду.

```SQL
SELECT f.title AS Название_фильма, f.release_year AS Год_выхода, p.payment_date, r.return_date FROM payment p JOIN rental r ON r.rental_id = p.rental_id JOIN inventory i ON i.inventory_id = r.inventory_id JOIN film f ON f.film_id = i.film_id WHERE p.amount = 0 ORDER BY f.title;
```

![alt text](https://github.com/mxssclxck/hw-12.04/blob/main/img/5.png)
