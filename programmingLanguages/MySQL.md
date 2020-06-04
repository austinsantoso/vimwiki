# Basic MySQL Syntax

# Listing all tables from database
To show the databases available
```sql
SHOW DATABASES
```

To show the tables present in a database
```sql
SHOW TABLES FROM <database>
```

To describe a table, show the column properties
```sql
DESCRIBE <database>.<table>
```

# Inserting Data into a Table
```sql
INSERT INTO <databaseName>.<tableName> (column1, column2...) VALUES (value1, value2...)
```

# Reading Data from a Table
To read a single column
```sql
SELECT <column> FROM <databaseName>.<tableName> {WHERE <condition>}
```

# Updating Data in a Table

# Deleting Data from a Table
To delete a particular row
```sql
DELETE FROM <databaseName>.<tableName> {WHERE <condition>}
```

To Delete the entire table
```sql
DROP TABLE <tableName>
```

To delete the data insie the table, but not the table itself
```sql
TRUNCATE TABLE <tableName>
```
