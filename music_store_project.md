# <p align="center" style="margin-top: 0px;"> Music Store Analysis 💰
## <p align="center"> Music Data Exploration

### QUESTION 1: WHO IS THE MOST SENIOR EMPLOYEE BASED ON JOB TITLE?
```sql
SELECT * FROM employee
ORDER BY levels DESC
LIMIT 1
```
### Output
employee_id| last_name | first_name | title | reports_to | levels
-- | -- | -- | -- | -- | --
9 | Madan |Mohan | Senior General Manager | | L7

Senior General Manager is the most senior employee

---

### QUESTION 2: WHICH COUNTRIES HAS THE MOST INVOICES?
```sql
SELECT billing_country, COUNT(*) FROM invoice 
GROUP BY billing_country ORDER BY billing_country DESC
LIMIT 1
```
### Output
billing_country | count
--|--
USA | 131
---

### QUESTION 3: WHAT ARE TOP 3 VALUES OF TOTAL INVOICES?
```sql
SELECT total FROM invoice
ORDER BY total DESC
LIMIT 3
```
### Output
|total|
|--|
|23.759999999999998|
|19.8|
|19.8|
---

### QUESTION 4: WHICH CITY HAS THE BEST CUSTOMERS? WE WOULD LIKE TO THROW A PROMOTIONAL MUSIC FESTIVAL IN THE CITY WE MADE THE MOST MONEY. WRITE A QUERY THAT RETURNS ONE CITY THAT HAS THE HIGHEST SUM OF INVOICE TOTALS. RETURNS BOTH THE CITY NAME & SUM OF ALL INVOICE TOTALS
```sql
SELECT billing_city, SUM(total) AS invoice_total FROM invoice
GROUP BY billing_city
ORDER BY invoice_total DESC
LIMIT 1
```
### Output
billing_city | invoice_total
--|--
Prague | 273.24000000000007
---

### QUESTION 5: WHO IS THE BEST CUSTOMER? THE CUSTOMER WHO HAS SPENT THE MOST MONEY WILL BE DECLARED AS THE BEST CUSTOMER. WRITE A QUERY THAT RETURNS THE PERSON WHO HAS SPENT THE MOST MONEY
```sql
SELECT customer.customer_id, customer.first_name, customer.last_name, 
SUM(invoice.total)
AS money_spent
FROM customer
JOIN invoice ON customer.customer_id= invoice.customer_id
GROUP BY customer.customer_id
ORDER BY money_spent DESC
LIMIT 1
```
### Output
customer_id | first_name | last_name | money_spent
-- | -- | -- | -- |
5 | R | Madhav | 144.54000000000002
---

### QUESTION 6: WRITE QUERY TO RETURN THE EMAIL, FIRST NAME, LAST NAME & GENRE OF ALL ROCK MUSIC LISTENERS. RETURN YOUR LIST ORDERED ALPHABETICALLY BY EMAIL STARTING WITH A
```sql
SELECT DISTINCT customer.email, customer.first_name, customer.last_name,  genre.name
AS genre_name
FROM customer
JOIN invoice ON customer.customer_id= invoice.customer_id
JOIN invoice_line ON invoice_line.invoice_id= invoice.invoice_id
JOIN track ON invoice_line.track_id= track.track_id
JOIN genre ON track.genre_id= genre.genre_id
WHERE genre.name LIKE 'Rock'
ORDER BY customer.email 
```
### Output
email | first_name | last_name | genre_name
-- | -- | -- | --
aaronmitchell@yahoo.ca | Aaron | Mitchell | Rock
alero@uol.com.br | Alexandre | Rocha | Rock
astrid.gruber@apple.at | Astrid | Gruber | Rock
bjorn.hansen@yahoo.no | Bjørn | Hansen | Rock
camille.bernard@yahoo.fr | Camille | Bernard | Rock
daan_peeters@apple.be | Daan | Peeters | Rock
diego.gutierrez@yahoo.ar | Diego | Gutiérrez | Rock
dmiller@comcast.com | Dan | Miller | Rock
dominiquelefebvre@gmail.com | Dominique | Lefebvre | Rock
edfrancis@yachoo.ca | Edward | Francis | Rock
eduardo@woodstock.com.br | Eduardo | Martins | Rock
ellie.sullivan@shaw.ca | Ellie | Sullivan | Rock
emma_jones@hotmail.com | Emma | Jones |Rock
enrique_munoz@yahoo.es | Enrique | Muñoz | Rock
fernadaramos4@uol.com.br | Fernanda | Ramos | Rock
fharris@google.com | Frank | Harris | Rock
fralston@gmail.com | Frank | Ralston | Rock
ftremblay@gmail.com | François | Tremblay | Rock
fzimmermann@yahoo.de | Fynn | Zimmermann | Rock
hannah.schneider@yahoo.de | Hannah | Schneider | Rock
hholy@gmail.com | Helena | Holý | Rock
hleacock@gmail.com | Heather | Leacock | Rock
hughoreilly@apple.ie | Hugh | O'Reilly | Rock
isabelle_mercier@apple.fr | Isabelle | Mercier | Rock
---

