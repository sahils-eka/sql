# Learning SQL, My way…

Created: October 16, 2022 12:03 PM
Tags: Learning, SQL

## **[`SQL - From TrendyTech`](https://www.youtube.com/playlist?list=PLtgiThe4j67rAoPmnCQmcgLS4iIc5ungg)**

## Basic SQL

### **Session 1**

- **CREATE**-ing a Table
    
    ```sql
    CREATE TABLE employee (
    -> firstname varchar(20) NOT NULL,
    -> middlename varchar(20),
    -> lastname varchar(20) NOT NULL,
    -> age int NOT NULL,
    -> salary int NOT NULL,
    -> location varchar(20) NOT NULL
    -> );
    ```
    
    **************************NOT NULL************************** will make sure that these fields cannot be never empty, there must be some value.
    
- **DROP**-ing a Table
    
    ```sql
    DROP TABLE employee;
    ```
    
- **DESCRIBE**
    
     Gives table definition.
    
    ```sql
    mysql> DESCRIBE employee;
    +------------+-------------+------+-----+-----------+-------+
    | Field      | Type        | Null | Key | Default   | Extra |
    +------------+-------------+------+-----+-----------+-------+
    | firstname  | varchar(20) | NO   |     | NULL      |       |
    | middlename | varchar(20) | YES  |     | NULL      |       |
    | lastname   | varchar(20) | NO   |     | NULL      |       |
    | age        | int(11)     | NO   |     | NULL      |       |
    | salary     | int(11)     | NO   |     | NULL      |       |
    | location   | varchar(20) | NO   |     | NULL      |       |
    +------------+-------------+------+-----+-----------+-------+
    ```
    
    Notice that for `middlename` the `Null` in the above table is marked as ************YES,************ for rest it’s NO.
    

### Session 2

- **INSERT command**
    
    ```sql
    INSERT INTO **employee (firstname, middlename, lastname, age, salary, location)** 
    VALUES ('satish', 'kumar', 'sharma', 28, 10000, 'bangalore');
    ```
    
    - **Best Practice:** Always give column names after providing the `Table Name`
    - Inserting ******************multiple rows in one go******************
        
        ```sql
        INSERT INTO employee (firstname, lastname, age, salary, location) 
        **VALUES ('flower', 'vans', 30, 20000, 'goa'),('sahil', 'singh', 24, 50000, 'pune');**
        ```
        
