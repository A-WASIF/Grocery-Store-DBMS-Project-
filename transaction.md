# Requirements :
    1. Auto commit is turned OFF.
    COMMAND : `set autocommit = 0;`

    2. Set isolation level of the session to be read-commit.
    COMMAND : `set session transaction isolation level read commited;`

# Conflicting Transactions

## Conflicting Transaction - 1
```
START TRANSACTION;

SELECT quantity
FROM PRODUCT
WHERE product_id = 2
FOR UPDATE;

UPDATE PRODUCT
SET quantity = quantity - 30
WHERE product_id = 2 AND quantity >= 30;

COMMIT;
```

This code is for updating the quantity of a particular product in product table when a product is buyed by any customer.

This code will make sure that the row is locked corresponding to the row where product_id is same as given. So, even if another customer tries to buy the product with same product_id it will block the transaction until the first one commits the transaction.

This is a conflicting transaction because if their are two customer at same time trying to buy same product in large quantity which they can see initially because no one has bought them yet but we can't provide to both because we have only enough to satisfy only one such demand. So, to prevent this we can lock that particular product for one customer(who came first) so that the other customer have to wait in queue until the first one has done buying.

## Conflicting Transaction - 2
```
START TRANSACTION;

LOCK TABLES PRODUCT WRITE;

INSERT INTO PRODUCT (product_id, productName, description, price, quantity)
VALUES (14, 'Appricote', 'Famouse fresh fruit from Kashmir', 10.00, 50);

UNLOCK TABLES;

COMMIT;
```
When shops try to add the products in product tables.

We have to make sure that table is locked while adding product so that if any customer tries to buy in mean time he have to wait until the tables are unlocked.

This is conflicting transaction because imagine if a customer has come to buy a specific product which is not right now not present in our product table, but the shops just got stock for that product and they added it so now even though the product is added in table the customer won't be able to see that product. So, to stop this from happening we can lock the table while adding product so once the addition has happened the table will unlock and customer can see new products also.




# Non-Conflicting Transactions

## Non-Conflicting Transactions - 1
```
START TRANSACTION;

INSERT INTO DELIVERY_ASSISTANT (assistant_id, name, houseNumber, street, city, pincode, phoneNumber, email_ID, password)
VALUES (12, 'Holms', '123', 'Main Street', 'Cityville', '123456', '1234567890', 'holms@example.com', 'password123');

COMMIT;
```

Their should be no conflicts in adding a new join in Delivery Assistant.

## Non-Conflicting Transactions - 2
```
START TRANSACTION;

UPDATE ORDERS
SET order_status = 'Delivered'
WHERE order_id = 1;

COMMIT;
```
This transaction updates the status of an order to 'Delivered' without affecting other orders or transactions.


## Non-Conflicting Transactions - 3
```
START TRANSACTION;

SELECT *
FROM PAYMENT
WHERE customer_id = <customer_id_to_select>;

COMMIT;
```
This transaction is showing the history of a particular customer.\
It is a non-conflicting transaction because it's a read only statement which will not generate any issues.


## Non-Conflicting Transactions - 4
```
START TRANSACTION;

UPDATE CUSTOMER
SET phoneNumber = '5555555555'
WHERE customer_id = 1;

COMMIT;
```
This transaction updates the phone number of a customer without affecting other customers or transactions.