### QUESTION 7: LET'S INVITE THE ARTIST WHO HAVE WRITTEN THE MOST ROCK MUSIC IN OUR DATASET. WRITE A QUERY THAT RETURNS THE ARTIST NAME AND TOTAL TRACK COUNT OF THE TOP 10 ROCK BANDS
```sql
SELECT artist.artist_id, artist.name/COUNT(artist.artist_id)
AS number_of_songs
FROM artist
JOIN album ON album.artist_id= artist.artist_id
JOIN track ON track.album_id= album.album_id
JOIN genre ON track.genre_id= genre.genre_id
WHERE genre.name LIKE 'Rock'
GROUP BY artist.artist_id
ORDER BY number_of_songs DESC
LIMIT 10
```
### Output
artist_id | name | number_of_songs
-- | -- | --
22 | Led Zeppelin | 114
150 | U2 | 112
58 | Deep Purple | 92
90 | Iron Maiden | 81
118 | Pearl Jam | 54
152 | an Halen | 52
51 | Queen | 45
142 | The Rolling Stones | 41
76 | Creedence Clearwater Revival | 40
52 | Kiss | 35
---

### QUESTION 8: RETURN ALL THE TRACK NAMES THAT HAVE SONG LENGTH LONGER THAN THE AVERAGE SONG LENGTH. RETURN THE NAME AND MIULLISECONDS FOR EACH TRACK. ORDER BY THE LONEST SONG  LISTED FIRST. RETURN THE TRACK NAME AND EMAILS OF CUSTOMERS THAT HAVE LISTENED TO SONGS LNGER THAN THE AVERAGE TRACK LENGTH.
```sql
SELECT name, milliseconds FROM track
WHERE milliseconds > (
	SELECT AVG(milliseconds) FROM track
)
ORDER BY milliseconds DESC
```
### Output
name | milliseconds
-- | --
Occupation / Precipice | 5286953
Through a Looking Glass | 5088838
Greetings from Earth, Pt. 1 |	2960293
The Man With Nine Lives | 2956998
Battlestar Galactica, Pt. 2 |	2956081
Battlestar Galactica, Pt. 1 |	2952702
Murder On the Rising Star | 2935894
Battlestar Galactica, Pt. 3 |	2927802
Take the Celestra | 2927677
Fire In Space | 2926593
The Long Patrol | 2925008
The Magnificent Warriors | 2924716
The Living Legend, Pt. 1 | 2924507
The Gun On Ice Planet Zero, Pt. 2 | 2924341
The Hand of God | 2924007
Experiment In Terra | 2923548
War of the Gods, Pt. 2 | 2923381
The Living Legend, Pt. 2 | 2923298
War of the Gods, Pt. 1 | 2922630
Lost Planet of the Gods, Pt. 1 | 2922547
Baltar's Escape | 2922088
The Lost Warrior |	2920045
Lost Planet of the Gods, Pt. 2 | 2914664
The Gun On Ice Planet Zero, Pt. 1 |	2907615
Greetings from Earth, Pt. 2 |	2903778
Crossroads, Pt. 2 |	2869953
The Young Lords | 2863571
Dave | 2825166
? | 2782333
Maternity Leave | 2780416
Three Minutes |	2763666
Hero |	2713755
One of Them | 2698791
How to Stop an Exploding Man |	2687103
The Long Con |	2679583
Live Together, Die Alone, Pt. 2 |	2656531
S.O.S. |  2639541
One of Us | 2638096
The Man from Tallahassee | 2637637
The Cost of Living | 2637500
The Glass Ballerina | 2637458
.....
---

