# Basic-SQL-Commands-for-Sales-Analysis
A list of 8 basic commands meant to be used for sales

-- Note: Make sure to run this file fromn top to bottom in a SQLite client, aside from that, enjoy.

DROP TABLE IF EXISTS sales;

CREATE TABLE sales(
    sale_id     INTEGER PRIMARY KEY,
    sale_date   TEXT NOT NULL,
    product     TEXT NOT NULL,
    category    TEXT NOT NULL,
    quantity    INTEGER NOT NULL CHECK (quantity > 0),
    unit_price  REAL NOT NULL CHECK (unit_price >= 0),
    region      TEXT NOT NULL
);

-- Example materials below
INSERT INTO sales (sale_id, sale_date, product, category, quantity, unit_price, region) VALUES
    (1,  '2026-01-03', 'Notebook',       'Stationery', 3,  4.50, 'North'),
    (2,  '2026-01-04', 'Water Bottle',   'Accessories',1, 18.00, 'West'),
    (3,  '2026-01-05', 'Pen Set',        'Stationery', 2,  7.25, 'East'),
    (4,  '2026-01-06', 'Backpack',       'Bags',       1, 42.00, 'South'),
    (5,  '2026-01-07', 'Notebook',       'Stationery', 5,  4.50, 'West'),
    (6,  '2026-01-09', 'Desk Lamp',      'Home',       1, 29.99, 'North'),
    (7,  '2026-01-10', 'Water Bottle',   'Accessories',2, 18.00, 'East'),
    (8,  '2026-01-11', 'Backpack',       'Bags',       2, 42.00, 'West'),
    (9,  '2026-01-13', 'Pen Set',        'Stationery', 4,  7.25, 'South'),
    (10, '2026-01-14', 'Desk Lamp',      'Home',       2, 29.99, 'East'),
    (11, '2026-01-16', 'Notebook',       'Stationery', 2,  4.50, 'North'),
    (12, '2026-01-17', 'Water Bottle',   'Accessories',3, 18.00, 'South'),
    (13, '2026-01-19', 'Backpack',       'Bags',       1, 42.00, 'East'),
    (14, '2026-01-21', 'Pen Set',        'Stationery', 1,  7.25, 'West'),
    (15, '2026-01-22', 'Desk Lamp',      'Home',       1, 29.99, 'South'),
    (16, '2026-01-24', 'Notebook',       'Stationery', 4,  4.50, 'East'),
    (17, '2026-01-26', 'Water Bottle',   'Accessories',1, 18.00, 'North'),
    (18, '2026-01-28', 'Backpack',       'Bags',       1, 42.00, 'North'),
    (19, '2026-01-29', 'Pen Set',        'Stationery', 3,  7.25, 'East'),
    (20, '2026-01-31', 'Desk Lamp',      'Home',       1, 29.99, 'West');

-- First command: Inspect the first few records.
SELECT * 
FROM sales
ORDER BY sale_date
LIMIT 5;

-- Second command: Find sales from a region.
SELECT sales_date, product, quantity, region
FROM sales
WHERE region = 'West' -- Replace 'West' with any other region wanted
ORDER BY sales_date;

-- Third command: Finding higher value individual items (ie. unit price being => $20)
SELECT product, category, unit_price
FROM sales
WHERE unit_price >= 20
ORDER BY unit_price DESC;

-- Fourth command: Calculate revenue for each sale line.
SELECT sale_id, product, quantity, unit_price, ROUND(quantity * unit_price, 2) AS line_revenue
FROM sales
ORDER BY line_revenue DESC;

-- Fifth command: Count sale records and units sold.
SELECT COUNT(*) AS number_of_sales, SUM(quantity) AS units_sold
FROM sales;

 -- Sixth command: Summarize units and revenue by product category.
SELECT category, SUM(quantity) AS units_sold, ROUND(SUM(quantity * unit_price), 2) AS revenue
FROM sales
GROUP BY category
ORDER BY revenue DESC;

-- Seventh command: Comparing revenue across regions.
SELECT region, ROUND(SUM(quantity * unit_price), 2) AS revenue
FROM sales
GROUP BY region
ORDER BY revenue DESC;

-- Eighth command: Showing products with at least four sale records.
SELECT product, COUNT(*) AS number_of_sales, SUM(quantity) AS units_sold
FROM sales
GROUP BY product
HAVING COUNT(*) >= 4
ORDER BY number_of_sales DESC, product;

-- Thanks for reading - SAT
