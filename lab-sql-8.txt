-- Lab | SQL Queries 8

USE sakila;

-- 1. Rank films by length (filter out the rows with nulls or zeros in length column). Select only columns title, length and rank in your output.
SELECT 
    title, 
    length,
    RANK() OVER(ORDER BY length) AS 'rank' 
FROM
    film
HAVING length >= 1;

-- 2. Rank films by length within the rating category (filter out the rows with nulls or zeros in length column). In your output, only select the columns title, length, rating and rank.
SELECT 
    title, 
    length, 
    rating, 
    RANK() OVER(PARTITION BY rating ORDER BY length) AS 'rank' 
FROM
    film
HAVING length >= 1;

-- 3. How many films are there for each of the categories in the category table? Hint: Use appropriate join between the tables "category" and "film_category".
SELECT 
    name AS 'film category',
    COUNT(*) AS 'films'
FROM
    category
        INNER JOIN
    film_category ON category.category_id = film_category.category_id
GROUP BY name;

-- 4. Which actor has appeared in the most films? Hint: You can create a join between the tables "actor" and "film actor" and count the number of times an actor appears.
SELECT 
    first_name,
    last_name,
    COUNT(actor.actor_id) AS 'participated in',
    RANK() OVER(ORDER BY count(actor.actor_id) DESC) AS 'rank'
FROM
    actor
        INNER JOIN
    film_actor ON actor.actor_id = film_actor.actor_id
GROUP BY actor.actor_id;

-- 5. Which is the most active customer (the customer that has rented the most number of films)?
-- Hint: Use appropriate join between the tables "customer" and "rental" and count the rental_id for each customer.
SELECT 
    customer.customer_id,
    COUNT(rental_id) AS 'rented movies'
FROM
    customer
        INNER JOIN
    rental ON customer.customer_id = rental.customer_id
GROUP BY customer.customer_id
ORDER BY COUNT(rental_id) DESC;


-- Bonus: Which is the most rented film? (The answer is Bucket Brotherhood).
-- This query might require using more than one join statement. Give it a try.
-- We will talk about queries with multiple join statements later in the lessons.
-- Hint: You can use join between three tables - "Film", "Inventory", and "Rental" and count the rental ids for each film.