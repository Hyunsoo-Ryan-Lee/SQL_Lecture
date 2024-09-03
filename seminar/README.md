## 1. SELECT 기능 훑어보기

#### 모든 내용 보기
```sql
SELECT * FROM orders;
```

#### 특정 컬럼만 보기
```sql
SELECT order_id, pizza_id, quantity FROM orders;
```

#### 일부만 가져오기 (LIMIT)
```sql
SELECT * FROM pizzas LIMIT 5;
```

#### 원하는 이름(alias)으로 가져오기
```sql
SELECT pizza_name AS name, size AS pizza_size, price AS cost FROM pizzas;
```

#### 고정값 넣기
```sql
SELECT pizza_name, size, price, 'Pizza Order' AS category FROM pizzas;
```

## 2. 필터링 기능 훑어보기

#### 연산자
```sql
SELECT * FROM pizzas WHERE price > 30000;
```

#### WHERE
```sql
SELECT * FROM orders WHERE pizza_id = 'big_meat_l';
```

#### IN
```sql
SELECT * FROM orders WHERE pizza_id IN ('big_meat_l', 'pepperoni_l');
```

#### BETWEEN
```sql
SELECT * FROM pizzas WHERE price BETWEEN 20000 AND 30000;
```

#### LIKE
```sql
SELECT * FROM pizza_types WHERE ingredients LIKE '%%Tomatoes%%';
```

#### NULL
```sql
SELECT * FROM orders WHERE pizza_id IS NULL;
```

## 3. ORDER BY
```sql
SELECT * FROM pizzas ORDER BY price ASC;
```
```sql
SELECT * FROM pizzas ORDER BY price DESC;
```

## 4. 수치형 계산 - COUNT, SUM, AVG, MIN, MAX, ROUND

#### COUNT
```sql
SELECT COUNT(*) AS total_orders FROM orders;
```

#### SUM
```sql
SELECT SUM(price) AS total_revenue FROM pizzas WHERE size = 'L';
```

#### AVG
```sql
SELECT AVG(price) AS average_price FROM pizzas;
```

#### MIN
```sql
SELECT MIN(price) AS cheapest_pizza FROM pizzas;
```

#### MAX
```sql
SELECT MAX(price) AS most_expensive_pizza FROM pizzas;
```
#### ROUND
```sql
SELECT ROUND(AVG(price), 3) AS average_price FROM pizzas;
```

## 5. GROUP BY - HAVING

#### GROUP BY
```sql
SELECT pizza_name, COUNT(*) AS order_count FROM pizza_types GROUP BY pizza_name;
```

#### HAVING
```sql
SELECT pizza_name, COUNT(*) AS order_count 
FROM pizza_types 
GROUP BY pizza_name 
HAVING COUNT(*) > 1;
```

## 6. 서브 쿼리
```sql
SELECT * FROM pizzas WHERE price > (SELECT AVG(price) FROM pizzas);
```

## 7. JOIN

#### INNER JOIN
```sql
SELECT o.order_id, p.pizza_name, p.price 
FROM orders o 
INNER JOIN pizzas p 
ON o.pizza_id = p.pizza_id;
```

#### LEFT JOIN
```sql
SELECT o.order_id, p.pizza_name, p.price 
FROM orders o 
LEFT JOIN pizzas p 
ON o.pizza_id = p.pizza_id;
```
---
## 응용 문제

### 1. unique 영업일 수
```sql
SELECT COUNT(DISTINCT DATE(order_timestamp)) AS unique_business_days
FROM orders;
```

### 2. 총 매출
```sql
SELECT SUM(p.price * o.quantity) AS total_revenue
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id;
```

### 3. 하루 평균 매출
```sql
SELECT SUM(p.price * o.quantity) / COUNT(DISTINCT DATE(o.order_timestamp)) AS avg_daily_revenue
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id;
```

### 4. 1년동안 주문 횟수
```sql
SELECT COUNT(DISTINCT order_id) AS total_orders
FROM orders
WHERE YEAR(order_timestamp) = 2023;
```

### 5. 평균 주문 금액
```sql
SELECT AVG(sub.total_order_value) AS avg_order_value
FROM (
    SELECT SUM(p.price * o.quantity) AS total_order_value
    FROM orders o
    JOIN pizzas p ON o.pizza_id = p.pizza_id
    GROUP BY o.order_id
) sub;
```

### 6. 총 판매량
```sql
SELECT SUM(quantity) AS total_quantity_sold
FROM orders;
```

### 7. 한 주문당 시킨 피자 판 수 내림차순 정렬
```sql
SELECT order_id, SUM(quantity) AS total_pizzas_ordered
FROM orders
GROUP BY order_id
ORDER BY total_pizzas_ordered DESC;
```

### 8. 월별 매출
```sql
SELECT DATE_FORMAT(order_timestamp, '%Y-%m') AS month, SUM(p.price * o.quantity) AS monthly_revenue
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id
GROUP BY month
ORDER BY month;
```

### 9. 요일별 매출
```sql
SELECT DATE_FORMAT(order_timestamp, '%W') AS weekday, SUM(p.price * o.quantity) AS weekday_revenue
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id
GROUP BY weekday
ORDER BY FIELD(weekday, 'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday');
```

### 10. 시간대별 매출
```sql
SELECT DATE_FORMAT(order_timestamp, '%H:00') AS hour, SUM(p.price * o.quantity) AS hourly_revenue
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id
GROUP BY hour
ORDER BY hour;
```

### 11. 피자 메뉴별 판매량
```sql
SELECT pt.name AS pizza_name, SUM(o.quantity) AS total_quantity_sold
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY total_quantity_sold DESC;
```

### 12. 각 피자 메뉴의 월별 판매량
```sql
SELECT pt.name AS pizza_name, DATE_FORMAT(o.order_timestamp, '%Y-%m') AS month, SUM(o.quantity) AS monthly_quantity_sold
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name, month
ORDER BY pt.name, month;
```

### 13. 각 피자 메뉴의 사이즈별 판매량
```sql
SELECT pt.name AS pizza_name, p.size, SUM(o.quantity) AS total_quantity_sold
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name, p.size
ORDER BY pt.name, p.size;
```

### 14. 피자 카테고리별 판매량
```sql
SELECT pt.category, SUM(o.quantity) AS total_quantity_sold
FROM orders o
JOIN pizzas p ON o.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
ORDER BY total_quantity_sold DESC;
```