### QUESTION 9: FIND HOW MUCH AMOUNT SPENT BY EACH CUSTOMER ON ARIST? WRITE A QUERY TO RETURN CUSTOMER NAME, ARTIST NAME AND TOTAL SPENT?
```sql
SELECT customer.customer_id, customer.first_name, customer.last_name, artist.name,
SUM(invoice_line.unit_price * invoice_line.quantity) AS money_spent FROM customer
JOIN invoice ON customer.customer_id= invoice.customer_id
JOIN invoice_line ON invoice_line.invoice_id= invoice.invoice_id
JOIN track ON invoice_line.track_id= track.track_id
JOIN album ON track.album_id= album.album_id
JOIN artist ON album.artist_id= artist.artist_id
GROUP BY customer.customer_id, artist.name
ORDER BY money_spent DESC
```
### Output
customer_id | first_name | last_name | name | money_spent
-- | -- | -- | -- | --
46 | Hugh | O'Reilly | Queen | 27.719999999999985
42 | Wyatt | Girard  | Frank Sinatra | 23.75999999999999
3	| François | Tremblay | The Who |	19.799999999999997
6	| Helena  |	Holý | Red Hot Chili Peppers | 19.799999999999997
5	| R  | Madhav |  Kiss | 19.799999999999997
29 | Robert | Brown  | Creedence Clearwater Revival | 19.799999999999997
32 | Aaron  | Mitchell | ames Brown | 19.799999999999997
22 | Heather | Leacock | House Of Pain |	18.81
46	| Hugh | O'Reilly | Nirvana |	18.81
38	| Niklas | Schröder | Queen |	18.81
26	| Richard | Cunningham | Marvin Gaye | 17.82
45	| Ladislav |	Kovács | The Cult |	17.82
39	| Camille  | Bernard | Marisa Monte |	17.82
1	| Luís | Gonçalves | The Cult | 17.82
20 | Dan | Miller | Eric Clapton |	17.82
46	| Hugh  | O'Reilly | Marisa Monte |	17.82
3	| François |Tremblay | Queen |	17.82
5	| R | Madhav | Eric Clapton | 17.82
55 | Mark | Taylor | The Clash | 17.82
54 | Steve | Murray | AC/DC | 17.82
.......
---

