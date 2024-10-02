/**1. Who is the senior most employee based on job title?**/
select first_name,last_name from employee order by levels desc limit 1;
/**2. Which countries have the most Invoices?**/
select count(*) as NO_of_invoices,billing_country from invoice group by billing_country order by NO_of_invoices desc;
/**3. What are top 3 values of total invoice?**/
select total from invoice order by total desc;
/**4. Which city has the best customers? We would like to throw a promotional Music 
Festival in the city we made the most money. Write a query that returns one city that 
has the highest sum of invoice totals. Return both the city name & sum of all invoice 
totals**/
select billing_country,sum(total) as total from invoice group by billing_country order by total desc limit 1;
/**5. Who is the best customer? The customer who has spent the most money will be 
declared the best customer. Write a query that returns the person who has spent the 
most money**/
select customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) as total_spending
from customer
join invoice on customer.customer_id = invoice.customer_id
group by customer.customer_id, customer.first_name, customer.last_name
order by total_spending desc
limit 1;
/**
1. Write query to return the email, first name, last name, & Genre of all Rock Music 
listeners. Return your list ordered alphabetically by email starting with A
**/
select distinct customer.first_name, customer.last_name, customer.email
from customer
join invoice on customer.customer_id = invoice.customer_id
join invoice_line on invoice.invoice_id = invoice_line.invoice_id
join track on invoice_line.track_id = track.track_id
join genre on track.genre_id = genre.genre_id
where genre.name = 'Rock'
order by customer.email;



/**2. Let's invite the artists who have written the most rock music in our dataset. Write a 
query that returns the Artist name and total track count of the top 10 rock bands**/
select artist.artist_id, artist.name, count(track.track_id) as songs
from track
join album on album.album_id = track.album_id
join artist on artist.artist_id = album.artist_id
join genre on genre.genre_id = track.genre_id
where genre.name = 'Rock'
group by artist.artist_id, artist.name
order by songs desc
limit 10;

/**3. Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the 
longest songs listed first**/
select  track_id,name,milliseconds from track where milliseconds> (select avg(milliseconds) as avr_track_length from track) 
order by milliseconds desc;
/**Q1: 
Find how much amount spent by each customer on artists?
Write a query to return customer name, artist name and total spent**/

with best_selling_artist as (
    select artist.artist_id as artist_id, artist.name as artist_name, 
           sum(invoice_line.unit_price * invoice_line.quantity) as total_sales
    from invoice_line
    join track on track.track_id = invoice_line.track_id
    join album on album.album_id = track.album_id
    join artist on artist.artist_id = album.artist_id
    group by artist.artist_id, artist.name
    order by total_sales desc
    limit 1
)
select c.customer_id, c.first_name, c.last_name, bsa.artist_name, 
       sum(il.unit_price * il.quantity) as amount_spent
from invoice i
join customer c on c.customer_id = i.customer_id
join invoice_line il on il.invoice_id = i.invoice_id
join track t on t.track_id = il.track_id
join album alb on alb.album_id = t.album_id
join best_selling_artist bsa on bsa.artist_id = alb.artist_id
group by c.customer_id, c.first_name, c.last_name, bsa.artist_name
order by amount_spent desc;


/**Q2: We want to find out the most popular music Genre for each country. 
We determine the most popular genre as the genre with the highest amount of purchases.
 Write a query that returns each country along with the top Genre.
 For countries where the maximum number of purchases is shared return all Genres.**/

select * from invoice;

with popular_genre as (

select count(invoice_line.quantity) as purchase, customer.country,genre.name,genre.genre_id,
row_number() over( partition by customer.country order by count(invoice_line.quantity) desc) as RowNO 
from invoice_line
JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id
	JOIN customer ON customer.customer_id = invoice.customer_id
	JOIN track ON track.track_id = invoice_line.track_id
	JOIN genre ON genre.genre_id = track.genre_id
GROUP BY 2,3,4
	ORDER BY 2 ASC, 1 DESC
)
SELECT * FROM popular_genre WHERE RowNo <= 1;
/**
Q3: Write a query that determines the customer that has spent the most on music for each country.
 Write a query that returns the country along with the top customer and how much they spent. 
 For countries where the top amount spent is shared, provide all customers who spent this amount
**/
with customer_with_country as (
    select c.customer_id, c.first_name, c.last_name, i.billing_country,
           sum(i.total) as total_spending,
           row_number() over(partition by i.billing_country order by sum(i.total) desc) as rowno
    from invoice i
    join customer c on c.customer_id = i.customer_id
    group by c.customer_id, c.first_name, c.last_name, i.billing_country
)
select * from customer_with_country where rowno <= 1;






































