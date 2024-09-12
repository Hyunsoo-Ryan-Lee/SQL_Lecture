## 1. 데이터 추출/필터링

#### 1-1. 모든 내용 보기
```sql
SELECT * FROM codeit-hyunsoo.food.orders
```

#### 1-2. 특정 컬럼만 보기
```sql
SELECT order_id, pizza_id, quantity FROM codeit-hyunsoo.food.orders;
```

#### 1-3. 일부만 가져오기(LIMIT)
```sql
SELECT * FROM codeit-hyunsoo.food.pizzas LIMIT 10
```

#### 1-4. COUNT, DISTINCT
```sql
SELECT 
    COUNT(*)
FROM
    orders;

SELECT 
    DISTINCT(size)
FROM
    codeit-hyunsoo.food.pizzas

-- 카테고리 개수
SELECT 
    COUNT(DISTINCT(category))
FROM
    codeit-hyunsoo.food.pizza_types

-- 주문 횟수
SELECT
    COUNT(DISTINCT order_id)
FROM
    codeit-hyunsoo.food.orders
```

#### 1-5. 원하는 이름(alias)으로 가져오기
```sql
SELECT 
    pizza_name AS name, 
    size AS pizza_size, 
    price AS cost 
FROM
    codeit-hyunsoo.food.pizzas
```

#### 1-6. WHERE-비교 연산자
```sql
SELECT 
    * 
FROM
    codeit-hyunsoo.food.orders
WHERE 
    pizza_id = 'big_meat_l'

SELECT 
    * 
FROM
    codeit-hyunsoo.food.orders
WHERE 
    quantity != 2

SELECT 
    * 
FROM
    codeit-hyunsoo.food.pizzas
WHERE 
    price < 30000
```

#### 1-7. IN
```sql
SELECT
    *
FROM
    codeit-hyunsoo.food.orders
WHERE 
    pizza_id IN ('big_meat_l', 'pepperoni_l')

SELECT
    *
FROM
    codeit-hyunsoo.food.pizza
WHERE 
    size IN ('L', 'M')
```

#### 1-8. LIKE
```sql
SELECT
    *
FROM
    codeit-hyunsoo.food.pizza_types
WHERE
    ingredients LIKE '%%Tomatoes%%'
```

#### 1-9. BETWEEN
```sql
SELECT 
    * 
FROM
    codeit-hyunsoo.food.pizzas
WHERE 
    price BETWEEN 20000 AND 30000
```

#### 1-10. ORDER BY
```sql
SELECT
    *
FROM
    codeit-hyunsoo.food.pizzas
ORDER BY 
    price ASC/DESC;

SELECT
    * 
FROM
    codeit-hyunsoo.food.pizzas
WHERE 
    price BETWEEN 20000 AND 30000
ORDER BY 
    price DESC
```

#### 1-11. 계산 함수
```sql
SELECT 
    SUM(price) AS sum,
    AVG(price) AS average,
    MIN(price) AS min,
    MAX(price) AS max,
    ROUND(AVG(price), 3) AS round
FROM
    codeit-hyunsoo.food.pizzas

-- 피자의 총 판매량
SELECT 
    SUM(quantity) AS total_quantity_sold
FROM
    orders
```

#### 1-12. 서브 쿼리
```sql
SELECT 
    COUNT(pizza_type_id) as l_count
FROM
    (
    SELECT * FROM codeit-hyunsoo.food.pizzas
    WHERE size = 'L'
    )

SELECT 
  *
FROM
    codeit-hyunsoo.food.pizzas
WHERE price > 
    (
    SELECT AVG(price) FROM codeit-hyunsoo.food.pizzas
    )
```

## 2. 데이터 집계

#### 2-1. GROUP BY
```sql
SELECT 
    category,
    COUNT(*) AS order_count
FROM
    codeit-hyunsoo.food.pizza_types
GROUP BY
    category;
```

