# Домашнее задание к занятию "`SQL. Часть 2`" - `Фёдоров Илья`
### Задание 1
SELECT 
    CONCAT(ROUND(
        (SUM(index_length) / SUM(data_length)) * 100, 2), '%'
    ) AS index_to_data_ratio
FROM 
    information_schema.tables
WHERE 
    table_schema = 'sakila';

![alt text](https://github.com/Limzor/IndexHW/blob/main/Screenshot_1.png)
### Задание 2

![alt text](https://github.com/Limzor/IndexHW/blob/main/Screenshot_2.png)

SELECT DISTINCT 
    CONCAT(c.last_name, ' ', c.first_name) AS customer_name, 
    SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title) AS total_amount
FROM payment p
JOIN rental r ON p.payment_date = r.rental_date
JOIN customer c ON r.customer_id = c.customer_id
JOIN inventory i ON r.inventory_id = i.inventory_id
JOIN film f ON i.film_id = f.film_id
WHERE p.payment_date >= '2005-07-30 00:00:00' 
  AND p.payment_date < '2005-07-31 00:00:00';

CREATE INDEX idx_payment_date ON payment(payment_date);
CREATE INDEX idx_rental_date_customer_inventory ON rental(rental_date, customer_id, inventory_id);
CREATE INDEX idx_customer_id ON customer(customer_id);
CREATE INDEX idx_inventory_id ON inventory(inventory_id);
CREATE INDEX idx_film_id ON film(film_id);