- **`\` Escaping characters**
    - Inserting data
        
        ```sql
        INSERT INTO employee (firstname, lastname, age, salary, location) 
        VALUES (**'D\'siuuza'**, 'sharma', 32, 30000, 'goa');
        ```
        
    - Searching data
        
        ```sql
        SELECT * FROM employee **WHERE firstname='D\'siuuza';**
        ```
        
- **DEFAULT** constraint
    
    ```sql
    CREATE TABLE employee (
    -> firstname varchar(20) NOT NULL,
    -> middlename varchar(20),
    -> lastname varchar(20) NOT NULL,
    -> age int NOT NULL,
    -> salary int NOT NULL,
    -> location varchar(20) NOT NULL **DEFAULT 'bangalore'**
    -> );
    ```
    
    ```sql
    **mysql> DESC employee;
    +------------+-------------+------+-----+-----------+-------+
    | Field      | Type        | Null | Key | Default   | Extra |
    +------------+-------------+------+-----+-----------+-------+
    | firstname  | varchar(20) | NO   |     | NULL      |       |
    | middlename | varchar(20) | YES  |     | NULL      |       |
    | lastname   | varchar(20) | NO   |     | NULL      |       |
    | age        | int(11)     | NO   |     | NULL      |       |
    | salary     | int(11)     | NO   |     | NULL      |       |
    | location   | varchar(20) | NO   |     | bangalore |       |
    +------------+-------------+------+-----+-----------+-------+**
    ```
    
    Now, don’t provide the location. By default it will be set to ‘bangalore’
    
    ```sql
    ****************mysql> INSERT INTO employee (firstname, lastname, age, salary) 
    VALUES ('sahil', 'singh', 28, 50000);****************
    ```
    
    ```sql
    **mysql> SELECT * FROM employee;**
    +-----------+------------+----------+-----+--------+-----------+
    | firstname | middlename | lastname | age | salary | location  |
    +-----------+------------+----------+-----+--------+-----------+
    | satish    | kumar      | sharma   |  28 |  10000 | bangalore |
    | sahil     | NULL       | singh    |  28 |  50000 | bangalore |
    +-----------+------------+----------+-----+--------+-----------+
    ```
    

### Session 3

- ****************[PRIMARY](https://www.w3schools.com/sql/sql_primarykey.ASP)** key
    
    Helps to uniquely identify a record in a table. For this, let’s drop the existing table `employee` and create a new table with `primary` key constraint.
    
    ```sql
    CREATE TABLE employee (
    -> **id int PRIMARY KEY,**
    -> firstname varchar(20) NOT NULL,
    -> middlename varchar(20),
    -> lastname varchar(20) NOT NULL,
    -> age int NOT NULL,
    -> salary int NOT NULL,
    -> location varchar(20) NOT NULL DEFAULT 'bangalore'
    -> );
    ```
    
    ```sql
    **mysql> DESC employee;**
    +------------+-------------+------+-----+-----------+-------+
    | Field      | Type        | Null | Key | Default   | Extra |
    +------------+-------------+------+-----+-----------+-------+
    | id         | int(11)     | NO   | PRI | NULL      |       |
    | firstname  | varchar(20) | NO   |     | NULL      |       |
    | middlename | varchar(20) | YES  |     | NULL      |       |
    | lastname   | varchar(20) | NO   |     | NULL      |       |
    | age        | int(11)     | NO   |     | NULL      |       |
    | salary     | int(11)     | NO   |     | NULL      |       |
    | location   | varchar(20) | NO   |     | bangalore |       |
    +------------+-------------+------+-----+-----------+-------+
    ```
    
    What if we try inserting a record with same `id` again?
    
    ```sql
    mysql> INSERT INTO employee (id, firstname, lastname, age, salary) VALUES (1, 'eka', 'singh', 28, 50000);
    **ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'**
    ```
    
    [**SQL PRIMARY KEY Constraint**](https://www.w3schools.com/sql/sql_primarykey.ASP)
    
    The `PRIMARY KEY` constraint uniquely identifies each record in a table.
    
    Primary keys must contain UNIQUE values, and cannot contain NULL values.
    
    **A table can have only ONE primary key; and in the table, this primary key can consist of single or multiple columns (fields).**
    
    **************One more way to define PRIMARY KEY:**************
    
    ```sql
    CREATE TABLE employee (
    -> **id int,**
    -> firstname varchar(20) NOT NULL,
    -> middlename varchar(20),
    -> lastname varchar(20) NOT NULL,
    -> age int NOT NULL,
    -> salary int NOT NULL,
    -> location varchar(20) NOT NULL DEFAULT 'bangalore',
    -> **PRIMARY KEY (id)**
    -> );
    ```
    
- **Composite** **PRIMARY** key
    
    ```sql
    mysql> CREATE TABLE emp(
    -> firstname varchar(20),
    -> lastname varchar(20),
    -> age int NOT NULL,
    -> PRIMARY KEY(firstname, lastname)
    -> );
    ```
    
    Here, the `firstname` and the `lastname` are PRIMARY keys or a **Composite PRIMARY Key**. That means there is just **one PRIMARY** Key in the table `emp` which satisfies the definition of primary keys.
    
    Let’s say we have a record in this table
    
    ```sql
    mysql> SELECT * FROM emp;
    +-----------+----------+-----+
    | firstname | lastname | age |
    +-----------+----------+-----+
    | sahil     | singh    |  24 |
    +-----------+----------+-----+
    1 row in set (0.00 sec)
    ```
    
    add some more rows…
    
    ```sql
    mysql> INSERT INTO emp(firstname, lastname, age) **VALUES('sahil', 'kumar', 20)**;
    Query OK, 1 row affected (0.01 sec)
    
    mysql> INSERT INTO emp(firstname, lastname, age) **VALUES('eka', 'singh', 23)**;
    Query OK, 1 row affected (0.01 sec)
    ```
    
    Notice that even if the `firstname` and `lastname` of these two records are the same as the existing record in the table, the INSERT query worked but, **********WHY?**********
    
    Reason is because `firstname` and `lastname` together makes a Primary Key called ********************************************Composite PRIMARY KEY,******************************************** which means it will fail if we try to do the below:
    
    ```sql
    mysql> INSERT INTO emp(firstname, lastname, age) VALUES('sahil', 'singh', 19);
    **ERROR 1062 (23000): Duplicate entry 'sahil-singh' for key 'PRIMARY'**
    ```
    
    The error is self-explanatory.
    
    ```sql
    mysql> SELECT * FROM emp;
    +-----------+----------+-----+
    | firstname | lastname | age |
    +-----------+----------+-----+
    | eka       | singh    |  23 |
    | sahil     | kumar    |  20 |
    | sahil     | singh    |  24 |
    +-----------+----------+-----+
    ```
    
- ****************************AUTO_INCREMENT****************************
    
    ```sql
    CREATE TABLE employee (
    -> **id int AUTO_INCREMENT,**
    -> firstname varchar(20) NOT NULL,
    -> middlename varchar(20),
    -> lastname varchar(20) NOT NULL,
    -> age int NOT NULL,
    -> salary int NOT NULL,
    -> location varchar(20) NOT NULL DEFAULT 'bangalore',
    -> **PRIMARY KEY (id)**
    -> );
    ```
    
    ```sql
    mysql> DESC employee;
    +------------+-------------+------+-----+-----------+----------------+
    | Field      | Type        | Null | Key | Default   | Extra          |
    +------------+-------------+------+-----+-----------+----------------+
    | id         | int(11)     | NO   | PRI | NULL      | auto_increment |
    | firstname  | varchar(20) | NO   |     | NULL      |                |
    | middlename | varchar(20) | YES  |     | NULL      |                |
    | lastname   | varchar(20) | NO   |     | NULL      |                |
    | age        | int(11)     | NO   |     | NULL      |                |
    | salary     | int(11)     | NO   |     | NULL      |                |
    | location   | varchar(20) | NO   |     | bangalore |                |
    +------------+-------------+------+-----+-----------+----------------+
    ```
    
    In the table `Desription` we can see that under `Extra` column **auto_increment** is added. Now, we don’t have to worry about the `id` column. 
    
    We can ignore this column by not explicitly inserting its values, the **auto_increment** will take care of that.
    
    ```sql
    **mysql> INSERT INTO employee (firstname, lastname , age, salary) VALUES ('sahil', 'singh', 28, 50000);**
    Query OK, 1 row affected (0.01 sec)
    
    **mysql> SELECT * FROM employee;**
    +----+-----------+------------+----------+-----+--------+-----------+
    | id | firstname | middlename | lastname | age | salary | location  |
    +----+-----------+------------+----------+-----+--------+-----------+
    |  1 | sahil     | NULL       | singh    |  28 |  50000 | bangalore |
    +----+-----------+------------+----------+-----+--------+-----------+
    1 row in set (0.00 sec)
    ```
    
    ```sql
    **mysql> INSERT INTO employee (firstname, lastname, age, salary, location) 
    VALUES ('eka', 'singh', 24, 60000, 'pune'), 
    ('rajesh', 'kumar', 32, 20000, 'gandhinagar');**
    
    Query OK, 2 rows affected (0.00 sec)
    Records: 2  Duplicates: 0  Warnings: 0
    
    **mysql> SELECT * FROM employee;**
    +----+-----------+------------+----------+-----+--------+-------------+
    | id | firstname | middlename | lastname | age | salary | location    |
    +----+-----------+------------+----------+-----+--------+-------------+
    |  1 | sahil     | NULL       | singh    |  28 |  50000 | bangalore   |
    |  2 | eka       | NULL       | singh    |  24 |  60000 | pune        |
    |  3 | rajesh    | NULL       | kumar    |  32 |  20000 | gandhinagar |
    +----+-----------+------------+----------+-----+--------+-------------+
    3 rows in set (0.00 sec)
    ```
    
    <aside>
    💡 What if we try to delete the Column which has `auto_increment` ?
    
    </aside>
    
    ```sql
    mysql> ALTER TABLE employee DROP PRIMARY KEY;
    **ERROR 1075 (42000): Incorrect table definition; there can be only one auto column 
    and it must be defined as a key**
    ```
    
- ********************UNIQUE KEY********************
    - UNIQUE key’s purpose is to avoid duplication, duplication of values.
    - The UNIQUE key can hold NULL values, while the PRIMARY key cannot.
    - We can have only one PRIMARY KEY(or composite primary key) but multiple UNIQUE keys in a table.
    
    ```sql
    mysql> CREATE TABLE emp(
    -> id int UNIQUE KEY,
    -> firstname varchar(20),
    -> lastname varchar(20),
    -> age int NOT NULL);
    ```
    
    ```sql
    mysql> DESCRIBE emp;
    +-----------+-------------+------+-----+---------+-------+
    | Field     | Type        | Null | Key | Default | Extra |
    +-----------+-------------+------+-----+---------+-------+
    | id        | int(11)     | YES  | UNI | NULL    |       |
    | firstname | varchar(20) | YES  |     | NULL    |       |
    | lastname  | varchar(20) | YES  |     | NULL    |       |
    | age       | int(11)     | NO   |     | NULL    |       |
    +-----------+-------------+------+-----+---------+-------+
    ```
    
    The `Null` column, in this case, is set to ********YES******** for the field `id` ****************but the same field was set to ******NO****** in case of the `Primary` key.
    
    ```sql
    mysql> INSERT INTO emp(firstname, lastname, age) VALUES('eka', 'singh', 19);
    Query OK, 1 row affected (0.01 sec)
    
    mysql> SELECT * FROM emp;
    +------+-----------+----------+-----+
    | id   | firstname | lastname | age |
    +------+-----------+----------+-----+
    |    1 | sahil     | singh    |  19 |
    | NULL | eka       | singh    |  19 |
    +------+-----------+----------+-----+
    2 rows in set (0.00 sec)
    
    **-- Similarly, as per the definition, we can have multiple NULL ids(UNIQUE) in 
    -- the table.**
    
    mysql> INSERT INTO emp(firstname, lastname, age) VALUES('raj', 'singh', 20);
    Query OK, 1 row affected (0.01 sec)
    
    mysql> SELECT * FROM emp;
    +------+-----------+----------+-----+
    | id   | firstname | lastname | age |
    +------+-----------+----------+-----+
    |    1 | sahil     | singh    |  19 |
    | NULL | eka       | singh    |  19 |
    | NULL | raj       | singh    |  20 |
    +------+-----------+----------+-----+
    ```
    
    Now let’s add a record with `id` = ******1.******
    
    ```sql
    mysql> INSERT INTO emp(id, firstname, lastname, age) VALUES(1, 'Veronica', 'singh', 20);
    **ERROR 1062 (23000): Duplicate entry '1' for key 'id'**
    ```
    
- **Composite UNIQUE KEY** or **MUL**TIPLE KEY
    
    If we give more than two UNIQUE Keys in a table definition, they would work independently, see below:
    
    ```sql
    mysql> CREATE TABLE imp(**id int UNIQUE KEY**, name varchar(20), **age int UNIQUE KEY**);
    ```
    
    ```sql
    mysql> DESCRIBE imp;
    +-------+-------------+------+-----+---------+-------+
    | Field | Type        | Null | Key | Default | Extra |
    +-------+-------------+------+-----+---------+-------+
    | id    | int(11)     | YES  | UNI | NULL    |       |
    | name  | varchar(20) | YES  |     | NULL    |       |
    | age   | int(11)     | YES  | UNI | NULL    |       |
    +-------+-------------+------+-----+---------+-------+
    ```
    
    But if we give UNIQUE Keys in the below fashion, they are considered as Single UNIQUE KEY, concatenation of both the keys, a **********MUL key.**********
    
    ```sql
    CREATE TABLE emp(id int, firstname varchar(20), middlename varchar(20), 
    lastname varchar(20),age int NOT NULL, **UNIQUE KEY(id, middlename)**);
    ```
    
    ```sql
    mysql> DESCRIBE emp;
    +------------+-------------+------+-----+---------+-------+
    | Field      | Type        | Null | Key | Default | Extra |
    +------------+-------------+------+-----+---------+-------+
    | **id         | int(11)     | YES  | MUL** | NULL    |       |
    | firstname  | varchar(20) | YES  |     | NULL    |       |
    | middlename | varchar(20) | YES  |     | NULL    |       |
    | lastname   | varchar(20) | YES  |     | NULL    |       |
    | age        | int(11)     | NO   |     | NULL    |       |
    +------------+-------------+------+-----+---------+-------+
    ```
    
    The above key will work in the similar fashion as the Composite PRIMARY Key works.
    

### Session 4

- **WHERE**
    
    ```sql
    mysql> SELECT * FROM employee WHERE age > 24;
    +----+-----------+------------+----------+-----+--------+-------------+
    | id | firstname | middlename | lastname | age | salary | location    |
    +----+-----------+------------+----------+-----+--------+-------------+
    |  1 | sahil     | NULL       | singh    |  28 |  50000 | bangalore   |
    |  3 | rajesh    | NULL       | kumar    |  32 |  20000 | gandhinagar |
    |  4 | Raj       | NULL       | gupta    |  41 |   2000 | bangalore   |
    |  5 | raj       | NULL       | gupta    |  31 |   1000 | bangalore   |
    +----+-----------+------------+----------+-----+--------+-------------+
    ```
    
    ```sql
    mysql> **SELECT * FROM employee WHERE firstname = 'Raj';**
    +----+-----------+------------+----------+-----+--------+-----------+
    | id | firstname | middlename | lastname | age | salary | location  |
    +----+-----------+------------+----------+-----+--------+-----------+
    |  4 | Raj       | NULL       | gupta    |  41 |   2000 | bangalore |
    |  5 | raj       | NULL       | gupta    |  31 |   1000 | bangalore |
    +----+-----------+------------+----------+-----+--------+-----------+
    2 rows in set (0.00 sec)
    ```
    
    Noticed one thing? the above statement gave us two records, even though we provided `firstname=**'Raj'`** and not `raj`. 
    
    This is because the `WHERE` is **CASE INSENSITIVE,** it doesn’t diff between `Raj` and `raj`.
    
    To make it CASE SENSITIVE or **to match the exact case**, use `binary`. See below.
    
    ```sql
    mysql> SELECT * FROM employee WHERE **BINARY** firstname = 'Raj';
    +----+-----------+------------+----------+-----+--------+-----------+
    | id | firstname | middlename | lastname | age | salary | location  |
    +----+-----------+------------+----------+-----+--------+-----------+
    |  4 | Raj       | NULL       | gupta    |  41 |   2000 | bangalore |
    +----+-----------+------------+----------+-----+--------+-----------+
    1 row in set (0.00 sec)
    ```
    
- ************UPDATE************
    
    **** Always do the `SELECT` operation with the `WHERE` clause before the UPDATE operation, to make sure that you update only the required records.**
    
    ```sql
    mysql> **UPDATE employee SET firstname='rudra' WHERE BINARY firstname='Raj';**
    
    Query OK, 1 row affected (0.00 sec)
    Rows matched: 1  Changed: 1  Warnings: 0
    ```
    
    ```sql
    mysql> SELECT * FROM employee;
    +----+-----------+------------+----------+-----+--------+-------------+
    | id | firstname | middlename | lastname | age | salary | location    |
    +----+-----------+------------+----------+-----+--------+-------------+
    |  1 | sahil     | NULL       | singh    |  28 |  50000 | bangalore   |
    |  2 | eka       | NULL       | singh    |  24 |  60000 | pune        |
    |  3 | rajesh    | NULL       | kumar    |  32 |  20000 | gandhinagar |
    |  4 | rudra     | NULL       | gupta    |  41 |   2000 | bangalore   |
    |  5 | raj       | NULL       | gupta    |  31 |   1000 | bangalore   |
    +----+-----------+------------+----------+-----+--------+-------------+
    5 rows in set (0.00 sec)
    ```
    
    ```sql
    mysql> UPDATE employee SET location='pune' WHERE firstname='sahil';
    
    +----+-----------+------------+----------+-----+--------+----------+
    | id | firstname | middlename | lastname | age | salary | location |
    +----+-----------+------------+----------+-----+--------+----------+
    |  1 | sahil     | NULL       | singh    |  28 |  50000 | pune     |
    +----+-----------+------------+----------+-----+--------+----------+
    ```
    
    What if we don’t provide the WHERE clause during UPDATE?
    
    ```sql
    mysql> SELECT * from imp;
    +------+-------+------+
    | id   | name  | age  |
    +------+-------+------+
    |    1 | raj   |   31 |
    |    2 | sahil |   29 |
    |    3 | eka   |   26 |
    +------+-------+------+
    ```
    
    ```sql
    mysql> UPDATE imp SET name='sahil';
    
    **Query OK, 2 rows affected (0.01 sec)
    Rows matched: 3  Changed: 2  Warnings: 0**
    ```
    
    The query worked and it updated two records, but why only two? because for one of the record the `name` was already `sahil`.
    
    ```sql
    mysql> SELECT * from imp;
    +------+-------+------+
    | id   | name  | age  |
    +------+-------+------+
    |    1 | sahil |   31 |
    |    2 | sahil |   29 |
    |    3 | sahil |   26 |
    +------+-------+------+
    ```
    
- **DELETE**
    
    **** Always do the `SELECT` operation with the `WHERE` clause before the DELETE operation, to make sure that you delete** **only the required records.**
    
    ```sql
    mysql> **DELETE FROM imp WHERE id=3**;
    Query OK, 1 row affected (0.02 sec)
    ```
    
    ```sql
    mysql> SELECT * FROM imp;
    +------+-------+------+
    | id   | name  | age  |
    +------+-------+------+
    |    1 | sahil |   31 |
    |    2 | sahil |   29 |
    +------+-------+------+
    2 rows in set (0.00 sec)
    ```
    
- **********ALTER**********
    
    Used to update the ************************Structure************************ or ************************************Schema of a table.************************************
    
    ```sql
    mysql> DESC employee;
    +------------+-------------+------+-----+-----------+----------------+
    | Field      | Type        | Null | Key | Default   | Extra          |
    +------------+-------------+------+-----+-----------+----------------+
    | id         | int(11)     | NO   | PRI | NULL      | auto_increment |
    | firstname  | varchar(20) | NO   |     | NULL      |                |
    | middlename | varchar(20) | YES  |     | NULL      |                |
    | lastname   | varchar(20) | NO   |     | NULL      |                |
    | age        | int(11)     | NO   |     | NULL      |                |
    | salary     | int(11)     | NO   |     | NULL      |                |
    | location   | varchar(20) | NO   |     | bangalore |                |
    +------------+-------------+------+-----+-----------+----------------+
    7 rows in set (0.00 sec)
    ```
    
    - **MODIFY** an existing column
        
        ```sql
        ALTER TABLE employee MODIFY firstname varchar(30);
        
        DESCRIBE employee;
        +------------+-------------+------+-----+-----------+----------------+
        | Field      | Type        | Null | Key | Default   | Extra          |
        +------------+-------------+------+-----+-----------+----------------+
        | id         | int(11)     | NO   | PRI | NULL      | auto_increment |
        | firstname  | varchar(30) | YES  |     | NULL      |                |
        | middlename | varchar(20) | YES  |     | NULL      |                |
        | lastname   | varchar(20) | NO   |     | NULL      |                |
        | age        | int(11)     | NO   |     | NULL      |                |
        | salary     | int(11)     | NO   |     | NULL      |                |
        | location   | varchar(20) | NO   |     | bangalore |                |
        +------------+-------------+------+-----+-----------+----------------+
        7 rows in set (0.00 sec)
        ```
        
        ```sql
        DESC emp;
        +-----------+-------------+------+-----+---------+----------------+
        | Field     | Type        | Null | Key | Default | Extra          |
        +-----------+-------------+------+-----+---------+----------------+
        | empid     | int(11)     | NO   | PRI | NULL    | auto_increment |
        | firstname | varchar(20) | YES  |     | NULL    |                |
        | lastname  | varchar(20) | YES  |     | NULL    |                |
        | age       | int(11)     | NO   |     | NULL    |                |
        | location  | varchar(20) | YES  |     | NULL    |                |
        +-----------+-------------+------+-----+---------+----------------+
        
        mysql> ALTER TABLE emp MODIFY location varchar(20) DEFAULT 'bangalore';
        Query OK, 0 rows affected (0.01 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        mysql> DESC emp;
        +-----------+-------------+------+-----+-----------+----------------+
        | Field     | Type        | Null | Key | Default   | Extra          |
        +-----------+-------------+------+-----+-----------+----------------+
        | empid     | int(11)     | NO   | PRI | NULL      | auto_increment |
        | firstname | varchar(20) | YES  |     | NULL      |                |
        | lastname  | varchar(20) | YES  |     | NULL      |                |
        | age       | int(11)     | NO   |     | NULL      |                |
        | location  | varchar(20) | YES  |     | bangalore |                |
        +-----------+-------------+------+-----+-----------+----------------+
        ```
        
    - [**CHANGE v/s MODIFY Column**](https://stackoverflow.com/questions/14767174/modify-column-vs-change-column)
        
        (Click on the link above for a detailed explanation)
        
        Both are similar, with just slight differences.
        
        `Change` is used to change column name (we can also give the same name) in a table, and also modify the column properties.
        
        `Modify` can do everything that the `Change` command can do, but it cannot change the column.
        
        ```sql
        mysql> desc emp;
        +-----------+-------------+------+-----+---------+-------+
        | Field     | Type        | Null | Key | Default | Extra |
        +-----------+-------------+------+-----+---------+-------+
        | id        | int(11)     | NO   | PRI | NULL    |       |
        | firstname | varchar(20) | YES  |     | NULL    |       |
        | lastname  | varchar(20) | YES  |     | NULL    |       |
        | age       | int(11)     | NO   |     | NULL    |       |
        | location  | varchar(20) | YES  |     | NULL    |       |
        +-----------+-------------+------+-----+---------+-------+
        5 rows in set (0.00 sec)
        ```
        
        `MODIFY`
        
        ```sql
        mysql> ALTER TABLE emp **MODIFY** id int AUTO_INCREMENT;
        Query OK, 0 rows affected (0.14 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        mysql> DESC emp;
        +-----------+-------------+------+-----+---------+----------------+
        | Field     | Type        | Null | Key | Default | Extra          |
        +-----------+-------------+------+-----+---------+----------------+
        | id        | int(11)     | NO   | PRI | NULL    | auto_increment |
        | firstname | varchar(20) | YES  |     | NULL    |                |
        | lastname  | varchar(20) | YES  |     | NULL    |                |
        | age       | int(11)     | NO   |     | NULL    |                |
        | location  | varchar(20) | YES  |     | NULL    |                |
        +-----------+-------------+------+-----+---------+----------------+
        ```
        
        `CHANGE`
        
        ```sql
        mysql> ALTER TABLE emp **CHANGE** id empid int AUTO_INCREMENT;
        Query OK, 0 rows affected (0.03 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        mysql> DESC emp;
        +-----------+-------------+------+-----+---------+----------------+
        | Field     | Type        | Null | Key | Default | Extra          |
        +-----------+-------------+------+-----+---------+----------------+
        | empid     | int(11)     | NO   | PRI | NULL    | auto_increment |
        | firstname | varchar(20) | YES  |     | NULL    |                |
        | lastname  | varchar(20) | YES  |     | NULL    |                |
        | age       | int(11)     | NO   |     | NULL    |                |
        | location  | varchar(20) | YES  |     | NULL    |                |
        +-----------+-------------+------+-----+---------+----------------+
        ```
        
    - **ADD** a new column
        
        ```sql
        mysql> ALTER TABLE employee ADD email varchar(20);
        Query OK, 0 rows affected (0.16 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        mysql> DESC employee;
        +------------+-------------+------+-----+-----------+----------------+
        | Field      | Type        | Null | Key | Default   | Extra          |
        +------------+-------------+------+-----+-----------+----------------+
        | id         | int(11)     | NO   | PRI | NULL      | auto_increment |
        | firstname  | varchar(30) | YES  |     | NULL      |                |
        | middlename | varchar(20) | YES  |     | NULL      |                |
        | lastname   | varchar(20) | NO   |     | NULL      |                |
        | age        | int(11)     | NO   |     | NULL      |                |
        | salary     | int(11)     | NO   |     | NULL      |                |
        | location   | varchar(20) | NO   |     | bangalore |                |
        | email      | varchar(20) | YES  |     | NULL      |                |
        +------------+-------------+------+-----+-----------+----------------+
        8 rows in set (0.00 sec)
        ```
        
    - **DROP**(delete) a column
        
        ```sql
        mysql> ALTER TABLE employee DROP email;
        Query OK, 0 rows affected (0.14 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        mysql> DESC employee;
        +------------+-------------+------+-----+-----------+----------------+
        | Field      | Type        | Null | Key | Default   | Extra          |
        +------------+-------------+------+-----+-----------+----------------+
        | id         | int(11)     | NO   | PRI | NULL      | auto_increment |
        | firstname  | varchar(30) | YES  |     | NULL      |                |
        | middlename | varchar(20) | YES  |     | NULL      |                |
        | lastname   | varchar(20) | NO   |     | NULL      |                |
        | age        | int(11)     | NO   |     | NULL      |                |
        | salary     | int(11)     | NO   |     | NULL      |                |
        | location   | varchar(20) | NO   |     | bangalore |                |
        +------------+-------------+------+-----+-----------+----------------+
        7 rows in set (0.00 sec)
        ```
        
    - DROP a PRIMARY KEY **(just the key, not the column)**
        
        ```sql
        mysql> DESC emp;
        +-----------+-------------+------+-----+---------+-------+
        | Field     | Type        | Null | Key | Default | Extra |
        +-----------+-------------+------+-----+---------+-------+
        | id        | int(11)     | NO   | PRI | NULL    |       |
        | firstname | varchar(20) | YES  |     | NULL    |       |
        | lastname  | varchar(20) | YES  |     | NULL    |       |
        | age       | int(11)     | YES  |     | NULL    |       |
        | location  | varchar(20) | YES  |     | NULL    |       |
        +-----------+-------------+------+-----+---------+-------+
        5 rows in set (0.01 sec)
        
        mysql> ALTER TABLE emp DROP PRIMARY KEY;
        Query OK, 0 rows affected (0.20 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        mysql> DESC emp;
        +-----------+-------------+------+-----+---------+-------+
        | Field     | Type        | Null | Key | Default | Extra |
        +-----------+-------------+------+-----+---------+-------+
        | id        | int(11)     | NO   |     | NULL    |       |
        | firstname | varchar(20) | YES  |     | NULL    |       |
        | lastname  | varchar(20) | YES  |     | NULL    |       |
        | age       | int(11)     | YES  |     | NULL    |       |
        | location  | varchar(20) | YES  |     | NULL    |       |
        +-----------+-------------+------+-----+---------+-------+
        5 rows in set (0.00 sec)
        ```
        
        Here, just the PRIMARY KEY property got deleted…not the column.
        
    - ADD PRIMARY KEY
        
        To demonstrate this, let’s make `age` the PRIMARY key.
        
        ```sql
        mysql> ALTER TABLE emp ADD PRIMARY KEY(age);
        Query OK, 0 rows affected (0.12 sec)
        Records: 0  Duplicates: 0  Warnings: 0
        
        mysql> DESC emp;
        +-----------+-------------+------+-----+---------+-------+
        | Field     | Type        | Null | Key | Default | Extra |
        +-----------+-------------+------+-----+---------+-------+
        | id        | int(11)     | NO   |     | NULL    |       |
        | firstname | varchar(20) | YES  |     | NULL    |       |
        | lastname  | varchar(20) | YES  |     | NULL    |       |
        | age       | int(11)     | NO   | PRI | NULL    |       |
        | location  | varchar(20) | YES  |     | NULL    |       |
        +-----------+-------------+------+-----+---------+-------+
        ```
        
- ******************DELETE v/s TRUNCATE******************
    
    DELETE is a **DML** command, it deals with the data inside a Table and not the Schema of the table.
    
    Whereas, TRUNCATE is a ******DDL****** command. Internally, TRUNCATE ****************************************************************drops the table and recreates it.****************************************************************
    
    The `[TRUNCATE TABLE](https://www.w3schools.com/sql/sql_drop_table.asp)`statement is used to delete the data inside a table, but not the table itself.
    
    ```sql
    -- This will delete all the records in the table `emp`. 
    -- The schema will be intact.
    DELETE FROM emp;
    
    -- TRUNCATE will DROP the table, delete all the records
    -- And recreate the same table with same SCHEMA.
    TRUNCATE TABLE emp;
    ```
    
    We are given the below table, let’s try DELETE and TRUNCATE.
    
    ```sql
    mysql> SELECT * FROM emp;
    +-------+-----------+----------+-----+-----------+
    | empid | firstname | lastname | age | location  |
    +-------+-----------+----------+-----+-----------+
    |     1 | sahil     | singh    |  24 | bangalore |
    |     2 | eka       | singh    |  23 | bangalore |
    +-------+-----------+----------+-----+-----------+
    2 rows in set (0.00 sec)
    ```
    
    ```sql
    **-- DELETE**
    mysql> DELETE FROM emp;
    Query OK, 2 rows affected (0.01 sec)
    
    mysql> DESC emp;
    +-----------+-------------+------+-----+-----------+----------------+
    | Field     | Type        | Null | Key | Default   | Extra          |
    +-----------+-------------+------+-----+-----------+----------------+
    | empid     | int(11)     | NO   | PRI | NULL      | auto_increment |
    | firstname | varchar(20) | YES  |     | NULL      |                |
    | lastname  | varchar(20) | YES  |     | NULL      |                |
    | age       | int(11)     | NO   |     | NULL      |                |
    | location  | varchar(20) | YES  |     | bangalore |                |
    +-----------+-------------+------+-----+-----------+----------------+
    5 rows in set (0.00 sec)
    
    mysql> SELECT * FROM emp;
    Empty set (0.00 sec)
    ```
    
    ```sql
    **--TRUNCATE**
    mysql> TRUNCATE TABLE emp;
    Query OK, 0 rows affected (0.09 sec)
    
    mysql> SELECT * FROM emp;
    Empty set (0.00 sec)
    
    mysql> DESC emp;
    +-----------+-------------+------+-----+-----------+----------------+
    | Field     | Type        | Null | Key | Default   | Extra          |
    +-----------+-------------+------+-----+-----------+----------------+
    | empid     | int(11)     | NO   | PRI | NULL      | auto_increment |
    | firstname | varchar(20) | YES  |     | NULL      |                |
    | lastname  | varchar(20) | YES  |     | NULL      |                |
    | age       | int(11)     | NO   |     | NULL      |                |
    | location  | varchar(20) | YES  |     | bangalore |                |
    +-----------+-------------+------+-----+-----------+----------------+
    5 rows in set (0.00 sec)
    ```
    
    Both commands generated the same result, so what to use when? TRUNCATE is a good option to choose if you’re dealing with the deletion of a large amount of data. TRUNCATE is faster than DELETE FROM.
    

### Session 5 **********************[FOREIGN KEY](https://www.w3schools.com/sql/sql_foreignkey.asp)**********************

We will understand the FOREIGN key concept with the help of the below 2 tables:

- students
- courses

```sql
**-- students table**
CREATE TABLE students(
student_id int AUTO_INCREMENT,
student_fname varchar(30) NOT NULL,
student_lname varchar(30) NOT NULL,
student_mname varchar(30), 
student_email varchar(40) NOT NULL,
student_phone varchar(15) NOT NULL, 
student_alternative_phone varchar(15), 
enrollment_date TIMESTAMP NOT NULL,
selected_course int NOT NULL DEFAULT 1,
years_of_exp int NOT NULL, 
student_company varchar(30), 
batch_date varchar(30) NOT NULL,
source_of_joining varchar(30) NOT NULL, 
location varchar(30) NOT NULL, 
PRIMARY KEY(student_id), 
UNIQUE KEY(student_email)
);
```

```sql
mysql> DESC students;
+---------------------------+-------------+------+-----+-------------------+-----------------------------+
| Field                     | Type        | Null | Key | Default           | Extra                       |
+---------------------------+-------------+------+-----+-------------------+-----------------------------+
| student_id                | int(11)     | NO   | PRI | NULL              | auto_increment              |
| student_fname             | varchar(30) | NO   |     | NULL              |                             |
| student_lname             | varchar(30) | NO   |     | NULL              |                             |
| student_mname             | varchar(30) | YES  |     | NULL              |                             |
| student_email             | varchar(40) | NO   | UNI | NULL              |                             |
| student_phone             | varchar(15) | NO   |     | NULL              |                             |
| student_alternative_phone | varchar(15) | YES  |     | NULL              |                             |
| enrollment_date           | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| selected_course           | int(11)     | NO   |     | 1                 |                             |
| years_of_exp              | int(11)     | NO   |     | NULL              |                             |
| student_company           | varchar(30) | YES  |     | NULL              |                             |
| batch_date                | varchar(30) | NO   |     | NULL              |                             |
| source_of_joining         | varchar(30) | NO   |     | NULL              |                             |
| location                  | varchar(30) | NO   |     | NULL              |                             |
+---------------------------+-------------+------+-----+-------------------+-----------------------------+
14 rows in set (0.00 sec)
```

[S6_students_INSERT_COMMANDS.txt](Learning%20SQL,%20My%20way%E2%80%A6%203cd0f52f4a6d459d8734814cf27d7657/S6_students_INSERT_COMMANDS.txt)

```sql
mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students;
+------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
| student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location  |
+------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
|          1 | 2022-10-17 11:26:34 |               2 | rohit         | rohit@gmail.com   |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
|          2 | 2022-10-17 11:26:34 |               1 | virat         | virat@gmail.com   |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
|          3 | 2022-10-17 11:26:34 |               3 | shikhar       | shikhar@gmail.com |           12 | NULL            | 19-02-2021 | google            | bangalore |
|          4 | 2022-10-17 11:26:35 |               1 | rahul         | rahul@gmail.com   |            8 | walmart         | 19-02-2021 | quora             | chennai   |
|          5 | 2022-10-17 11:26:36 |               1 | kapil         | kapil@gmail.com   |           15 | microsoft       | 5-02-2021  | friend            | pune      |
|          6 | 2022-10-17 11:26:36 |               1 | brian         | brian@gmail.com   |           18 | tcs             | 5-02-2021  | youtube           | pune      |
|          7 | 2022-10-17 11:26:36 |               1 | cart          | carl@gmail.com    |           20 | wipro           | 19-02-2021 | youtube           | pune      |
|          8 | 2022-10-17 11:26:36 |               1 | saurabh       | saurabh@gmail.com |           14 | wipro           | 19-02-2021 | google            | chennai   |
+------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
8 rows in set (0.00 sec)
```

```sql
**-- courses table**
CREATE TABLE courses
(course_id int NOT NULL, 
course_name varchar(30) NOT NULL, 
course_duration_months int NOT NULL, 
course_fee int NOT NULL, 
PRIMARY KEY(course_id));
```

[S6_courses_Seed_Data.txt](Learning%20SQL,%20My%20way%E2%80%A6%203cd0f52f4a6d459d8734814cf27d7657/S6_courses_Seed_Data.txt)

```sql
mysql> SELECT * FROM courses;
+-----------+-----------------+------------------------+------------+
| course_id | course_name     | course_duration_months | course_fee |
+-----------+-----------------+------------------------+------------+
|         1 | big data        |                      6 |      50000 |
|         2 | web development |                      3 |      20000 |
|         3 | data science    |                      6 |      40000 |
|         4 | devops          |                      1 |      10000 |
+-----------+-----------------+------------------------+------------+
4 rows in set (0.00 sec)
```

Now l**********et’s********** try to add a record in ********the******** `students` table with ********************************************selected_course as 5.******************************************** Will this work? yes, it will, but is it right? No, it isn’t!

There is no course with id as ******5,****** ideally, it should not work. This is where the concept of the ********************FOREIGN key******************** comes into the picture.

```sql
INSERT INTO students (student_fname, student_lname, student_email, student_phone, selected_course, years_of_exp, batch_date, source_of_joining, location)
VALUES('jaspreet', 'bumrah', 'jaspreet@gmail.com', '9595959595', 5, 13,'19-02-2021', 'quora', 'chennai');

Query OK, 1 row affected (0.01 sec)
```

```sql
mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students;
+------------+---------------------+-----------------+---------------+--------------------+--------------+-----------------+------------+-------------------+-----------+
| student_id | enrollment_date     | selected_course | student_fname | student_email      | years_of_exp | student_company | batch_date | source_of_joining | location  |
+------------+---------------------+-----------------+---------------+--------------------+--------------+-----------------+------------+-------------------+-----------+
|          1 | 2022-10-17 11:26:34 |               2 | rohit         | [rohit@gmail.com](mailto:rohit@gmail.com)    |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
|          2 | 2022-10-17 11:26:34 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com)    |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
|          3 | 2022-10-17 11:26:34 |               3 | shikhar       | [shikhar@gmail.com](mailto:shikhar@gmail.com)  |           12 | NULL            | 19-02-2021 | google            | bangalore |
|          4 | 2022-10-17 11:26:35 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com)    |            8 | walmart         | 19-02-2021 | quora             | chennai   |
|          5 | 2022-10-17 11:26:36 |               1 | kapil         | [kapil@gmail.com](mailto:kapil@gmail.com)    |           15 | microsoft       | 5-02-2021  | friend            | pune      |
|          6 | 2022-10-17 11:26:36 |               1 | brian         | [brian@gmail.com](mailto:brian@gmail.com)    |           18 | tcs             | 5-02-2021  | youtube           | pune      |
|          7 | 2022-10-17 11:26:36 |               1 | cart          | [carl@gmail.com](mailto:carl@gmail.com)     |           20 | wipro           | 19-02-2021 | youtube           | pune      |
|          8 | 2022-10-17 11:26:36 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com)  |           14 | wipro           | 19-02-2021 | google            | chennai   |
|          9 | 2022-10-17 11:40:18 |               5 | jaspreet      | [jaspreet@gmail.com](mailto:jaspreet@gmail.com) |           13 | NULL            | 19-02-2021 | quora             | chennai   |
+------------+---------------------+-----------------+---------------+--------------------+--------------+-----------------+------------+-------------------+-----------+
```

Here, the last record should have been rejected as there is no course with id 5.

To fix this, we need to implement the **`FOREIGN KEY.`** For that, we will have to drop the `students` table and create a new schema.

```sql
**-- FOREIGN KEY**
CREATE TABLE students(
student_id int AUTO_INCREMENT,
student_fname varchar(30) NOT NULL,
student_lname varchar(30) NOT NULL,
student_mname varchar(30), 
student_email varchar(40) NOT NULL,
student_phone varchar(15) NOT NULL, 
student_alternative_phone varchar(15), 
enrollment_date TIMESTAMP NOT NULL,
selected_course int NOT NULL DEFAULT 1,
years_of_exp int NOT NULL, 
student_company varchar(30), 
batch_date varchar(30) NOT NULL,
source_of_joining varchar(30) NOT NULL, 
location varchar(30) NOT NULL, 
PRIMARY KEY(student_id), 
UNIQUE KEY(student_email),
FOREIGN KEY(selected_course) REFERENCES courses(course_id)
);
```

How the last line should be read- Create a FOREIGN KEY `selected_course` in table **students** that corresponds to, or refers to, the `course_id` column of the ****************************courses table.**

Here, the **courses** table becomes the parent table for the **students** table, as one of the columns in the **students** table depends on the **courses** table.

```sql
mysql> DESC students;
+---------------------------+-------------+------+-----+-------------------+-----------------------------+
| Field                     | Type        | Null | Key | Default           | Extra                       |
+---------------------------+-------------+------+-----+-------------------+-----------------------------+
| student_id                | int(11)     | NO   | PRI | NULL              | auto_increment              |
| student_fname             | varchar(30) | NO   |     | NULL              |                             |
| student_lname             | varchar(30) | NO   |     | NULL              |                             |
| student_mname             | varchar(30) | YES  |     | NULL              |                             |
| student_email             | varchar(40) | NO   | UNI | NULL              |                             |
| student_phone             | varchar(15) | NO   |     | NULL              |                             |
| student_alternative_phone | varchar(15) | YES  |     | NULL              |                             |
| enrollment_date           | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| selected_course           | int(11)     | NO   | MUL | 1                 |                             |
| years_of_exp              | int(11)     | NO   |     | NULL              |                             |
| student_company           | varchar(30) | YES  |     | NULL              |                             |
| batch_date                | varchar(30) | NO   |     | NULL              |                             |
| source_of_joining         | varchar(30) | NO   |     | NULL              |                             |
| location                  | varchar(30) | NO   |     | NULL              |                             |
+---------------------------+-------------+------+-----+-------------------+-----------------------------+
14 rows in set (0.00 sec)
```

Now add all the data and then try to add a record with `selected_course` as 5.

```sql
mysql> INSERT INTO students (student_fname, student_lname, student_email, student_phone, selected_course, years_of_exp, batch_date, source_of_joining, location)
-> VALUES('jaspreet', 'bumrah', 'jaspreet@gmail.com', '9595959595', 5, 13,'19-02-2021', 'quora', 'chennai');

**ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint 
fails (sahilsi.students, CONSTRAINT students_ibfk_1 FOREIGN KEY (selected_course) REFERENCES courses (course_id))**
```

<aside>
💡 **Deleting FOREIGN keys**
Now suppose, the web development course, id=2, is discontinued then there is no point in keeping this record in the `courses` table. Can we **delete** this course? let’s give it a try

</aside>

```sql
mysql> DELETE FROM courses WHERE course_id=2;

**ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key 
constraint fails (sahilsi.students, CONSTRAINT students_ibfk_1 FOREIGN KEY
 (selected_course) REFERENCES courses (course_id))**
```

Hehe…we cannot delete that as it’s a violation of FOREIGN KEY rules. Think in this way too, if we delete course_id=2, what will happen to the records that are dependent on it in the `students` table.

So what will happen if try to delete a course which is not opted by any student in the `students` table? **It will work!!**

```sql
mysql> DELETE FROM courses WHERE course_id=4;
Query OK, 1 row affected (0.01 sec)
```

The deletion worked as no record in `students` table had selected_course=4.

### Session 6

- ************DISTINCT************
    
    ```sql
    mysql> SELECT DISTINCT(location) FROM students;
    +-----------+
    | location  |
    +-----------+
    | bangalore |
    | hyderabad |
    | chennai   |
    | pune      |
    +-----------+
    ```
    
- ****************ORDER BY****************
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location 
    FROM students 
    **ORDER BY years_of_exp;**
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com)   |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    |          1 | 2022-10-17 11:55:58 |               2 | rohit         | [rohit@gmail.com](mailto:rohit@gmail.com)   |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
    |          4 | 2022-10-17 11:55:58 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com)   |            8 | walmart         | 19-02-2021 | quora             | chennai   |
    |          3 | 2022-10-17 11:55:58 |               3 | shikhar       | [shikhar@gmail.com](mailto:shikhar@gmail.com) |           12 | NULL            | 19-02-2021 | google            | bangalore |
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai   |
    |          5 | 2022-10-17 11:55:59 |               1 | kapil         | [kapil@gmail.com](mailto:kapil@gmail.com)   |           15 | microsoft       | 5-02-2021  | friend            | pune      |
    |          6 | 2022-10-17 11:55:59 |               1 | brian         | [brian@gmail.com](mailto:brian@gmail.com)   |           18 | tcs             | 5-02-2021  | youtube           | pune      |
    |          7 | 2022-10-17 11:55:59 |               1 | cart          | [carl@gmail.com](mailto:carl@gmail.com)    |           20 | wipro           | 19-02-2021 | youtube           | pune      |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    8 rows in set (0.00 sec)
    ```
    
    By default **********************ORDER BY********************** orders the records in ********ASC******** order.
    `DESC` will order the result in Descending order.
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location 
    FROM students 
    **ORDER BY years_of_exp DESC;**
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    |          7 | 2022-10-17 11:55:59 |               1 | cart          | [carl@gmail.com](mailto:carl@gmail.com)    |           20 | wipro           | 19-02-2021 | youtube           | pune      |
    |          6 | 2022-10-17 11:55:59 |               1 | brian         | [brian@gmail.com](mailto:brian@gmail.com)   |           18 | tcs             | 5-02-2021  | youtube           | pune      |
    |          5 | 2022-10-17 11:55:59 |               1 | kapil         | [kapil@gmail.com](mailto:kapil@gmail.com)   |           15 | microsoft       | 5-02-2021  | friend            | pune      |
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai   |
    |          3 | 2022-10-17 11:55:58 |               3 | shikhar       | [shikhar@gmail.com](mailto:shikhar@gmail.com) |           12 | NULL            | 19-02-2021 | google            | bangalore |
    |          4 | 2022-10-17 11:55:58 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com)   |            8 | walmart         | 19-02-2021 | quora             | chennai   |
    |          1 | 2022-10-17 11:55:58 |               2 | rohit         | [rohit@gmail.com](mailto:rohit@gmail.com)   |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com)   |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    8 rows in set (0.01 sec)
    ```
    
    You can ORDER BY the records by multiple columns.
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location 
    FROM students
    **ORDER BY selected_course, years_of_exp;**
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com)   |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    |          4 | 2022-10-17 11:55:58 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com)   |            8 | walmart         | 19-02-2021 | quora             | chennai   |
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai   |
    |          5 | 2022-10-17 11:55:59 |               1 | kapil         | [kapil@gmail.com](mailto:kapil@gmail.com)   |           15 | microsoft       | 5-02-2021  | friend            | pune      |
    |          6 | 2022-10-17 11:55:59 |               1 | brian         | [brian@gmail.com](mailto:brian@gmail.com)   |           18 | tcs             | 5-02-2021  | youtube           | pune      |
    |          7 | 2022-10-17 11:55:59 |               1 | cart          | [carl@gmail.com](mailto:carl@gmail.com)    |           20 | wipro           | 19-02-2021 | youtube           | pune      |
    |          1 | 2022-10-17 11:55:58 |               2 | rohit         | [rohit@gmail.com](mailto:rohit@gmail.com)   |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
    |          3 | 2022-10-17 11:55:58 |               3 | shikhar       | [shikhar@gmail.com](mailto:shikhar@gmail.com) |           12 | NULL            | 19-02-2021 | google            | bangalore |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    8 rows in set (0.00 sec)
    ```
    
    (Above)We have ordered the record first by ********************************selected_course******************************** id and then by ************************years_of_exp************************.
    
    <aside>
    💡 Check the below query.
    
    </aside>
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location 
    FROM students 
    ORDER BY selected_course DESC, years_of_exp DESC;
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    |          3 | 2022-10-17 11:55:58 |               3 | shikhar       | [shikhar@gmail.com](mailto:shikhar@gmail.com) |           12 | NULL            | 19-02-2021 | google            | bangalore |
    |          1 | 2022-10-17 11:55:58 |               2 | rohit         | [rohit@gmail.com](mailto:rohit@gmail.com)   |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
    |          7 | 2022-10-17 11:55:59 |               1 | cart          | [carl@gmail.com](mailto:carl@gmail.com)    |           20 | wipro           | 19-02-2021 | youtube           | pune      |
    |          6 | 2022-10-17 11:55:59 |               1 | brian         | [brian@gmail.com](mailto:brian@gmail.com)   |           18 | tcs             | 5-02-2021  | youtube           | pune      |
    |          5 | 2022-10-17 11:55:59 |               1 | kapil         | [kapil@gmail.com](mailto:kapil@gmail.com)   |           15 | microsoft       | 5-02-2021  | friend            | pune      |
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai   |
    |          4 | 2022-10-17 11:55:58 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com)   |            8 | walmart         | 19-02-2021 | quora             | chennai   |
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com)   |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    8 rows in set (0.00 sec)
    ```
    
    <aside>
    💡 Tips & tricks!?
    
    </aside>
    
    ```sql
    SELECT student_fname, years_of_exp 
    FROM students 
    ORDER BY years_of_exp DESC;
    +---------------+--------------+
    | student_fname | years_of_exp |
    +---------------+--------------+
    | cart          |           20 |
    | brian         |           18 |
    | kapil         |           15 |
    | saurabh       |           14 |
    | shikhar       |           12 |
    | rahul         |            8 |
    | rohit         |            6 |
    | virat         |            3 |
    +---------------+--------------+
    8 rows in set (0.00 sec)
    ```
    
    Below is a variation of the above query, here we are selecting two columns `student_fname` and `years_of_exp` and then ordering the data based on the 2nd column, that is `years_of_exp.`  
    
    The same query can be written as…
    
    ```sql
    mysql> SELECT student_fname, years_of_exp 
    FROM students 
    **ORDER BY 2 DESC;**
    +---------------+--------------+
    | student_fname | years_of_exp |
    +---------------+--------------+
    | cart          |           20 |
    | brian         |           18 |
    | kapil         |           15 |
    | saurabh       |           14 |
    | shikhar       |           12 |
    | rahul         |            8 |
    | rohit         |            6 |
    | virat         |            3 |
    +---------------+--------------+
    8 rows in set (0.00 sec)
    ```
    
- **********LIMIT**********
    
    ** This is better used with ******************ORDER BY clause.******************
    
    Using `LIMIT`
    
    ```sql
    mysql> SELECT student_fname, years_of_exp 
    FROM students 
    ORDER BY years_of_exp DESC 
    **LIMIT 3;**
    +---------------+--------------+
    | student_fname | years_of_exp |
    +---------------+--------------+
    | cart          |           20 |
    | brian         |           18 |
    | kapil         |           15 |
    +---------------+--------------+
    3 rows in set (0.04 sec)
    ```
    
    We got only the top 3 records satisfying the conditions provided in the query.
    The above query gives us the Top 3 students having the most exp.
    
    Q: Give the record of the last enrolled student.
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> ORDER BY enrollment_date DESC
    -> LIMIT 1;
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    ```
    
    Q: Give the details of the last 3 enrolled students. **[LIMTI 0,3]**
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> ORDER BY enrollment_date DESC
    -> **LIMIT 0,3;**
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    |          7 | 2022-10-17 11:55:59 |               1 | cart          | [carl@gmail.com](mailto:carl@gmail.com)    |           20 | wipro           | 19-02-2021 | youtube           | pune     |
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai  |
    |          6 | 2022-10-17 11:55:59 |               1 | brian         | [brian@gmail.com](mailto:brian@gmail.com)   |           18 | tcs             | 5-02-2021  | youtube           | pune     |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    3 rows in set (0.00 sec)
    ```
    
    Notice that we have given LIMIT 0,3, which means after ORDERing the records as enrollment_date in DESC order, **********************************************************************************************************start the LIMIT from 0th row and give 3 records, i.e., LIMIT 0,3.**********************************************************************************************************
    
    Q: Give the details of first 3rd and 4th student to enroll. (ORDER in ASC and give 3rd and 4th records)
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> ORDER BY enrollment_date
    **-> LIMIT 2,2;**
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    |          3 | 2022-10-17 11:55:58 |               3 | shikhar       | [shikhar@gmail.com](mailto:shikhar@gmail.com) |           12 | NULL            | 19-02-2021 | google            | bangalore |
    |          4 | 2022-10-17 11:55:58 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com)   |            8 | walmart         | 19-02-2021 | quora             | chennai   |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    2 rows in set (0.00 sec)
    ```
    
- **LIKE**
    
    Q: Select students whose name contains ‘**ra’.**
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> WHERE student_fname LIKE '%ra%';
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com)   |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    |          4 | 2022-10-17 11:55:58 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com)   |            8 | walmart         | 19-02-2021 | quora             | chennai   |
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai   |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+-----------+
    3 rows in set (0.00 sec)
    ```
    
    the `%` means **************************************************************0 or more, nothing or anything.**************************************************************
    
    So in the result we can see that we got 2 records which has ************ra************ in middle and also 1 record which contains ra at the very beginning.
    
    Q: Select students whose name contains starts with ‘vi’**.**
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> WHERE student_fname LIKE 'vi%';
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email   | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com) |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    1 row in set (0.00 sec)
    ```
    
    Another wild card that is used in LIKE is `_` underscore. Underscore means anything but not null, 1 `_` means one character, 2 `_` means two and so on…
    Q: Select students whose name contains two characters before ‘ra’.
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> WHERE student_fname LIKE '__ra%';
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email   | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com) |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    1 row in set (0.00 sec)
    ```
    
    Q: Select students whose name contains 3 characters before ‘ra’.
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> WHERE student_fname LIKE '___ra%';
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email     | years_of_exp | student_company | batch_date | source_of_joining | location |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    |          8 | 2022-10-17 11:55:59 |               1 | saurabh       | [saurabh@gmail.com](mailto:saurabh@gmail.com) |           14 | wipro           | 19-02-2021 | google            | chennai  |
    +------------+---------------------+-----------------+---------------+-------------------+--------------+-----------------+------------+-------------------+----------+
    1 row in set (0.06 sec)
    ```
    
    Q: Select students whose first name contains only 5 characters.
    
    ```sql
    mysql> SELECT student_id, enrollment_date, selected_course, student_fname, student_email, years_of_exp, student_company, batch_date, source_of_joining, location FROM students
    -> WHERE student_fname LIKE '_____';
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    | student_id | enrollment_date     | selected_course | student_fname | student_email   | years_of_exp | student_company | batch_date | source_of_joining | location  |
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    |          1 | 2022-10-17 11:55:58 |               2 | rohit         | [rohit@gmail.com](mailto:rohit@gmail.com) |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
    |          2 | 2022-10-17 11:55:58 |               1 | virat         | [virat@gmail.com](mailto:virat@gmail.com) |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
    |          4 | 2022-10-17 11:55:58 |               1 | rahul         | [rahul@gmail.com](mailto:rahul@gmail.com) |            8 | walmart         | 19-02-2021 | quora             | chennai   |
    |          5 | 2022-10-17 11:55:59 |               1 | kapil         | [kapil@gmail.com](mailto:kapil@gmail.com) |           15 | microsoft       | 5-02-2021  | friend            | pune      |
    |          6 | 2022-10-17 11:55:59 |               1 | brian         | [brian@gmail.com](mailto:brian@gmail.com) |           18 | tcs             | 5-02-2021  | youtube           | pune      |
    +------------+---------------------+-----------------+---------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
    5 rows in set (0.00 sec)
    ```
    

### [Session 7](https://www.youtube.com/watch?v=JUCTcHsNkyM&list=PLtgiThe4j67rAoPmnCQmcgLS4iIc5ungg&index=8)

I would suggest going through the video explanation. (click on Session 7)

### Session 8 [AGGREGATE FUNCTIONS](https://www.geeksforgeeks.org/aggregate-functions-in-sql/)

- **COUNT()**
    
    ```sql
    **-- DISTINCT student_company**
    mysql> SELECT COUNT(DISTINCT(student_company)) FROM students;
    +----------------------------------+
    | COUNT(DISTINCT(student_company)) |
    +----------------------------------+
    |                                5 |
    +----------------------------------+
    1 row in set (0.00 sec)
    
    SELECT DISTINCT(student_company) FROM students;
    +-----------------+
    | student_company |
    +-----------------+
    | walmart         |
    | flipkart        |
    | NULL            |
    | microsoft       |
    | tcs             |
    | wipro           |
    +-----------------+
    ```
    
    ```sql
    **-- Count of students joining from diff sources**
    mysql> SELECT source_of_joining, COUNT(student_id) AS no_of_students
    -> FROM students
    -> GROUP BY source_of_joining;
    +-------------------+----------------+
    | source_of_joining | no_of_students |
    +-------------------+----------------+
    | friend            |              1 |
    | google            |              2 |
    | linkedin          |              2 |
    | quora             |              1 |
    | youtube           |              2 |
    +-------------------+----------------+
    5 rows in set (0.00 sec)
    ```
    
    ```sql
    **-- ORDER BY DESC Count of students joining from diff sources**
    mysql> SELECT source_of_joining, COUNT(student_id) AS no_of_students
    -> FROM students
    -> GROUP BY source_of_joining
    -> **ORDER BY no_of_students DESC;**
    +-------------------+----------------+
    | source_of_joining | no_of_students |
    +-------------------+----------------+
    | youtube           |              2 |
    | linkedin          |              2 |
    | google            |              2 |
    | quora             |              1 |
    | friend            |              1 |
    +-------------------+----------------+
    5 rows in set (0.00 sec)
    ```
    
- GROUP BY more than 1 column
    
    ```sql
    mysql> SELECT batch_date, selected_course FROM students;
    +------------+-----------------+
    | batch_date | selected_course |
    +------------+-----------------+
    | 5-02-2021  |               2 |
    | 5-02-2021  |               1 |
    | 19-02-2021 |               3 |
    | 19-02-2021 |               1 |
    | 5-02-2021  |               1 |
    | 5-02-2021  |               1 |
    | 19-02-2021 |               1 |
    | 19-02-2021 |               1 |
    +------------+-----------------+
    8 rows in set (0.00 sec)
    ```
    
    <aside>
    💡 Let’s say we have a use case where we need to find courses and their counts enrolled in every batch.
    
    here we will need to use aggregation on two columns
    
    </aside>
    
    ```sql
    mysql> SELECT batch_date, selected_course, COUNT(selected_course)
    -> FROM students
    -> GROUP BY batch_date, selected_course ;
    +------------+-----------------+------------------------+
    | batch_date | selected_course | COUNT(selected_course) |
    +------------+-----------------+------------------------+
    | 19-02-2021 |               1 |                      3 |
    | 19-02-2021 |               3 |                      1 |
    | 5-02-2021  |               1 |                      3 |
    | 5-02-2021  |               2 |                      1 |
    +------------+-----------------+------------------------+
    4 rows in set (0.00 sec)
    ```
    
- **MIN() & MAX()**
    
    ```sql
    mysql> SELECT MAX(years_of_exp) FROM students;
    +-------------------+
    | MAX(years_of_exp) |
    +-------------------+
    |                20 |
    +-------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT MIN(years_of_exp ) FROM students;
    +--------------------+
    | MIN(years_of_exp ) |
    +--------------------+
    |                  3 |
    +--------------------+
    1 row in set (0.00 sec)
    ```
    
    ```sql
    **-- For each source of joining, get MAX exp**
    
    mysql> SELECT source_of_joining, MAX(years_of_exp) 
    FROM students 
    GROUP BY source_of_joining ;
    +-------------------+-------------------+
    | source_of_joining | MAX(years_of_exp) |
    +-------------------+-------------------+
    | friend            |                15 |
    | google            |                14 |
    | linkedin          |                 6 |
    | quora             |                 8 |
    | youtube           |                20 |
    +-------------------+-------------------+
    5 rows in set (0.00 sec)
    ```
    
- **AVG()**
    
    ```sql
    **-- Get AVG years of exp for each source of joining**
    
    mysql> SELECT source_of_joining, AVG(years_of_exp)
    -> FROM students
    -> GROUP BY source_of_joining;
    +-------------------+-------------------+
    | source_of_joining | AVG(years_of_exp) |
    +-------------------+-------------------+
    | friend            |           15.0000 |
    | google            |           13.0000 |
    | linkedin          |            4.5000 |
    | quora             |            8.0000 |
    | youtube           |           19.0000 |
    +-------------------+-------------------+
    5 rows in set (0.00 sec)
    ```
    

### Session 9

Some more data types- DECIMAL and TIMESTAMP

```sql
mysql> DESC courses;
+------------------------+-------------+------+-----+---------+-------+
| Field                  | Type        | Null | Key | Default | Extra |
+------------------------+-------------+------+-----+---------+-------+
| course_id              | int(11)     | NO   | PRI | NULL    |       |
| course_name            | varchar(30) | NO   |     | NULL    |       |
| course_duration_months | int(11)     | NO   |     | NULL    |       |
| course_fee             | int(11)     | NO   |     | NULL    |       |
+------------------------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

mysql> SELECT * FROM courses;
+-----------+-----------------+------------------------+------------+
| course_id | course_name     | course_duration_months | course_fee |
+-----------+-----------------+------------------------+------------+
|         1 | big data        |                      6 |      50000 |
|         2 | web development |                      3 |      20000 |
|         3 | data science    |                      6 |      40000 |
+-----------+-----------------+------------------------+------------+
3 rows in set (0.00 sec)
```

Let’s say we need to update course duration, it is possible that a course can be of 6 and 1/2 months, i.e., 6.5 months etc. Can we update data science course to 6.5 in the existing table?

```sql
mysql> UPDATE courses SET course_duration_months=6.5 WHERE course_name='data science';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

**-- Ohh, the query worked...but is it giving the correct result?**

mysql> SELECT * FROM courses;
+-----------+-----------------+------------------------+------------+
| course_id | course_name     | course_duration_months | course_fee |
+-----------+-----------------+------------------------+------------+
|         1 | big data        |                      6 |      50000 |
|         2 | web development |                      3 |      20000 |
|         3 | data science    |                      7 |      40000 |
+-----------+-----------------+------------------------+------------+
3 rows in set (0.00 sec)
```

As expected, the output is not correct. SQL rounded 6.5 to 7 because we are trying to add a `decimal` in an `int` field.

- **************decimal()**************
    
    ```sql
    mysql> CREATE TABLE courses_new(
    -> course_id int AUTO_INCREMENT,
    -> course_name varchar(20),
    -> course_duration_months **decimal(3,1)**,
    -> course_fee int,
    -> PRIMARY KEY(course_id));
    Query OK, 0 rows affected (0.06 sec)
    ```
    
    `decimal(3,1)` means that the field can have a max of 3 digits and it can have only 1 digit after the decimal point.
    
    - 12.5 - correct
    - 3.5 - correct
    - 3 - correct
    - 3.53 - incorrect (2 digits after the decimal point) data will be rounded and the value will be set as `3.5`
    - 125.2 - incorrect (4 digits) `ERROR - Out of range value`
    
    ```sql
    mysql> DESC courses_new;
    +------------------------+--------------+------+-----+---------+----------------+
    | Field                  | Type         | Null | Key | Default | Extra          |
    +------------------------+--------------+------+-----+---------+----------------+
    | course_id              | int(11)      | NO   | PRI | NULL    | auto_increment |
    | course_name            | varchar(20)  | YES  |     | NULL    |                |
    | course_duration_months | decimal(3,1) | YES  |     | NULL    |                |
    | course_fee             | int(11)      | YES  |     | NULL    |                |
    +------------------------+--------------+------+-----+---------+----------------+
    4 rows in set (0.00 sec)
    ```
    
    ```sql
    -- let's add some data
    insert into courses_new values(1, 'big data', 6, 50000);
    insert into courses_new values(2, 'web development', 12.5, 20000);
    insert into courses_new values(3, 'data science', 6.5, 40000);
    
    mysql> SELECT * FROM courses_new;
    +-----------+-----------------+------------------------+------------+
    | course_id | course_name     | course_duration_months | course_fee |
    +-----------+-----------------+------------------------+------------+
    |         1 | big data        |                    6.0 |      50000 |
    |         2 | web development |                   12.5 |      20000 |
    |         3 | data science    |                    6.5 |      40000 |
    +-----------+-----------------+------------------------+------------+
    ```
    
    Try to add incorrectly formatted data
    
    ```sql
    mysql> INSERT INTO courses_new(course_name, course_duration_months, course_fee)
    VALUES('data analytics', 125.56, 40000);
    
    **ERROR 1264 (22003): Out of range value for column 'course_duration_months' 
    at row 1**
    ```
    
- **TIMESTAMP**
    
    ```sql
    mysql> CREATE TABLE courses_new(
    -> course_id int AUTO_INCREMENT,
    -> course_name varchar(20),
    -> course_duration_months decimal(3,1),
    -> course_fee int,
    -> **changed_at TIMESTAMP DEFAULT NOW()**,
    -> PRIMARY KEY(course_id));
    
    DESC courses_new;
    +------------------------+--------------+------+-----+-------------------+----------------+
    | Field                  | Type         | Null | Key | Default           | Extra          |
    +------------------------+--------------+------+-----+-------------------+----------------+
    | course_id              | int(11)      | NO   | PRI | NULL              | auto_increment |
    | course_name            | varchar(20)  | YES  |     | NULL              |                |
    | course_duration_months | decimal(3,1) | YES  |     | NULL              |                |
    | course_fee             | int(11)      | YES  |     | NULL              |                |
    | changed_at             | timestamp    | NO   |     | CURRENT_TIMESTAMP |                |
    +------------------------+--------------+------+-----+-------------------+----------------+
    5 rows in set (0.00 sec)
    ```
    
    ```sql
    INSERT INTO courses_new(course_name, course_duration_months, course_fee) 
    VALUES('big data', 6.5, 40000);
    
    mysql> SELECT changed_at, course_id, course_name, course_duration_months, course_fee  
    FROM courses_new;
    +---------------------+-----------+-------------+------------------------+------------+
    | changed_at          | course_id | course_name | course_duration_months | course_fee |
    +---------------------+-----------+-------------+------------------------+------------+
    | 2022-10-22 09:30:13 |         1 | big data    |                    6.5 |  40000     |
    +---------------------+-----------+-------------+------------------------+------------+
    1 row in set (0.00 sec)
    ```
    
    Let’s update the course_fee from 40000 to 51000, then check the `changed_at`
    
    ```sql
    mysql> SELECT changed_at, course_id, course_name, course_duration_months, course_fee  FROM courses_new;
    +---------------------+-----------+-------------+------------------------+------------+
    | changed_at          | course_id | course_name | course_duration_months | course_fee |
    +---------------------+-----------+-------------+------------------------+------------+
    | 2022-10-22 09:30:13 |         1 | big data    |                    6.5 |      51000 |
    +---------------------+-----------+-------------+------------------------+------------+
    1 row in set (0.05 sec)
    ```
    
    Oops, it didn’t work. Its same as before, but what could have went wrong?
    For this field to change it’s timestamp on every new UPDATE, we need to change the schema of the table and add `ON UPDATE` property to this field.
    
    ```sql
    ALTER TABLE courses_new 
    MODIFY changed_at TIMESTAMP DEFAULT NOW() **ON UPDATE CURRENT_TIMESTAMP()**;
    Query OK, 0 rows affected (0.10 sec)
    Records: 0  Duplicates: 0  Warnings: 0
    
    mysql> DESC courses_new;
    +------------------------+--------------+------+-----+-------------------+-----------------------------+
    | Field                  | Type         | Null | Key | Default           | Extra                       |
    +------------------------+--------------+------+-----+-------------------+-----------------------------+
    | course_id              | int(11)      | NO   | PRI | NULL              | auto_increment              |
    | course_name            | varchar(20)  | YES  |     | NULL              |                             |
    | course_duration_months | decimal(3,1) | YES  |     | NULL              |                             |
    | course_fee             | int(11)      | YES  |     | NULL              |                             |
    | changed_at             | timestamp    | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
    +------------------------+--------------+------+-----+-------------------+-----------------------------+
    5 rows in set (0.00 sec)
    ```
    
    Here, I have used **CURRENT_TIMESTAMP(), which is the same as NOW().**
    
    Now lets update something…
    
    ```sql
    mysql> UPDATE courses_new SET course_duration_months= 7.5 WHERE course_id=1;
    Query OK, 1 row affected (0.01 sec)
    Rows matched: 1  Changed: 1  Warnings: 0
    ```
    
    ```sql
    mysql> SELECT changed_at, course_id, course_name, course_duration_months, course_fee  FROM courses_new;
    +---------------------+-----------+-------------+------------------------+------------+
    | changed_at          | course_id | course_name | course_duration_months | course_fee |
    +---------------------+-----------+-------------+------------------------+------------+
    | **2022-10-25 11:32:30** |         1 | big data    |                    7.5 |      51000 |
    +---------------------+-----------+-------------+------------------------+------------+
    1 row in set (0.00 sec)
    ```
    
    It worked!! **So, to create a table with this functionality, we need to add the `DEFAULT` and `ON UPDATE` properties to the required field.**
    
    ```sql
    CREATE TABLE courses_new(
    course_id int AUTO_INCREMENT,
    course_name varchar(20),
    course_duration_months decimal(3,1),
    course_fee int,
    **changed_at TIMESTAMP DEFAULT NOW() ON UPDATE NOW()**,
    PRIMARY KEY(course_id));
    ```
    

### Session 10 Logical operators and CASE

- **Logical Operators**
    - **NOT**
        
        ```sql
        mysql> SELECT * FROM students WHERE location **!=** 'bangalore';
        +------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        | student_id | student_fname | student_lname | student_mname | student_email     | student_phone | student_alternative_phone | enrollment_date     | selected_course | years_of_exp | student_company | batch_date | source_of_joining | location  |
        +------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        |          2 | virat         | kohli         | NULL          | [virat@gmail.com](mailto:virat@gmail.com)   | 9292929292    | NULL                      | 2022-10-17 11:55:58 |               1 |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
        |          4 | rahul         | dravid        | NULL          | [rahul@gmail.com](mailto:rahul@gmail.com)   | 9494949494    | NULL                      | 2022-10-17 11:55:58 |               1 |            8 | walmart         | 19-02-2021 | quora             | chennai   |
        |          5 | kapil         | dev           | NULL          | [kapil@gmail.com](mailto:kapil@gmail.com)   | 9291292292    | NULL                      | 2022-10-17 11:55:59 |               1 |           15 | microsoft       | 5-02-2021  | friend            | pune      |
        |          6 | brian         | lara          | NULL          | [brian@gmail.com](mailto:brian@gmail.com)   | 9291297292    | NULL                      | 2022-10-17 11:55:59 |               1 |           18 | tcs             | 5-02-2021  | youtube           | pune      |
        |          7 | cart          | hooper        | NULL          | [carl@gmail.com](mailto:carl@gmail.com)    | 9291297392    | NULL                      | 2022-10-17 11:55:59 |               1 |           20 | wipro           | 19-02-2021 | youtube           | pune      |
        |          8 | saurabh       | ganguly       | NULL          | [saurabh@gmail.com](mailto:saurabh@gmail.com) | 9291297492    | NULL                      | 2022-10-17 11:55:59 |               1 |           14 | wipro           | 19-02-2021 | google            | chennai   |
        +------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        6 rows in set (0.00 sec)
        ```
        
    - ****************NOT LIKE****************
        
        ```sql
        mysql> SELECT * FROM courses WHERE course_name **NOT LIKE '%data%';**
        +-----------+-----------------+------------------------+------------+
        | course_id | course_name     | course_duration_months | course_fee |
        +-----------+-----------------+------------------------+------------+
        |         2 | web development |                      3 |      20000 |
        +-----------+-----------------+------------------------+------------+
        1 row in set (0.00 sec)
        ```
        
    - **AND**
        
        ```sql
        mysql> SELECT * FROM students 
        WHERE source_of_joining='linkedin' AND years_of_exp < 4;
        +------------+---------------+---------------+---------------+-----------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        | student_id | student_fname | student_lname | student_mname | student_email   | student_phone | student_alternative_phone | enrollment_date     | selected_course | years_of_exp | student_company | batch_date | source_of_joining | location  |
        +------------+---------------+---------------+---------------+-----------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        |          2 | virat         | kohli         | NULL          | [virat@gmail.com](mailto:virat@gmail.com) | 9292929292    | NULL                      | 2022-10-17 11:55:58 |               1 |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
        +------------+---------------+---------------+---------------+-----------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        1 row in set (0.00 sec)
        ```
        
    - ****OR****
        
        ```sql
        mysql> SELECT * FROM students 
        WHERE location='bangalore' AND location='hyderabad';
        Empty set (0.00 sec)
        
        --This will never work as it is logically incorrect, there cannot be 
        -- a record in the table with location as `bangalore` and `hyderabad`
        ```
        
        ```sql
        mysql> SELECT * FROM students 
        WHERE location='bangalore' OR location='hyderabad';
        +------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        | student_id | student_fname | student_lname | student_mname | student_email     | student_phone | student_alternative_phone | enrollment_date     | selected_course | years_of_exp | student_company | batch_date | source_of_joining | location  |
        +------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        |          1 | rohit         | Sharma        | NULL          | [rohit@gmail.com](mailto:rohit@gmail.com)   | 9191919191    | NULL                      | 2022-10-17 11:55:58 |               2 |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
        |          2 | virat         | kohli         | NULL          | [virat@gmail.com](mailto:virat@gmail.com)   | 9292929292    | NULL                      | 2022-10-17 11:55:58 |               1 |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
        |          3 | shikhar       | dhawan        | NULL          | [shikhar@gmail.com](mailto:shikhar@gmail.com) | 9393939393    | NULL                      | 2022-10-17 11:55:58 |               3 |           12 | NULL            | 19-02-2021 | google            | bangalore |
        +------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        3 rows in set (0.00 sec)
        ```
        
    - **************BETWEEN**************
        
        ```sql
        mysql> SELECT years_of_exp, students.* FROM students 
        **WHERE years_of_exp BETWEEN 3 AND 12;**
        +--------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        | years_of_exp | student_id | student_fname | student_lname | student_mname | student_email     | student_phone | student_alternative_phone | enrollment_date     | selected_course | years_of_exp | student_company | batch_date | source_of_joining | location  |
        +--------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        |            6 |          1 | rohit         | Sharma        | NULL          | [rohit@gmail.com](mailto:rohit@gmail.com)   | 9191919191    | NULL                      | 2022-10-17 11:55:58 |               2 |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
        |            3 |          2 | virat         | kohli         | NULL          | [virat@gmail.com](mailto:virat@gmail.com)   | 9292929292    | NULL                      | 2022-10-17 11:55:58 |               1 |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
        |           12 |          3 | shikhar       | dhawan        | NULL          | [shikhar@gmail.com](mailto:shikhar@gmail.com) | 9393939393    | NULL                      | 2022-10-17 11:55:58 |               3 |           12 | NULL            | 19-02-2021 | google            | bangalore |
        |            8 |          4 | rahul         | dravid        | NULL          | [rahul@gmail.com](mailto:rahul@gmail.com)   | 9494949494    | NULL                      | 2022-10-17 11:55:58 |               1 |            8 | walmart         | 19-02-2021 | quora             | chennai   |
        +--------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        4 rows in set (0.00 sec)
        ```
        
    - **********************NOT BETWEEN**********************
        
        ```sql
        mysql> SELECT years_of_exp, students.* FROM students 
        **WHERE years_of_exp NOT BETWEEN 3 AND 12;**
        +--------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+----------+
        | years_of_exp | student_id | student_fname | student_lname | student_mname | student_email     | student_phone | student_alternative_phone | enrollment_date     | selected_course | years_of_exp | student_company | batch_date | source_of_joining | location |
        +--------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+----------+
        |           15 |          5 | kapil         | dev           | NULL          | [kapil@gmail.com](mailto:kapil@gmail.com)   | 9291292292    | NULL                      | 2022-10-17 11:55:59 |               1 |           15 | microsoft       | 5-02-2021  | friend            | pune     |
        |           18 |          6 | brian         | lara          | NULL          | [brian@gmail.com](mailto:brian@gmail.com)   | 9291297292    | NULL                      | 2022-10-17 11:55:59 |               1 |           18 | tcs             | 5-02-2021  | youtube           | pune     |
        |           20 |          7 | cart          | hooper        | NULL          | [carl@gmail.com](mailto:carl@gmail.com)    | 9291297392    | NULL                      | 2022-10-17 11:55:59 |               1 |           20 | wipro           | 19-02-2021 | youtube           | pune     |
        |           14 |          8 | saurabh       | ganguly       | NULL          | [saurabh@gmail.com](mailto:saurabh@gmail.com) | 9291297492    | NULL                      | 2022-10-17 11:55:59 |               1 |           14 | wipro           | 19-02-2021 | google            | chennai  |
        +--------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+----------+
        4 rows in set (0.00 sec)
        ```
        
    - ****IN****
        
        ```sql
        mysql> SELECT student_company, students.* FROM students 
        **WHERE student_company IN ('walmart', 'flipkart', 'tcs');**
        +-----------------+------------+---------------+---------------+---------------+-----------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        | student_company | student_id | student_fname | student_lname | student_mname | student_email   | student_phone | student_alternative_phone | enrollment_date     | selected_course | years_of_exp | student_company | batch_date | source_of_joining | location  |
        +-----------------+------------+---------------+---------------+---------------+-----------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        | walmart         |          1 | rohit         | Sharma        | NULL          | [rohit@gmail.com](mailto:rohit@gmail.com) | 9191919191    | NULL                      | 2022-10-17 11:55:58 |               2 |            6 | walmart         | 5-02-2021  | linkedin          | bangalore |
        | flipkart        |          2 | virat         | kohli         | NULL          | [virat@gmail.com](mailto:virat@gmail.com) | 9292929292    | NULL                      | 2022-10-17 11:55:58 |               1 |            3 | flipkart        | 5-02-2021  | linkedin          | hyderabad |
        | walmart         |          4 | rahul         | dravid        | NULL          | [rahul@gmail.com](mailto:rahul@gmail.com) | 9494949494    | NULL                      | 2022-10-17 11:55:58 |               1 |            8 | walmart         | 19-02-2021 | quora             | chennai   |
        | tcs             |          6 | brian         | lara          | NULL          | [brian@gmail.com](mailto:brian@gmail.com) | 9291297292    | NULL                      | 2022-10-17 11:55:59 |               1 |           18 | tcs             | 5-02-2021  | youtube           | pune      |
        +-----------------+------------+---------------+---------------+---------------+-----------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+-----------+
        4 rows in set (0.00 sec)
        ```
        
    - ************NOT IN************
        
        ```sql
        mysql> mysql> SELECT student_company, students.* FROM students 
        **WHERE student_company NOT IN ('walmart', 'flipkart', 'tcs');**
        +-----------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+----------+
        | student_company | student_id | student_fname | student_lname | student_mname | student_email     | student_phone | student_alternative_phone | enrollment_date     | selected_course | years_of_exp | student_company | batch_date | source_of_joining | location |
        +-----------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+----------+
        | microsoft       |          5 | kapil         | dev           | NULL          | [kapil@gmail.com](mailto:kapil@gmail.com)   | 9291292292    | NULL                      | 2022-10-17 11:55:59 |               1 |           15 | microsoft       | 5-02-2021  | friend            | pune     |
        | wipro           |          7 | cart          | hooper        | NULL          | [carl@gmail.com](mailto:carl@gmail.com)    | 9291297392    | NULL                      | 2022-10-17 11:55:59 |               1 |           20 | wipro           | 19-02-2021 | youtube           | pune     |
        | wipro           |          8 | saurabh       | ganguly       | NULL          | [saurabh@gmail.com](mailto:saurabh@gmail.com) | 9291297492    | NULL                      | 2022-10-17 11:55:59 |               1 |           14 | wipro           | 19-02-2021 | google            | chennai  |
        +-----------------+------------+---------------+---------------+---------------+-------------------+---------------+---------------------------+---------------------+-----------------+--------------+-----------------+------------+-------------------+----------+
        3 rows in set (0.00 sec)
        ```
        
- ********CASE********
    
    ```sql
    mysql> SELECT student_id, student_fname, student_lname, student_company,
        CASE 
    		WHEN student_company IN ('walmart', 'flipkart', 'microsoft') THEN 'product based'
        WHEN student_company IS NULL THEN 'invalid company'
        ELSE 'service based'
        END AS company_type
        FROM students;
    +------------+---------------+---------------+-----------------+-----------------+
    | student_id | student_fname | student_lname | student_company | company_type    |
    +------------+---------------+---------------+-----------------+-----------------+
    |          1 | rohit         | Sharma        | walmart         | product based   |
    |          2 | virat         | kohli         | flipkart        | product based   |
    |          3 | shikhar       | dhawan        | NULL            | invalid company |
    |          4 | rahul         | dravid        | walmart         | product based   |
    |          5 | kapil         | dev           | microsoft       | product based   |
    |          6 | brian         | lara          | tcs             | service based   |
    |          7 | cart          | hooper        | wipro           | service based   |
    |          8 | saurabh       | ganguly       | wipro           | service based   |
    +------------+---------------+---------------+-----------------+-----------------+
    8 rows in set (0.01 sec)
    ```
    

### Session 11 [JOINS](https://www.w3schools.com/mysql/mysql_join.asp)

- **INNER JOIN**
    
    By default, MySQL considers `JOIN` as the  [INNER JOIN](https://www.w3schools.com/mysql/mysql_join_inner.asp) (click on the link for proper explanation)
    
    ```sql
    mysql> SELECT s.student_fname, c.course_name 
    FROM students s 
    **JOIN courses c ON s.selected_course=c.course_id;**
    +---------------+-----------------+
    | student_fname | course_name     |
    +---------------+-----------------+
    | rohit         | web development |
    | virat         | big data        |
    | shikhar       | data science    |
    | rahul         | big data        |
    | kapil         | big data        |
    | brian         | big data        |
    | cart          | big data        |
    | saurabh       | big data        |
    +---------------+-----------------+
    8 rows in set (0.00 sec)
    ```
    
    INNER JOIN gives just the matching records, the intersection between the two tables.
    
- **LEFT JOIN**
    
    All matching records (Intersection) + the records present only in the LEFT table and not in Right, **padded with NULL.**
    
    ```sql
    mysql> SELECT c.course_id, c.course_name, s.student_fname, s.student_company 
    FROM courses c 
    **LEFT JOIN students s ON c.course_id=s.selected_course** 
    ORDER BY c.course_id DESC;
    +-----------+---------------------+---------------+-----------------+
    | course_id | course_name         | student_fname | student_company |
    +-----------+---------------------+---------------+-----------------+
    |         5 | data engineering    | NULL          | NULL            |
    |         4 | android development | NULL          | NULL            |
    |         3 | data science        | shikhar       | NULL            |
    |         2 | web development     | rohit         | walmart         |
    |         1 | big data            | brian         | tcs             |
    |         1 | big data            | rahul         | walmart         |
    |         1 | big data            | virat         | flipkart        |
    |         1 | big data            | cart          | wipro           |
    |         1 | big data            | kapil         | microsoft       |
    |         1 | big data            | saurabh       | wipro           |
    +-----------+---------------------+---------------+-----------------+
    10 rows in set (0.00 sec)
    ```
    
    Here the LEFT table is `courses` and the right becomes `students` . As expected, we can see NULL values for student_fname and student_company as there are 0 records satisfying that condition in the students table.
    
- ********************RIGHT JOIN********************
    
    All matching records (Intersection) + the records present only in the RIGHT table and not in LEFT, **padded with NULL.**
    
    ```sql
    mysql> SELECT c.course_id, c.course_name, s.student_fname, s.student_company 
    FROM students s 
    **RIGHT JOIN courses c ON c.course_id=s.selected_course** 
    ORDER BY c.course_id DESC;
    +-----------+---------------------+---------------+-----------------+
    | course_id | course_name         | student_fname | student_company |
    +-----------+---------------------+---------------+-----------------+
    |         5 | data engineering    | NULL          | NULL            |
    |         4 | android development | NULL          | NULL            |
    |         3 | data science        | shikhar       | NULL            |
    |         2 | web development     | rohit         | walmart         |
    |         1 | big data            | brian         | tcs             |
    |         1 | big data            | rahul         | walmart         |
    |         1 | big data            | virat         | flipkart        |
    |         1 | big data            | cart          | wipro           |
    |         1 | big data            | kapil         | microsoft       |
    |         1 | big data            | saurabh       | wipro           |
    +-----------+---------------------+---------------+-----------------+
    10 rows in set (0.00 sec)
    ```
    
    Here the RIGHT table is `courses` and the LEFT becomes `students` . 
    
    The output is same as the LEFT JOIN because I have interchanged the table name for demonstrating RIGHT JOIN.
    As expected, we can see NULL values for student_fname and student_company as there are 0 records satisfying the given condition in the students table.
    

### Session 12 WHERE v/s HAVING

In simple words, HAVING is used to apply a filter on the GROUP BY while, the WHERE is used to filter out records before Grouping

```sql
mysql> SELECT source_of_joining, COUNT(*) no_of_students 
FROM students 
GROUP BY source_of_joining ;
+-------------------+----------------+
| source_of_joining | no_of_students |
+-------------------+----------------+
| friend            |              1 |
| google            |              2 |
| linkedin          |              2 |
| quora             |              1 |
| youtube           |              2 |
+-------------------+----------------+
5 rows in set (0.00 sec)
```

Let’s say we have a problem statement where we need to print only those source_of_joining having more than 1 students. 
LOL, in the problem statement itself, you got the hint that we have to use HAVING here.

```sql
mysql> SELECT source_of_joining, COUNT(*) no_of_students 
FROM students 
GROUP BY source_of_joining 
**HAVING no_of_students > 1;**
+-------------------+----------------+
| source_of_joining | no_of_students |
+-------------------+----------------+
| google            |              2 |
| linkedin          |              2 |
| youtube           |              2 |
+-------------------+----------------+
3 rows in set (0.02 sec)
```

So, can we use WHERE and HAVING in the same query? Yes! and we should. 
WHERE should be used to perform any filter operation before the Grouping, then HAVING can be used to filter the Grouped result, if needed.

**************************Ex: Get the locations from which more than 1 student has joined & the student’s experience is more than 5 years.**************************

```sql
mysql> SELECT location, COUNT(*) no_of_students_exp_gt5
FROM students WHERE years_of_exp > 5 
GROUP BY location HAVING no_of_students_exp_gt5 > 1;

+-----------+------------------------+
| location  | no_of_students_exp_gt5 |
+-----------+------------------------+
| bangalore |                      2 |
| chennai   |                      2 |
| pune      |                      3 |
+-----------+------------------------+
3 rows in set (0.00 sec)
```

### Session 13 OVER & PARTITION BY

Using [onecompiler](https://onecompiler.com/mysql/)

[Session 13 queries.txt](Learning%20SQL,%20My%20way%E2%80%A6%203cd0f52f4a6d459d8734814cf27d7657/Session_13_queries.txt)

**Q: How many people are from each location and the avg salary.**

```sql
SELECT location, COUNT(*) no_of_employee, AVG(salary) avg_salary 
FROM employee 
GROUP BY location;

location		no_of_employee		avg_salary
bangalore			3				        16666.6667
hyderabad			2				        27500.0000
pune				  2				        12500.0000
```

**Q: Let’s say we have a problem statement where we need the above result for each firstname & lastname in the employee table, something like the below.**

```sql
firstname	 lastname	location	no_of_employee	avg_salary
sachin	   sharma	  bangalore	     3	         16666.6667
shane	     warne	  bangalore	     3	         16666.6667
rohit	     sharma	  hyderabad	     2	         27500.0000
shikhar	   dhawan	  hyderabad	     2	         27500.0000
rahul	     dravid	  bangalore	     3	         16666.6667
saurabh	   ganguly	pune	         2	         12500.0000
kapil	     dev	    pune	         2	         12500.0000
```

We can achieve this by using JOIN, by joining the tables `employee` and the aggregated table obtained in the previous question.

```sql
SELECT firstname, lastname, e.location, no_of_employee, avg_salary
FROM employee e
JOIN (
SELECT location, COUNT(*) no_of_employee, AVG(salary) avg_salary
FROM employee GROUP BY location
) e2
ON e.location = e2.location;

firstname	 lastname	location	no_of_employee	avg_salary
sachin	   sharma	  bangalore	     3	         16666.6667
shane	     warne	  bangalore	     3	         16666.6667
rohit	     sharma	  hyderabad	     2	         27500.0000
shikhar	   dhawan	  hyderabad	     2	         27500.0000
rahul	     dravid	  bangalore	     3	         16666.6667
saurabh	   ganguly	pune	         2	         12500.0000
kapil	     dev	    pune	         2	         12500.0000
```

Now the problem here is that JOINS are not that performant as compared to OVER clause. We can use a syntactic sugar to obtain the same result…using OVER and PARTITION BY.

```sql
SELECT firstname, lastname, location,
**COUNT(location) OVER (PARTITION BY location) total_emp,
AVG(salary) OVER (PARTITION BY location) avg_salary**
FROM employee;

**firstname	 lastname	 location	  total_emp	 avg_salary**
**sachin	   sharma	   bangalore	  3	       16666.6667
shane	     warne	   bangalore	  3	       16666.6667
rahul	     dravid	   bangalore	  3	       16666.6667
rohit	     sharma	   hyderabad	  2	       27500.0000
shikhar	   dhawan	   hyderabad	  2	       27500.0000
saurabh	   ganguly	 pune	        2	       12500.0000
kapil	     dev	     pune	        2	       12500.0000**
```

See, it’s a lot simpler and better looking too, easy to read. We didn’t even have to use GROUP BY

### Session 14 ROW_NUMBER()

- Whenever we use ROW_NUMBER(), there must be ORDER BY in the OVER clause.
- As per the problem statement, we can even use PARTITION BY inside the OVER clause.
- The row number starts from **1** for every new Partition.

```sql
SELECT firstname, lastname, salary,
**ROW_NUMBER() OVER (ORDER BY salary DESC) AS salary_rank**
FROM employee;

**firstname	 lastname	 salary	 salary_rank
rohit	      sharma	 30000	    1
shikhar	    dhawan	 25000	    2
shane	      warne	   20000	    3
rahul	      dravid	 20000	    4
saurabh	    ganguly	 15000	    5
sachin	    sharma	 10000	    6
kapil	      dev	     10000	    7**
```

**Q: Find the 5th highest salary.**

```sql
SELECT firstname, lastname, age, salary, location 
FROM (
SELECT firstname, lastname, age, salary, location,
ROW_NUMBER() OVER(ORDER BY salary DESC) AS salary_rank
FROM employee
) AS temp_table
WHERE temp_table.salary_rank = 5;

**firstname	 lastname	 age	 salary	 location
saurabh	    ganguly	  32	 15000	   pune**
```

**Q: Assign Row number Over `salary` for each Partition based on `location`**

```sql
SELECT firstname, lastname, location, salary,
ROW_NUMBER() OVER (PARTITION BY location ORDER BY salary DESC) AS salary_rank
FROM employee;

**firstname	  lastname	 location	  salary	salary_rank**
shane	      warne	     bangalore	20000	      1
rahul	      dravid	   bangalore	20000	      2
sachin	    sharma	   bangalore	10000	      3
rohit	      sharma	   hyderabad	30000	      1
shikhar	    dhawan	   hyderabad	25000	      2
saurabh	    ganguly	   pune	      15000	      1
kapil	      dev	       pune	      10000	      2
```

**Q: Find the highest salary getters at each location.**

```sql
SELECT firstname, lastname, location, salary
FROM (
SELECT firstname, lastname, location, salary,
ROW_NUMBER() OVER (PARTITION BY location ORDER BY salary DESC) as salary_rank
FROM employee) AS e
WHERE e.salary_rank = 1 ORDER BY e.salary DESC;

**firstname	  lastname	 location	  salary**
rohit	      sharma	   hyderabad	30000
shane	      warne	     bangalore	20000
saurabh	    ganguly	   pune	      15000
```

### Session 15 RANK and DENSE RANK

```sql
SELECT firstname, lastname, salary,
**ROW_NUMBER() OVER (ORDER BY salary DESC) AS salary_rank**
FROM employee;

**firstname	 lastname	 salary	 salary_rank
rohit	      sharma	 30000	    1
shikhar	    dhawan	 25000	    2
shane	      warne	   20000	    3
rahul	      dravid	 20000	    4
saurabh	    ganguly	 15000	    5
sachin	    sharma	 10000	    6
kapil	      dev	     10000	    7**
```

In the above result, there is one problem, even though the salary of shane and rahul is the same, 20000, the rank for rahul dravid is set as 4 (same with sachin and kapil). 
This doesn’t make sense in a real-world scenario, does it?

To tackle this case and to assign proper ranks MySQL provides two other functions similar to ROW_NUMBER(), **RANK()** and **DENSE_RANK().** 

- ************RANK()
Skips ranks in between.************
    
    ```sql
    SELECT firstname, lastname, salary,
    **RANK()** OVER (ORDER BY salary DESC) as salary_rank
    FROM employee;
    
    **firstname	 lastname	 salary	 salary_rank
    rohit	      sharma	 30000	    1
    shikhar	    dhawan	 25000	    2
    shane	      warne	   20000	    3
    rahul	      dravid	 20000	    3
    saurabh	    ganguly	 15000	    5
    sachin	    sharma	 10000	    6
    kapil	      dev	     10000	    6**
    ```
    
    Now we are able to fix that issue with the ROW_NUMBER(), at least to some extent.
    
    Still, there is a problem here, the ranks for shane-rahul and sachin-kapil is same, which is correct but the rank of saurabh is 5, MySQL or RANK() skipped the rank 4. Which again doesn’t seem right. So now we will use DENSE_RANK().
    
- ******DENSE_RANK()
Doesn’t skip ranks in between.******
    
    ```sql
    SELECT firstname, lastname, salary,
    **DENSE_RANK()** OVER (ORDER BY salary DESC) AS salary_rank
    FROM employee;
    
    **firstname	 lastname	 salary	 salary_rank
    rohit	      sharma	 30000	    1
    shikhar	    dhawan	 25000	    2
    shane	      warne	   20000	    3
    rahul	      dravid	 20000	    3
    saurabh	    ganguly	 15000	    4
    sachin	    sharma	 10000	    5
    kapil	      dev	     10000	    5**
    ```
    

## Advanced SQL

### CTE - Common Table Expression

```sql
-- Syntax 1:
mysql> WITH table_name (column_names) AS (SELECT column FROM other table)
-> SELECT * FROM table_name;

--Syntax 2:
mysql> WITH table_one AS (SELECT query),
table_two AS (SELECT query),
table_three AS (SELECT query)...
SELECT * FROM table_three;
```

It works similarly to the sub-query but is easy to read and is good in performance.

**IMP:**

- CTE or WITH doesn’t create a new table `table_name` it just keeps the result in memory and as soon as the next statement is executed and completed, the program deletes it from the memory.
- CTEs define a temporary result set that is held in memory until its purpose is fulfilled.
- We need to use SELECT, INSERT, UPDATE, etc, immediately after the WITH clauses.
- CTEs can improve performance, but not always.