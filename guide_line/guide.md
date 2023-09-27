## create database

1. click new query then create database

    ```
    create database workshop;
    GO
    ```

2. create table workshop_member inside dbo schema

    table must have 

    - id int NOT NULL
    - name varchar(250) NOT NULL
    - first_name varchar(250) NOT NULL
    - email varchar(250) NOT NULL
    - phone int NOT NULL

3. insert data into workshop_member

    for example:
    ```
    INSERT into workshop_member (name,first_name,email,phone)
    VALUES ('mark','kanin','mark@gmail.com','0888888888'),
    ('tho','pipat','tho@gmail.com','0866666666'),
    ('feel','varit','feel@gmail.com','0877777777'),
    ('green','thanachan','green@gmail.com','0811111111');
    GO
    ```
    for sure we check table with

    ```
    select * from workshop_member;
    GO
    ```

4. masking data 

    we want to add masking function inside table

    ```
    ALTER TABLE workshop_member 
    ALTER COLUMN name ADD MASKED WITH (FUNCTION = 'default()');
    GO

    ALTER TABLE workshop_member 
    ALTER COLUMN first_name ADD MASKED WITH (FUNCTION = 'partial(0,"xx",1)')
    GO

    ALTER TABLE workshop_member 
    ALTER COLUMN email ADD MASKED WITH (FUNCTION = 'email()')
    GO

    ALTER TABLE workshop_member 
    ALTER COLUMN phone ADD MASKED WITH (FUNCTION = 'default()')
    GO
    ```

5. create user

    create user

    ```
    CREATE USER testuser WITHOUT LOGIN;

    GRANT SELECT ON SCHEMA::dbo TO testuser;
    ```

    use user 

    ```
    EXECUTE AS USER = 'testuser';
    ```

6. check data

    select to see data inside table

    ```
    SELECT * FROM workshop_member;
    ```

7. create more user for more permission

    create user staff and admin

    ```
    CREATE USER staff WITHOUT LOGIN;
    GO

    CREATE USER admin WITHOUT LOGIN;
    GO
    ```

    grant read permission

    ```
    ALTER ROLE db_datareader ADD MEMBER testuser;

    ALTER ROLE db_datareader ADD MEMBER staff;

    ALTER ROLE db_datareader ADD MEMBER admin;
    ```

    give different unmask permission
    ```
    GRANT UNMASK ON dbo.workshop_member(name) TO staff;

    GRANT UNMASK TO admin;
    ```

8. check result

    ```
    EXECUTE AS USER = 'testuser';

    select * from workshop_member;
    GO

    REVERT

    EXECUTE AS USER = 'staff';

    select * from workshop_member;
    GO

    REVERT

    EXECUTE AS USER = 'admin';

    select * from workshop_member;
    GO

    REVERT
    ```