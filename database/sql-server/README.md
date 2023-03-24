## Deploy MS-SQL server on your local machine with docker conatiner.

### Step1:
Deploy SQL server using the docker-compose file.
    
    `docker-compose up -d`

### Step2:
Create table using SQLCMD using the inventory.sql file or updaete the query as required.
    
    -- Create the test database
    CREATE DATABASE testDB;
    GO
    USE testDB;
    EXEC sys.sp_cdc_enable_db;

    -- Create some customers ...
    CREATE TABLE customers (
    id INTEGER IDENTITY(1001,1) NOT NULL PRIMARY KEY,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE
    );
    INSERT INTO customers(first_name,last_name,email)
        VALUES ('Sally','Thomas','sally.thomas@acme.com');
    INSERT INTO customers(first_name,last_name,email)
        VALUES ('George','Bailey','gbailey@foobar.com');
    INSERT INTO customers(first_name,last_name,email)
        VALUES ('Edward','Walker','ed@walker.com');
    INSERT INTO customers(first_name,last_name,email)
        VALUES ('Anne','Kretchmar','annek@noanswer.org');
    EXEC sys.sp_cdc_enable_table @source_schema = 'dbo', @source_name = 'customers', @role_name = NULL, @supports_net_changes = 0;
    GO

Execute the below command to create the database and enable the CDC.

    `cat inventory.sql | docker exec -i sql-server bash -c '/opt/mssql-tools/bin/sqlcmd -U sa -P YOUR_PASSWORD'`