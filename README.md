# SQL_Project_Music_Store_Analysis

SQL project to analyze online music store data (Postgre SQL Server)
![PNG](https://github.com/user-attachments/assets/5a185087-bb14-47d4-9819-ff451ecdb1ee)

This beginner-friendly project will guide you through analyzing a music playlist database. Using SQL, 
you'll explore the dataset and help the store gain insights into its business growth by answering a series of straightforward questions.

I received a database file and a questionnaire from my university for reference. and same has been attached in the profile also as a part of project.

The project is divided into three levels of difficulty: Easy, Medium, and Hard.
& Each level contains a minimum of 3 to 5 questions.

## Question set one - Easy

Question 1 - Who is the senior most employee based on job title?

Ans - Madan Mohan emp ID 9 (senior genral manager) is the most senior employee based on job title.

    SELECT title, last_name, first_name 
    FROM employee
    ORDER BY levels DESC
    LIMIT 1

Question 2: Which countries have the most Invoices?

Ans - "country"	"billing_country"
131	"USA"
76	"Canada"
61	"Brazil"
50	"France"
41	"Germany"
30	"Czech Republic" Etc......

    SELECT count (*) as Country, billing_country
    from invoice
    group by billing_country
    order by Country desc;

Question 3: What are top 3 values of total invoice?

Ans  - "total"
1- 23.759999999999998
2- 19.8
3- 19.8

    SELECT total from invoice
    order by total desc
    limit 3

Question 4: Which city has the best customers? we would like to throw a promotional music festival in the coty we made the most money. write a query that returns one city that has the highest sum of invoice totals 
Return both the city name and sum of all invoice totals.

Ans - Prague has the best customer base with total of 
"invoice_total"
273.24000000000007

    Select sum (total) as invoice_total, billing_city
    from invoice
    group by billing_city
    order by invoice_total desc

Question 5: Who is the best customer? The customer who has spent the most money will be declared the best customer. 
Write a query that returns the person who has spent the most money.*/

Ans -  "customer_id"	"first_name"	"last_name"	"total"
ID:5	"R "	"Madhav   "	144.54000000000002 

    Select customer.customer_id, customer.first_name, customer.last_name, sum(invoice.total) as total
    from customer
    join invoice on customer.customer_id = invoice.customer_id
    group by customer.customer_id
    order by total desc
    limit 1

****************************************************************************************************************************************************************************************

## Question set two - Modrate

Question 1 - Write query to return the email, first name, last name, & Genre of all Rock Music listeners. Return your list ordered alphabetically by email starting with A.

Ans - "aaronmitchell@yahoo.ca"	"Aaron  "	"Mitchell "
      "alero@uol.com.br"	"Alexandre    "	"Rocha    "
      "astrid.gruber@apple.at"	"Astrid "	"Gruber   "

    Select distinct email, first_name, last_name
    from customer
    join invoice on customer.customer_id = invoice.invoice_id
    join invoice_line on invoice.invoice_id = invoice_line.invoice_id
    where track_id  In(
               select track_id from track
			   join genre on track.genre_id = genre.genre_id
			   where genre.name  like 'Rock'
      )
      order by email;

Question 2 - Let's invite the artists who have written the most rock music in our dataset. 
Write a query that returns the Artist name and total track count of the top 10 rock bands.

Ans -  "Artistid"   "Name"   "No of songs"
"22"	"Led Zeppelin"	114
"150"	"U2"	112
"58"	"Deep Purple"	92
"90"	"Iron Maiden"	81
"118"	"Pearl Jam"	54
"152"	"Van Halen"	52
"51"	"Queen"	45
"142"	"The Rolling Stones"	41
"76"	"Creedence Clearwater Revival"	40

    SELECT artist.artist_id, artist.name,COUNT(artist.artist_id) AS number_of_songs
    FROM track
    JOIN album ON album.album_id = track.album_id
    JOIN artist ON artist.artist_id = album.artist_id
    JOIN genre ON genre.genre_id = track.genre_id
    WHERE genre.name LIKE 'Rock'
    GROUP BY artist.artist_id
    ORDER BY number_of_songs DESC
    LIMIT 10;
  
Question 3 - Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first.

Ans = "name"	"milliseconds"
"Occupation / Precipice"	5286953
"Through a Looking Glass"	5088838
"Greetings from Earth, Pt. 1"	2960293
"The Man With Nine Lives"	2956998
"Battlestar Galactica, Pt. 2"	2956081


    select name, milliseconds
    from track
    where milliseconds > 
    (select avg (milliseconds) as avg_track_length
    from track)
	order by milliseconds desc;

## Question set Three - Advance

Question 1 - Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent?

Ans = "customer_id"	"first_name"	"last_name"	"artist_name"	"amount_spent"
46	"Hugh                                              "	"O'Reilly                                          "	"Queen"	27.719999999999985
38	"Niklas                                            "	"Schröder                                          "	"Queen"	18.81
3	"François                                          "	"Tremblay                                          "	"Queen"	17.82




    WITH best_selling_artist AS (
	SELECT artist.artist_id AS artist_id, artist.name AS artist_name, SUM(invoice_line.unit_price*invoice_line.quantity) AS total_sales
	FROM invoice_line
	JOIN track ON track.track_id = invoice_line.track_id
	JOIN album ON album.album_id = track.album_id
	JOIN artist ON artist.artist_id = album.artist_id
	GROUP BY 1
	ORDER BY 3 DESC
	LIMIT 1)

    SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name, SUM(il.unit_price*il.quantity) AS amount_spent
    FROM invoice i
    JOIN customer c ON c.customer_id = i.customer_id
    JOIN invoice_line il ON il.invoice_id = i.invoice_id
    JOIN track t ON t.track_id = il.track_id
    JOIN album alb ON alb.album_id = t.album_id
    JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id
    GROUP BY 1,2,3,4
    ORDER BY 5 DESC;
