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




