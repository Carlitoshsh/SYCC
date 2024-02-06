Create a simple database schema for a drugstore with two tables, one for publishers and the other for medicines:

Table 1: Publishers

- publisher_id: Unique identifier for each publisher.
- publisher_name: Name of the publisher.
- publisher_address: Address of the publisher.

Table 2: Medicines

- medicine_id: Unique identifier for each medicine.
- medicine_name: Name of the medicine.
- publisher_id: Foreign key referencing the publisher who published the medicine.
- medicine_price: Price of the medicine.
- medicine_quantity: Quantity of the medicine in stock.

, here are some sample data for the two tables:

Table 1: Publishers

Table 1: Publishers

| publisher_id | publisher_name    | publisher_address                              |
|--------------|------------------|------------------------------------------------|
| 1            | Pfizer           | 235 E 42nd St, New York, NY 10017, United States |
| 2            | Johnson & Johnson | 1 Johnson And Johnson Plz, New Brunswick, NJ 08933, United States |
| 3            | Novartis         | Lichtstrasse 35, 4056 Basel, Switzerland        |

Table 2: Medicines

| medicine_id | medicine_name | publisher_id | medicine_price | medicine_quantity |
|-------------|---------------|--------------|----------------|-------------------|
| 1           | Aspirin       | 1            | $5.99          | 1000              |
| 2           | Tylenol       | 2            | $7.99          | 500               |
| 3           | Advil         | 2            | $6.99          | 750               |
| 4           | Benadryl      | 1            | $8.99          | 250               |
| 5           | Zyrtec        | 3            | $9.99          | 100               |

Retrieve All Publishers:

Write an SQL query to retrieve all records from the Publishers table.
Find Medicines by Publisher Name:

Given a publisher name (e.g., “ABC Publishers”), find all medicines published by that publisher.
Calculate Total Medicine Price:

Calculate the total price of all medicines in stock.
List Publishers with No Medicines:

Identify publishers who have not published any medicines.
Find Expensive Medicines:

Retrieve medicines with a price greater than a specified value (e.g., $100).
Join Publishers and Medicines:

Write an SQL query to join the Publishers and Medicines tables, displaying the publisher name, medicine name, and quantity.
Count Medicines per Publisher:

Count the number of medicines published by each publisher.
Update Medicine Quantity:

Update the quantity of a specific medicine (e.g., increase the quantity of “Medicine X” by 50).
Delete Publishers with No Medicines:

Delete publishers who have not published any medicines.
Find Publishers with High-Priced Medicines:

Retrieve publishers who have at least one medicine with a price greater than $500.
