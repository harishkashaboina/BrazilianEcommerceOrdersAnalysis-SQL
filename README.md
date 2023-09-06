# Brazilian e-commerce orders (made at Olist Store) Analysis

## Project Title : 
Brazilian e-commerce Orders Analysis - Using SQL

## About Dataset:
Welcome! This is a Brazilian e-commerce public dataset of orders made at Olist Store. The dataset has information on 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. Its features allow viewing orders from various dimensions: from order status, price, payment, and freight performance to customer location, product attributes, and finally reviews written by customers. 

Analyzing this extensive dataset makes it possible to gain valuable insights. The information can shed light on various aspects of the business, such as order processing, pricing strategies, payment and shipping efficiency, customer demographics, product characteristics, and customer satisfaction levels.

## Data set
The data is available in 8 CSV files:

- customers.csv
- sellers.csv
- order_items.csv
- geolocation.csv
- payments.csv
- reviews.csv
- orders.csv
- products.csv

The customers.csv contains the following features:

Features                  | Description
-------------             | -------------
customer_id               | ID of the consumer who made the purchase
customer_unique_id        | Unique ID of the consumer
customer_zip_code_prefix  | Zip Code of consumer’s location
customer_city             | Name of the City from where an order is made
customer_state            | State Code from where the order is made (Eg. são Paulo - SP)


The sellers.csv contains the following features:

Features                    | Description
-------------               | -------------
seller_id                   | Unique ID of the seller registered
seller_zip_code_prefix      | Zip Code of the seller’s location
seller_city                 | Name of the City of the seller
seller_state                | State Code (Eg. são paulo - SP)

The order_items.csv contains the following features:

Features                    | Description
-------------               | -------------
order_id                    | A Unique ID of the order made by the consumers
order_item_id               | A Unique ID is given to each item ordered in the order
product_id                  | A Unique ID given to each product available on the site
seller_id                   | Unique ID of the seller registered
shipping_limit_date         | The date before which the ordered product must be shipped
price                       | Actual price of the products ordered
freight_value               | Price rate at which a product is delivered from one point to another


The geolocations.csv contains the following features:

Features                        | Description
-------------                   | -------------
geolocation_zip_code_prefix     | First 5 digits of Zip Code
geolocation_lat                 | Latitude
geolocation_lng                 | Longitude
geolocation_city                | City
geolocation_state               | State


The payments.csv contains the following features:

Features                        | Description
-------------                   | -------------
order_id                        | A Unique ID of order made by the consumers
payment_sequential              | Sequences of the payments made in case of EMI
payment_type                    | Mode of payment used (Eg. Credit Card)
payment_installments            | Number of installments in case of EMI purchase
payment_value                   | Total amount paid for the purchase order


The orders.csv contains the following features:

Features                        | Description
-------------                   | -------------
order_id                        | A Unique ID of the order made by the consumers
customer_id                     | ID of the consumer who made the purchase
order_status                    | Status of the order made i.e. delivered, shipped, etc.
order_purchase_timestamp        | Timestamp of the purchase
order_delivered_carrier_date    | Delivery date at which the carrier made the delivery
order_delivered_customer_date   | Date on which the customer got the product
order_estimated_delivery_date   | Estimated delivery date of the products


The reviews.csv contains the following features:

Features                        | Description
-------------                   | -------------
review_id                       | ID of the review given on the product ordered by the order id
order_id                        | A Unique ID of order made by the consumers
review_score                    | Review score given by the customer for each order on a scale of 1-5
review_comment_title            | Title of the review
review_comment_message          | Review comments posted by the consumer for each order
review_creation_date            | Timestamp of the review when it is created
review_answer_timestamp         | Timestamp of the review answered


The products.csv contains the following features:

Features                        | Description
-------------                   | -------------
product_id                      | A Unique identifier for the proposed project.
product_category_name           | Name of the product category
product_name_lenght             | Length of the string that specifies the name given to the products ordered
product_description_lenght      | Length of the description written for each product ordered on the site
product_photos_qty              | Number of photos of each product ordered available on the shopping portal
product_weight_g                | Weight of the products ordered in grams
product_length_cm               | Length of the products ordered in centimeters
product_height_cm               | Height of the products ordered in centimeters
product_width_cm                | Width of the product ordered in centimeters

#### Schema

<img src="./images/schema.png" alt="schema"/>

## Analysis
1. The data type of columns in a table

```
SELECT * FROM `TABLE_CATALOG.TABLE_SCHEMA.INFORMATION_SCHEMA.COLUMNS` 
WHERE table_catalog='TABLE_CATALOG' and table_schema='TABLE_SCHEMA' and table_name='customers';
```
Here
- TABLE_CATALOG: The project ID of the project that contains the dataset
- TABLE_SCHEMA: The name of the dataset that contains the table also referred to as the datasetId

Output:
<img src="./images/ss1.png" alt="result"/>

Similarly, we can check for other tables as well. 

2. Time period for which the data is given

```
SELECT MIN(order_purchase_timestamp) AS min_order_date , MAX(order_purchase_timestamp) AS max_order_date FROM `ecomm.orders`
```

Max date: 2018-10-17 17:30:18 UTC
Min date: 2016-09-04 21:15:19 UTC
Observation: Order data between 09/2016 to 10/2018

Output:
<img src="./images/ss2.png" alt="result"/>

3. Cities and States of customers ordered during the given period

```
SELECT  DISTINCT c.customer_city,c.customer_state FROM `target.orders` AS o INNER JOIN `target.customers` c ON o.customer_id = c.customer_id WHERE EXTRACT (YEAR FROM o.order_purchase_timestamp) BETWEEN 2016 AND 2018
```

Output:
<img src="./images/ss3.png" alt="result"/>

4. Checking orders growing trend on e-commerce in Brazil. And checking seasonality with peaks at specific months.

```
SELECT EXTRACT(YEAR FROM o.order_purchase_timestamp) AS year,EXTRACT (MONTH FROM o.order_purchase_timestamp) AS month, COUNT(o.order_id) AS no_of_orders FROM `ecomm.orders` AS o GROUP BY 1, 2 ORDER BY 1, 2;
```

Output:
<img src="./images/ss4.png" alt="result"/>

- Orders trends increasing from 01/2017 to 11/2017
- Orders trends decreasing from 03/2018 to 07/2018
- Maximum orders in 11/2017
- Minimum orders in 12/2016

