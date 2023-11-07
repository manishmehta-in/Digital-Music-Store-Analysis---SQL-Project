# Digital Music Store Analysis - SQL Project

## Objective

As an aspiring data analyst, this project serves as a hands-on opportunity to apply and enhance my SQL skills while exploring a real-world dataset. The main objective is to delve into the digital music store's database and extract valuable insights through structured SQL queries. By addressing a set of progressively challenging questions, I aim to demonstrate my ability to manipulate data, draw meaningful conclusions, and communicate the results effectively. This project is a stepping stone in my journey towards a career in data analysis, allowing me to showcase my analytical capabilities and make a compelling case for potential employers.

## Project Overview

This project involves the analysis of a digital music store's database using SQL. By addressing a series of SQL queries, we will uncover interesting patterns and derive valuable insights from the dataset.
The project is divided into three question sets, ranging from easy to advanced, each designed to test and improve SQL skills. The goal is not only to obtain the correct answers but also to develop an efficient, structured, and readable SQL code. The project helps demonstrate competence in SQL data analysis and problem-solving.

## Project Structure

- **SQLQueries:** This directory contains SQL queries used to answer the project questions. Each question set has a separate SQL file.

## Project Results

For the results of each question set, you can refer to the project's SQL queries and their respective output, which provides the insights and answers derived from the dataset.

###### Easy Questions

### Q1 Who is the senior most employee based on job title?
```sql
 SELECT * FROM employee
            ORDER BY levels desc
            limit 1

```
## Result
![Q1_RESULT](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/deace825-94f1-491e-9aa1-7ca160224ccf)

### Q2 Which countries have the most Invoices?

```sql
SELECT COUNT(*) as c, billing_country
FROM invoice
GROUP BY billing_country 
ORDER BY c desc

```
## Result
![Q2_RESULT](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/7587c0c5-4c0d-4187-9d59-54471ece98f0)

### Q3 What are the top 3 values of total invoice?
```sql
SELECT total FROM invoice
ORDER BY total desc
limit 3

```
## Result
![Q3_RESULT](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/4fb14240-be63-440f-9a6e-b2d765776176)

### Q4 Which city has the best customers? We would like to throw a promotional music festival in the city where we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals ?
```sql
SELECT SUM(total) as invoice_total, billing_city
FROM invoice
group by billing_city 
order by invoice_total desc

```
## Result
![Q4_RESULT](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/c29dcd61-4013-49bb-b0c6-d04e40289925)

### Q5 Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money?

```sql
SELECT customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) as total
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
GROUP BY customer.customer_id
ORDER BY total DESC
limit 1

```
## Result
![Q5_RESULT](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/78c28ef3-3b98-486f-9e58-b887db7ada72)

###### Moderate Questions
### Q6 Write a query to return the email, first name, last name, & Genre of all Rock Music listeners. Return your list ordered alphabetically by email starting with A

```sql
SELECT DISTINCT email, first_name, last_name
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id
WHERE track_id IN(
	SELECT track_id FROM track
	JOIN genre ON track.genre_id = genre.genre_id
	WHERE genre.name LIKE 'Rock'
)
ORDER BY email;

```
#### Method -2
```sql
SELECT DISTINCT email AS Email,first_name AS FirstName, last_name AS LastName, genre.name AS Name
FROM customer
JOIN invoice ON invoice.customer_id = customer.customer_id
JOIN invoice_line ON invoice_line.invoice_id = invoice.invoice_id
JOIN track ON track.track_id = invoice_line.track_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock'
ORDER BY email;

```
## Result
![Q6 Result](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/9555eb0b-c637-4dfe-a7d6-1dcb09a6ad73)

### Q7 Let’s invite the artist who has written the most rock music in our dataset. Write a query that returns the Artist name and total track count of the top 10 rock bands

```sql
SELECT artist.artist_id, artist.name, COUNT(artist.artist_id) AS number_of_songs
FROM track
JOIN album ON album.album_id = track.album_id
JOIN artist ON artist.artist_id = album.artist_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock'
GROUP BY artist.artist_id
ORDER BY number_of_songs DESC
LIMIT 10;

```
## Result
![Q7 result](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/107a9f68-a8bd-467c-88ca-ecb98f448a36)

### Q8 Return all the track names that have a song length longer than the average song length. Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first.

```sql
SELECT name,milliseconds
FROM track 
WHERE milliseconds > (
	SELECT AVG(milliseconds) AS avg_track_length
	FROM track
)
ORDER BY milliseconds DESC;

```
## Result
![Q8 result](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/854e9bd7-61d0-4381-a16a-38605fc8a107)

