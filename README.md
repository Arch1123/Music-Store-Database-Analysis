# Music Store Database Analysis

## Project Description
This project analyzes a music store’s sales, customers, and inventory using SQL. The dataset includes albums, artists, customers, invoices, employees, genres, playlists, and tracks. The goal is to extract business insights through SQL queries.

## Tech Stack
- **Database**: PostgreSQL / MySQL / SQL Server  
- **Tools**: pgAdmin / MySQL Workbench / SQLite  
- **Data Source**: Provided CSV files  

## 1 Database Schema Design
The music store database consists of the following tables:

| Table Name      | Description                                        |
|----------------|----------------------------------------------------|
| artist         | Stores artist details                             |
| album          | Contains album details linked to artists         |
| track          | Tracks and their metadata, including album, genre, and media type |
| genre          | Stores different music genres                     |
| media_type     | Defines available media formats                   |
| playlist       | Stores playlists created by customers             |
| playlist_track | Links tracks to playlists                         |
| customer       | Stores customer details                           |
| invoice        | Sales transactions with customers                 |
| invoice_line   | Detailed breakdown of each invoice                |
| employee       | Stores employee details                           |

## 2 SQL Queries & Business Insights

### A. Customer Analysis
#### Top 5 customers who spent the most money:
```sql
SELECT c.customer_id, c.first_name, c.last_name, SUM(i.total) AS total_spent
FROM customer c
JOIN invoice i ON c.customer_id = i.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name
ORDER BY total_spent DESC
LIMIT 5;
```
#### Number of customers by country:
```sql
SELECT country, COUNT(customer_id) AS total_customers
FROM customer
GROUP BY country
ORDER BY total_customers DESC;
```

### B. Sales Performance
#### Monthly revenue trend:
```sql
SELECT DATE_TRUNC('month', invoice_date) AS month, SUM(total) AS revenue
FROM invoice
GROUP BY month
ORDER BY month;
```
#### Most sold tracks:
```sql
SELECT t.name, COUNT(il.track_id) AS times_sold
FROM invoice_line il
JOIN track t ON il.track_id = t.track_id
GROUP BY t.name
ORDER BY times_sold DESC
LIMIT 10;
```

### C. Inventory & Music Insights
#### Top 5 most popular genres by sales:
```sql
SELECT g.name AS genre, COUNT(il.track_id) AS total_sold
FROM invoice_line il
JOIN track t ON il.track_id = t.track_id
JOIN genre g ON t.genre_id = g.genre_id
GROUP BY g.name
ORDER BY total_sold DESC
LIMIT 5;
```
#### Albums with the highest number of tracks:
```sql
SELECT a.title, COUNT(t.track_id) AS track_count
FROM album a
JOIN track t ON a.album_id = t.album_id
GROUP BY a.title
ORDER BY track_count DESC
LIMIT 5;
```

## 3 Database Optimization
### Indexes: Improve query performance using indexes.
```sql
CREATE INDEX idx_invoice_date ON invoice(invoice_date);
CREATE INDEX idx_customer_country ON customer(country);
```
### Views: Create pre-aggregated views for frequent queries.
```sql
CREATE VIEW top_customers AS
SELECT c.customer_id, c.first_name, c.last_name, SUM(i.total) AS total_spent
FROM customer c
JOIN invoice i ON c.customer_id = i.customer_id
GROUP BY c.customer_id, c.first_name, c.last_name
ORDER BY total_spent DESC;
```

## 4 How to Run This Project
```sh
git clone https://github.com/yourusername/music-store-sql.git
cd music-store-sql
```
1. Import the dataset into PostgreSQL/MySQL.
2. Run SQL queries in your database tool.

---

### 📌 Contributions
Feel free to fork this repository and make improvements!