#### 2-2. 집계 함수
```sql
SELECT 
    order_id,
    COUNT(order_id) AS cnt,
    MIN(quantity) AS min_q,
    MAX(quantity) AS max_q,
    AVG(quantity) AS average,
    ROUND(AVG(quantity)) AS round_avg
FROM
    codeit-hyunsoo.food.orders
GROUP BY 
    order_id

-- 피자 사이즈별 평균 금액
SELECT
    size,
    ROUND(AVG(price), 0) AS average_price
FROM
    codeit-hyunsoo.food.pizzas
GROUP BY
    size
```

#### 2-3. 정렬
```sql
SELECT 
    order_id,
    COUNT(order_id) AS cnt,
    MIN(quantity) AS min_q,
    MAX(quantity) AS max_q,
    AVG(quantity) AS average,
    ROUND(AVG(quantity)) AS round_avg
FROM
    codeit-hyunsoo.food.orders
GROUP BY 
    order_id
ORDER BY 
    max_q DESC

-- 한 주문당 시킨 피자 판 수 내림차순 정렬
SELECT
    order_id,
    SUM(quantity) AS total_pizzas_ordered
FROM 
    codeit-hyunsoo.food.orders
GROUP BY
    order_id
ORDER BY
    total_pizzas_ordered DESC
```

#### 2-3. 조건 - HAVING
```sql
SELECT 
    category,
    COUNT(*) AS category_count
FROM
    codeit-hyunsoo.food.pizza_types
GROUP BY
    category
HAVING
    category_count >= 7
```

## 3. 데이터 변환

#### 3-1. 데이터 타입 변환 - CAST
```sql
SELECT
    SUM(quantity)
FROM 
  (
  SELECT
      quantity,
      CAST(quantity AS STRING) AS str_quantity
  FROM
      codeit-hyunsoo.food.orders
  )
```

#### 3-2. 문자열 변환
```sql
SELECT
    CONCAT(pizza_type_id, size) AS concat_col,
    LEFT(pizza_id, 3) AS left_col,
    RIGHT(pizza_id, 3) AS right_col,
    UPPER(pizza_id) AS upper_col,
    LOWER(size) AS lower_col,
    REPLACE(size, 'L', 'XL') AS replace_col,
    SUBSTRING(pizza_type_id, 1, 3) AS substring_col,
    SPLIT(pizza_id, '_') AS split_col
FROM 
    codeit-hyunsoo.food.pizzas
```

#### 3-3. 날짜 데이터 변환
```sql
SELECT
    EXTRACT(YEAR FROM order_timestamp) AS year,
    EXTRACT(MONTH FROM order_timestamp) AS month,
    EXTRACT(DAY FROM order_timestamp) AS day,
    EXTRACT(HOUR FROM order_timestamp) AS hour,
    EXTRACT(MINUTE FROM order_timestamp) AS minute,
    EXTRACT(SECOND FROM order_timestamp) AS second
FROM
    codeit-hyunsoo.food.orders

SELECT
    DATE(order_timestamp) AS date_time
FROM
    codeit-hyunsoo.food.orders


```

## 4. JOIN

#### 4-1. INNER JOIN
```sql
SELECT 
    o.*, p.*
FROM
    codeit-hyunsoo.food.orders o 
INNER JOIN
    codeit-hyunsoo.food.pizzas p 
ON 
    o.pizza_id = p.pizza_id
```

#### 4-2. LEFT/RIGHT/FULL JOIN
```sql
SELECT 
    p.*, t.*
FROM
    codeit-hyunsoo.food.pizzas p 
LEFT/RIGHT/FULL JOIN
    codeit-hyunsoo.food.pizza_types t 
ON 
    p.pizza_type_id = t.pizza_type_id
```

#### 4-3. CROSS JOIN
```sql
SELECT 
    *
FROM
    codeit-hyunsoo.food.pizzas p 
CROSS JOIN
    codeit-hyunsoo.food.pizza_types t 
```

#### 4-4. 2개 이상 테이블 JOIN
```sql
SELECT 
    o.*, p.*, t.*
FROM 
    codeit-hyunsoo.food.orders o
    JOIN 
        codeit-hyunsoo.food.pizzas p
    ON
        o.pizza_id = p.pizza_id
        JOIN
            codeit-hyunsoo.food.pizza_types t
        ON
            p.pizza_type_id = t.pizza_type_id
```
---
## 5. 응용 문제