###### Advance Questions
### Q9 Find how much amount is spent by each customer on the artist? Write a query to return customer name, artist name and total spent.
```sql
WITH best_selling_artist AS (
	SELECT artist.artist_id AS artist_id, artist.name AS artist_name, 
	SUM(invoice_line.unit_price*invoice_line.quantity) AS total_sales
	FROM invoice_line
	JOIN track ON track.track_id = invoice_line.track_id
	JOIN album ON album.album_id = track.album_id
	JOIN artist ON artist.artist_id = album.artist_id
	GROUP BY 1
	ORDER BY 3 DESC
	LIMIT 1
)
SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name, SUM(il.unit_price*il.quantity)
As amount_spent
FROM invoice i
JOIN customer c ON c.customer_id = i.customer_id
JOIN invoice_line il ON il.invoice_id = i.invoice_id
JOIN track t ON t.track_id = il.track_id
JOIN album alb ON alb.album_id = t.album_id
JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id
GROUP BY 1,2,3,4
ORDER BY 5 DESC;

```
## Result
![Q9 result](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/37905571-1f75-4d86-8ae3-b3d036056c59)

### Q10 We want to find out the most popular music Genre for each country. We determine the most popular genre as the genre with the highest amount  of purchases. Write a query that returns each country along with the top Genre. For countries where the maximum number of purchases is shared, return all Genres.

#### Method -1
```sql
WITH popular_genre AS
(
	SELECT COUNT(invoice_line.quantity) AS purchases, customer.country, genre.name, genre.genre_id,
	ROW_NUMBER() OVER(PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity) DESC) AS RowNo
	FROM invoice_line
	JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id
	JOIN customer ON customer.customer_id = invoice.customer_id
	JOIN track ON track.track_id = invoice_line.track_id
	JOIN genre ON genre.genre_id = track.genre_id
	GROUP BY 2,3,4
	ORDER BY 2 ASC, 1 DESC
)
SELECT * FROM popular_genre WHERE RowNo <= 1

```
#### Method-2
```sql
WITH RECURSIVE
    sales_per_country AS(
		SELECT COUNT(*) AS purchases_per_genre, customer.country, genre.name, genre.genre_id
		FROM invoice_line
		JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id
		JOIN customer ON customer.customer_id = invoice.customer_id
		JOIN track ON track.track_id = invoice_line.track_id
		JOIN genre ON genre.genre_id = track.genre_id
		GROUP BY 2,3,4
		ORDER BY 2
	),
	max_genre_per_country AS (SELECT MAX(purchases_per_genre) AS max_genre_number, country
							 FROM sales_per_country
							 GROUP BY 2
							 ORDER BY 2)

SELECT sales_per_country.*
FROM sales_per_country
JOIN max_genre_per_country ON sales_per_country.country = max_genre_per_country.country
WHERE sales_per_country.purchases_per_genre = max_genre_per_country.max_genre_number

```
## Result
![Q10 result](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/3b6e7f30-b325-4dd7-b9ca-d4fa998734a6)

### Q11 Write a query that determines the customer that has spent the most on music for each country. Write a query that returns the country along with the top customer and how much they spent. For countries where the top amount spent is shared, provide all customers who spent this amount
#### Method -1: Recursive Method
```sql
WITH RECURSIVE
    customer_with_country AS (
		SELECT customer.customer_id, first_name, last_name, billing_country, SUM(total) AS total_spending
		FROM invoice
		JOIN customer ON customer.customer_id = invoice_id
		GROUP BY 1,2,3,4
		ORDER BY 2,3 DESC
	),
	country_max_spending AS(
		SELECT billing_country, MAX(total_spending) AS max_spending
		FROM customer_with_country
		GROUP BY billing_country
	)
SELECT cc.billing_country, cc.total_spending, cc.first_name, cc.last_name, cc.customer_id
FROM customer_with_country cc
JOIN country_max_spending ms
ON cc.billing_country = ms.billing_country
WHERE cc.total_spending = ms.max_spending
ORDER BY 1;

```
#### Method -2: CTE Method
```sql
WITH customer_with_country AS (
	SELECT customer.customer_id,first_name,last_name,billing_country,SUM(total) AS total_spending,
	ROW_NUMBER() OVER(PARTITION BY billing_country ORDER BY SUM(total) DESC) AS RowNo
	FROM invoice
	JOIN customer ON customer.customer_id = invoice.customer_id
	GROUP BY 1,2,3,4
	ORDER BY 4 ASC, 5 DESC
)
SELECT * FROM customer_with_country WHERE RowNo <= 1

```
## Result
![Q11 result](https://github.com/manishmehta-in/Digital-Music-Store-Analysis---SQL-Project/assets/140696340/f945206a-cbdb-4327-aff7-c584ae764ff3)

