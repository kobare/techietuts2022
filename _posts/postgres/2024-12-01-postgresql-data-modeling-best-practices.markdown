---
layout: post
title:  "PostgreSQL Data Modeling Best Practices"
author: Denis Kobare
date:   2024-12-01 07:00:00 +0300
img: /assets/img/svg/postgresql.svg
categories: databases
sub_category: postgres
type: concepts
technology: postgres
permalink: "category/:categories/postgres/concepts/:year:month/:title"
---



### Introduction

When it comes to designing efficient and scalable database schemas in PostgreSQL, 
there are several best practices to keep in mind. Proper data modeling can 
significantly impact the performance and maintainability of your database. In 
this section, we'll explore key concepts such as normalization, denormalization, 
table partitioning, and considerations for handling large datasets. We'll also 
provide practical code explanations where applicable to help you implement these 
practices effectively.



<br>
### 1. Normalization

Normalization is a fundamental concept in database design that aims to eliminate 
data redundancy and improve data integrity. It involves breaking down a single 
table into multiple related tables, reducing the chances of data anomalies. 
PostgreSQL supports normalization through the use of primary keys, foreign keys, 
and relationships.


<br>
#### Example:

Let's say we have a database for an e-commerce platform. We want to store 
customer information along with their orders. A normalized approach would 
involve creating separate tables for customers and orders.

<br>
{% highlight sql %}

-- Customers Table
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);

-- Orders Table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    order_date DATE,
    total_amount DECIMAL(10, 2)
);

{% endhighlight %}

<br>
In this example, the <span class="badge">customer_id</span> column in the orders 
table is a foreign key referencing the <span class="badge">customer_id</span> 
primary key in the customers table.



<br>
### 2. Denormalization

While normalization helps maintain data integrity, denormalization can improve 
query performance by reducing the number of joins required. However, 
denormalization should be used judiciously, as it can lead to data redundancy 
and potential update anomalies.

<br>
#### Example:

Continuing with our e-commerce database, suppose we frequently need to retrieve 
the total amount spent by each customer. Instead of calculating this on the fly, 
we can denormalize the schema by adding a total_spent column to the customers table.

<br>
{% highlight sql %}

-- Add total_spent column to the Customers Table
ALTER TABLE customers ADD COLUMN total_spent DECIMAL(10, 2);

{% endhighlight %}

<br>
Now, whenever a new order is placed, we can update the total_spent column for 
the corresponding customer. This denormalized approach can significantly speed 
up queries that involve customer spending analysis.



<br>
### 3. Table Partitioning

Table partitioning is a technique used to divide a large table into smaller, 
more manageable pieces called partitions. This can greatly improve query 
performance, especially for time-series or large datasets.


<br>
#### Example:

Suppose we have a table to store sensor data, and the data is partitioned by the 
date.

<br>
{% highlight sql %}

-- Create a partitioned table for sensor data
CREATE TABLE sensor_data (
    sensor_id INT,
    reading_value FLOAT,
    reading_date DATE
) PARTITION BY RANGE (reading_date);

-- Create partitions for each month
CREATE TABLE sensor_data_january PARTITION OF sensor_data
    FOR VALUES FROM ('2023-01-01') TO ('2023-02-01');

CREATE TABLE sensor_data_february PARTITION OF sensor_data
    FOR VALUES FROM ('2023-02-01') TO ('2023-03-01');

-- ...and so on for each month

{% endhighlight %}


<br>
With table partitioning, queries that involve specific date ranges can be much 
faster, as the database can efficiently narrow down the search to relevant 
partitions.



<br>
### 4. Handling Large Datasets

When dealing with large datasets, optimizing storage and retrieval is crucial. 
Proper indexing, efficient query design, and using appropriate data types can 
make a significant difference.

<br>
### Example:

Let's consider a scenario where we have a table for logging user activities, and 
the table has millions of rows. We want to retrieve the latest activities for a 
specific user.

<br>
{% highlight sql %}

-- Create an index on the user_id and activity_date columns
CREATE INDEX idx_user_activity ON user_activities (user_id, activity_date DESC);

-- Retrieve the latest activities for a specific user
SELECT *
FROM user_activities
WHERE user_id = 123
ORDER BY activity_date DESC
LIMIT 10;

{% endhighlight %}

<br>
In this example, we create an index that covers both the 
<span class="badge">user_id</span> and <span class="badge">activity_date</span> 
columns. This index allows the database to efficiently narrow down the search 
based on the user ID and quickly retrieve the latest activities.



<br>
### Conclusion

Designing efficient and scalable database schemas in PostgreSQL requires a 
balance between normalization and denormalization, effective use of table 
partitioning, and careful consideration of best practices for handling large 
datasets. By following these best practices and understanding the unique 
requirements of your application, you can create a robust and high-performing 
database that meets the demands of your users while maintaining data integrity.



<br>
*Thanks for reading, see you in the next one!*
