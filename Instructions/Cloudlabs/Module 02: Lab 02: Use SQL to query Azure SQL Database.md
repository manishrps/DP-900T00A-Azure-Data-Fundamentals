## Exercise 1: Use SQL to query Azure SQL Database

Contoso has provisioned the SQL database and has imported all the inventory data into the data store. As lead developer, you've been asked to run some queries over the data.

In this exercise, you'll query the database to find how many products are in the database, and the number of items in stock for a particular product.

### Task 1: Connect to the query editor
---------------------------

You'll use the built-in Query editor in the Azure portal to connect to the database and query the data.

1.  Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com) using the same account you activated the sandbox with.

2.  In the portal, on the home page select SQL databases, and then select *Inventory* database located on the server you have just created.

    ![SQL Databases menu option on the home screen.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-select-sql-datbases.png)

3.  On the Overview page for your database, select Set server firewall.

    ![The Overview page for the SQL Database instance. The user has selected Set server firewall.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-server-firewall.png)

4.  On the Firewall settings page, select Add client IP, and then select Save.

    ![The Firewall settings page for the SQL Database instance. The user has selected Add client IP.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-set-client-ip.png)

5.  Close the Firewall settings page, and return to the Overview page for your database.

6.  On the Overview page, select Query editor (preview) in the left menu.

7.  Enter the username and password you recorded earlier when the setup script ran, and then select OK.

    ![The SQL Database sign-in page in the Azure portal.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-query-editor-login.png)

    You'll be presented with a screen similar to this example:

    ![The SQL Database Query Editor.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-simple-ui-query.png)

```
Tip: Adding your client IP in this step will not account for any existing VPN connections. If you can't complete step 7, disable any VPN connections or add the additional IP address manually from any errors displayed.
```


### Task 2: Run queries against the database
--------------------------------

1.  Copy the following SQL statement into the editor. Select Run, to check everything is working. You should see a list of four inventory items

    SQLCopy

    ```
    SELECT *
    FROM Inventory

    ```

    ![Run basic query in SQL Database Query Editor.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-run-basic-query.png)

2.  Replace the current SQL statement with the following statement to only show the number of bananas in stock:

    SQLCopy

    ```
    SELECT *
    FROM Inventory
    WHERE Name = 'banana'

    ```

    There should be 150 bananas.

    ![Run a WHERE query in SQL Database Query Editor.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-select-where-sql-databases.png)

3.  Replace the SQL statement with the following statement to retrieve the inventory items in order of the quantity in stock:

    SQLCopy

    ```
    SELECT *
    FROM Inventory
    ORDER BY Stock

    ```

    ![Run an ORDER query in SQL Database Query Editor.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-select-order-sql-databases.png)

4.  Replace the SQL statement with the statement shown below. This statement is a query that uses the JOIN operator to combine data from the *CustomerOrder* table and the *Inventory* table. It lists the details of orders placed by customers together with the inventory information for each item ordered:

    SQLCopy

    ```
    SELECT *
    FROM Inventory
    JOIN CustomerOrder ON Inventory.Id = CustomerOrder.InventoryId

    ```

    ![Run a JOIN query in SQL Database Query Editor.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/query-relational-data/media/6-select-join-sql-databases.png)

5.  Change the query to find the names of all customers who have ordered oranges.

    SQLCopy

    ```
    SELECT CustomerOrder.CustomerName
    FROM CustomerOrder
    JOIN Inventory ON CustomerOrder.InventoryId = Inventory.ID
    AND Inventory.Name = 'orange'

    ```

    This query should return two customers: John Smith and Jane Brown

6.  Find out how many customers have ordered lemons. This query uses the COUNT(*) function, which returns the number of rows that match the query criteria.

    SQLCopy

    ```
    SELECT COUNT(*)
    FROM CustomerOrder
    JOIN Inventory ON CustomerOrder.InventoryId = Inventory.ID
    AND Inventory.Name = 'lemon'

    ```

    The results of this query should indicate that only one customer has ordered lemons.

7.  Which fruits has John Smith ordered?

    SQLCopy

    ```
    SELECT Inventory.Name
    FROM CustomerOrder
    JOIN Inventory ON CustomerOrder.InventoryId = Inventory.ID
    AND CustomerOrder.CustomerName = 'John Smith'

    ```

    The results of this query should show that John Smith has only ordered oranges.

8.  What is the total quantity of items ordered by all customers? The *Quantity* column in the *CustomerOrder* table contains the quantity for each order. This query uses the SUM aggregate function to add the quantities together to product a grand total:

    SQLCopy

    ```
    SELECT SUM(CustomerOrder.Quantity)
    FROM CustomerOrder

    ```

    The answer should be 29.

You've now seen how to run SQL queries against a SQL database. If you have time, try to add some more rows into both tables using INSERT statements, modify the rows using UPDATE statements, and remove rows using DELETE statements.