#### 1. 1년 간 영업일 수
```sql
SELECT 
    COUNT(DISTINCT(date(order_timestamp))) AS workday
FROM
    codeit-hyunsoo.food.orders
```

#### 2. 1년 총 매출
```sql
SELECT 
    SUM(p.price * o.quantity) AS total_revenue
FROM 
    codeit-hyunsoo.food.orders o
JOIN 
    codeit-hyunsoo.food.pizzas p 
ON 
    o.pizza_id = p.pizza_id
```

#### 3. 하루 평균 매출
```sql
SELECT
    ROUND(SUM(p.price * o.quantity) / COUNT(DISTINCT DATE(o.order_timestamp)), -2) AS avg_daily_revenue
FROM
    codeit-hyunsoo.food.orders o
JOIN
    codeit-hyunsoo.food.pizzas p
ON
    o.pizza_id = p.pizza_id;
```

#### 4. 주문별 가격
```sql
SELECT 
    o.order_id, SUM(p.price * o.quantity) AS total_revenue
FROM 
    codeit-hyunsoo.food.orders o
JOIN 
    codeit-hyunsoo.food.pizzas p 
ON 
    o.pizza_id = p.pizza_id
GROUP BY 
    o.order_id
```

#### 5. 1회 평균 주문 금액
```sql
SELECT 
    ROUND(AVG(total_revenue), -2) AS avg_revenue 
FROM (
    SELECT 
        o.order_id, SUM(p.price * o.quantity) AS total_revenue
    FROM 
        codeit-hyunsoo.food.orders o
    JOIN 
        codeit-hyunsoo.food.pizzas p 
    ON 
        o.pizza_id = p.pizza_id
    GROUP BY
        o.order_id
    )
```

#### 6. 월별 매출
```sql
SELECT 
    EXTRACT(MONTH FROM o.order_timestamp) AS order_month, 
    SUM(p.price * o.quantity) AS monthly_revenue
FROM 
    codeit-hyunsoo.food.orders o
JOIN
    codeit-hyunsoo.food.pizzas p
ON
    o.pizza_id = p.pizza_id
GROUP BY
    order_month
ORDER BY
    order_month
```

#### 7. 시간대별 매출
```sql
SELECT 
    EXTRACT(HOUR FROM o.order_timestamp) AS order_hour, 
    SUM(p.price * o.quantity) AS monthly_revenue
FROM 
    codeit-hyunsoo.food.orders o
JOIN
    codeit-hyunsoo.food.pizzas p
ON
    o.pizza_id = p.pizza_id
GROUP BY
    order_hour
ORDER BY
    order_hour
```

#### 8. 피자 사이즈별 판매 수량
```sql
SELECT 
    size,
    SUM(quantity) AS sold_quantity 
FROM (
    SELECT 
        o.quantity, p.size
    FROM 
        codeit-hyunsoo.food.orders o
        JOIN 
            codeit-hyunsoo.food.pizzas p
        ON
            o.pizza_id = p.pizza_id
)
GROUP BY
    size
```

#### 9. 피자 카테고리별 판매 수량
```sql
SELECT 
    category,
    SUM(quantity) AS sold_quantity 
FROM (
    SELECT 
        o.quantity, t.category
    FROM 
        codeit-hyunsoo.food.orders o
        JOIN 
            codeit-hyunsoo.food.pizzas p
        ON
            o.pizza_id = p.pizza_id
            JOIN
                codeit-hyunsoo.food.pizza_types t
            ON
                p.pizza_type_id = t.pizza_type_id
)
GROUP BY
    category
```

#### 10. 피자의 카테고리 내 각 사이즈별 판매 수량
```sql
SELECT 
    category,
    size,
    SUM(quantity) AS sold_quantity 
FROM (
    SELECT 
        o.quantity, p.size, t.category
    FROM 
        codeit-hyunsoo.food.orders o
        JOIN 
            codeit-hyunsoo.food.pizzas p
        ON
            o.pizza_id = p.pizza_id
            JOIN
                codeit-hyunsoo.food.pizza_types t
            ON
                p.pizza_type_id = t.pizza_type_id
)
GROUP BY
    category, size
ORDER BY 
    category, size
```