### QUESTION 10: WE WANT TO FIND OUT THE MOST POPULAR MUSIC GENRE FOR EACH COUNTRY WE DETERMINE THE MOST POPULAR MUSIC GENRE AS THE GENRE WITH THE HIGHEST AMOUNT OF PURCHASES. WRITE A QUERY THAT RETURNS EACH COUNTRY ALONG WITH THE TOP GENRE. FOR COUNTRIES WHERE THE MAXIMUM NUMBER OF PURCHASES IS SHARED RETURN ALL GENRE.
```sql
WITH highest_purchase AS (
	SELECT customer.country, genre.name, genre.genre_id, COUNT(genre.genre_id)
	AS purchases,
	ROW_NUMBER() OVER (PARTITION BY customer.country ORDER BY COUNT(genre.genre_id) DESC)
	AS row_part
	FROM customer
	JOIN invoice ON invoice.invoice_id= customer.customer_id
	JOIN invoice_line ON invoice_line.invoice_id= invoice.invoice_id
	JOIN track ON invoice_line.track_id= track.track_id
	JOIN genre ON track.genre_id= genre.genre_id
	GROUP BY 1, 2, 3
	ORDER BY 1 DESC
)
SELECT highest_purchase.country, highest_purchase.name, highest_purchase.purchases
FROM highest_purchase
WHERE row_part = 1
```
### Output
country	| name | purchases
-- | -- | --
USA | Rock | 65
United Kingdom | R&B/Soul | 18
Sweden | Rock | 3
Spain | Rock | 3
Portugal | Rock | 3
Poland | Metal |	4
Norway | Rock | 3
Netherlands | Rock | 4
Italy | Rock |	4
Ireland | Rock | 11
India | Rock |	9
Hungary | Metal |	4
Germany | Rock | 14
France | Rock |	25
Finland | Rock |	12
Denmark | Rock | 2
Czech Republic | Rock |	18
Chile | Rock |	1
Canada | Rock | 50
Brazil  | Rock |	28
Belgium |Rock |	6
Austria | Rock |	6
Australia | Rock | 7
Argentina | Rock |	17
---

### QUESTION 11: WRITE A QUERY THAT DETERMINES THE CUSTOMER THAT HAS SPENT THE MOST ON MUSIC FOR EACH COUNTRY, WRITE A QUERY THAT RETURNS THE COUNTRY ALONG WITH THE TOP  CUTOMER AND HOW MUCH THEY SPENT. FOR COUNTRIES WHERE THE TOP AMOUNT SPENT IS SHARED, PROVIDE ALL CUSTOMERS WHO SPENT THIS AMOUNT
```sql
WITH top_customer AS (
	SELECT customer.customer_id, customer.first_name, customer.last_name,
	customer.country, SUM(invoice.total) AS money_spent,
	ROW_NUMBER() OVER (PARTITION BY customer.country ORDER BY SUM(invoice.total) DESC )
	AS row_num
	FROM invoice
	JOIN customer ON invoice.customer_id= customer.customer_id
	GROUP BY customer.customer_id
	ORDER BY 4
)
SELECT top_customer.country, top_customer.first_name, top_customer.last_name,
top_customer.money_spent FROM top_customer
WHERE row_num= 1
```
### Output
country | first_name | last_name | money_spent
-- | -- | -- | --
Argentina | Diego | Gutiérrez | 39.6
Australia | Mark | Taylor | 81.18
Austria | Astrid | ruber | 69.3
Belgium | Daan | Peeters | 60.38999999999999
Brazil | Luís | Gonçalves | 108.89999999999998
Canada | François | remblay | 99.99
Chile | Luis | Rojas | 97.02000000000001
Czech Republic | R | Madhav | 144.54000000000002
Denmark | Kara | Nielsen | 37.61999999999999
Finland | Terhi | Hämäläinen | 79.2
France | Wyatt | Girard | 99.99
Germany | Fynn  | Zimmermann | 94.05000000000001
Hungary | Ladislav |  Kovács | 78.21
India | Manoj | Pareek | 111.86999999999999
Ireland | Hugh | O'Reilly | 14.83999999999997
Italy | Lucas | Mancini | 50.49
Netherlands | Johannes | Van der Berg | 65.34
Norway | Bjørn | Hansen | 72.27000000000001
Poland | Stanisław | Wójcik | 76.22999999999999
Portugal | João | Fernandes | 102.96000000000001
Spain | Enrique | Muñoz | 98.01
Sweden | Joakim | Johansson  | 75.24
United Kingdom | Phil |  Hughes | 98.01
USA | Jack | Smith | 98.01
---
