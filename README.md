# Домашнее задание к занятию "`12.5. «Индексы»`" - `Живарев Игорь`


### Задание 1

`Процентное отношение общего размера всех индексов к общему размеру всех таблиц:`


```
SELECT  SUM(tt.DATA_LENGTH) , SUM(tt.INDEX_LENGTH), CONCAT( ROUND ((SUM(tt.INDEX_LENGTH) / SUM(tt.DATA_LENGTH)) *100), ' %') AS Отношение
FROM INFORMATION_SCHEMA.TABLES tt
WHERE  tt.TABLE_SCHEMA = 'sakila' ;
```

![IDE DBeaver](img/12.05-01.png)

---

### Задание 2

`Наиболее узким местом в предлагаемом запросе является то, что оконная функция обрабатывает излишние таблицы а именно inventory, rental и film. Т.к. нужно посчитать сумму платежей покупателей за конкретную дату, обработка и присоединение этих таблиц не имеет смысла т.к. дальше данные не используются. Все необходимые данные есть в таблицах payment и customer, соответственно, остальные таблицы можно исключить. Предлагается оптимизировать запрос следующим образом:`


```
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id)
from payment p, customer c
where date(p.payment_date) = '2005-07-30' and p.customer_id = c.customer_id;
```

---
