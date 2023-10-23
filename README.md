
![image](https://github.com/jil1710/readmedemo/assets/125335932/15e324dd-b534-4075-9f28-3fb2fa4ee99c)



# Table Partitioning

- Partitioning is a technique that splits a large table into smaller, more manageable pieces, based on some criteria. For example, you can partition a table by date, region, or category. Partitioning can improve the performance, availability, and maintainability of your database.

- Table partitioning allows you to store the data of a table in multiple physical sections or partitions. Each partition has the same columns but different set of rows.

## Types of table partitioning

 - **Vertical Partitioning:** Vertical table partitioning is mostly used to increase SQL Server performance especially in cases where a query retrieves all columns from a table that contains a number of very wide text or BLOB columns. In this case to reduce access times the BLOB columns can be split to its own table. Another example is to restrict access to sensitive data e.g. passwords, salary information etc. Vertical partitioning splits a table into two or more tables containing different columns:

   ![image](https://github.com/jil1710/readmedemo/assets/125335932/00a56bc4-d6b1-490c-ba87-5740c1487d88)

 - **An example of vertical partitioning**

    - An example for vertical partitioning can be a large table with reports for employees containing basic information, such as report name, id, number of report and a large column with report description. Assuming that ~95% of users are searching on the part of the report name, number, etc. and that only ~5% of requests are opening the reports description field and looking to the description. Let’s assume that all those searches will lead to the clustered index scans and since the index scan reads all rows in the table the cost of the query is proportional to the total number of rows in the table and our goal is to minimize the number of IO operations and reduce the cost of the search.
  
    - Let’s see the example on the Reports table:

      ![image](https://github.com/jil1710/readmedemo/assets/125335932/7cde9c8d-e174-48a5-9fe2-e021136eebe4)
      
      If we run a SQL query to pull ReportID, ReportName, ReportNumber data from the Reports table the result set that a scan count is 5 and represents a number of times that the table was accessed during the query, and that we had 113,288 logical reads that represent the total number of page accesses needed to process the query:

      ![image](https://github.com/jil1710/readmedemo/assets/125335932/10ec198d-7832-4f99-a1fc-1fb1ec7ea66f)

      As indicated, every page is read from the data cache, whether or not it was necessary to bring that page from disk into the cache for any given read. To reduce the cost of the query we will change the SQL Server database schema and split the Reports table vertically.

      ![image](https://github.com/jil1710/readmedemo/assets/125335932/7d7c08e9-3afd-4152-aea3-0122b80a9f33)

      The same search query will now give different results:

      ![image](https://github.com/jil1710/readmedemo/assets/125335932/8cf7eaac-2d97-4176-ae12-2745b0841b28)

      Vertical partitioning on SQL Server tables may not be the right method in every case. However, if you have, for example, a table with a lot of data that is not accessed equally, tables with data you want to restrict access to, or scans that return a lot of data, vertical partitioning can help.



 - **Horizontal Partitioning:** Horizontal partitioning divides a table into multiple tables that contain the same number of columns, but fewer rows. For example, if a table contains a large number of rows that represent monthly reports it could be partitioned horizontally into tables by years, with each table representing all monthly reports for a specific year. This way queries requiring data for a specific year will only reference the appropriate table. Tables should be partitioned in a way that queries reference as few tables as possible.

   ![image](https://github.com/jil1710/readmedemo/assets/125335932/3d8217b3-372a-44f5-b955-7ee9df4fb99c)

   Tables are horizontally partitioned based on a column which will be used for partitioning and the ranges associated to each partition. Partitioning column is usually a datetime column but all data types that are valid for use as index columns can be used as a partitioning column, except a timestamp column. The ntext, text, image, xml, varchar(max), nvarchar(max), or varbinary(max), Microsoft .NET Framework common language runtime (CLR) user-defined type, and alias data type columns cannot be specified.

   There are two different approaches we could use to accomplish table partitioning. The first is to create a new partitioned table and then simply copy the data from your existing table into the new table and do a table rename. The second approach is to partition an existing table by rebuilding or creating a clustered index on the table.

   - **An example of horizontal partitioning with creating a new partitioned table**

     To create a partitioned table for storing monthly reports we will first create additional filegroups. A filegroup is a logical storage unit. Every database has a primary filegroup that contains the primary data file (.mdf). An additional, user-defined, filegrups can be created to contain secondary files (.ndf). We will create 12 filegroups for every month:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/d3fcbf1e-fdab-491f-aaaf-0e0b3ecc1b04)

     To check created and available file groups in the current database run the following query:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/050b2b54-c535-4564-898b-82834f278ee3)

     When filegroups are created we will add .ndf file to every filegroup:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/d0b4a633-bf5e-4193-849b-a7951e8fd019)

     lly, you can add for each file group you created above.

     To check files created added to the filegroups run the following query:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/5cb195c3-0dad-4175-aa0f-1bcc8be7e417)

     After creating additional filegroups for storing data we’ll create a partition function. A partition function is a function that maps the rows of a partitioned table into partitions based on the values of a partitioning column. In this example we will create a partitioning function that partitions a table into 12 partitions, one for each month of a year’s worth of values in a datetime column:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/e61cbf0c-935b-4316-bdb4-7cdc4593212f)

     To map the partitions of a partitioned table to filegroups and determine the number and domain of the partitions of a partitioned table we will create a partition scheme:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/4867dc66-5c97-456b-972b-726347a1cc82)

     Now we’re going to create the table using the PartitionBymonth partition scheme, and fill it with the test data:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/f95318ba-6a72-4c3a-8af7-6f82172876b4)
     
     We will now verify the rows in the different partitions:

     ![image](https://github.com/jil1710/readmedemo/assets/125335932/27b7b796-d71b-4b55-a3ed-12b43de69aa7)

# Clustered &  Non-Clustered Key in Sql Server

- The goal of the index is to make the search operation faster.Indexes make the search operation faster by creating something called a B-Tree (Balanced Tree) structure internally.

- Whenever you create an index (or indexes) on some column(s) of a table in SQL Server then what happens internally is, it creates a B-Tree structure. In the B-Tree structure, the data is divided into three sections i.e. Root Node, Non-Leaf Nodes, and Leaf Nodes. In order to understand this better please have a look at the following image which shows how the data is divided and stored. As you can see, in the Root Node it has 30 and 50. In the Non-Leaf node, it has 10, 30, 40, and 50. And in the leaf node, we have the actual data. So, the leaf node is actually pointing to data.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/338d1af5-ff92-4bbc-81ed-065d6e542bfd)

- Suppose, you want to search 50 here, then what will happen internally is, the search engine will start the search from the root node. It will check whether 50 is less than or equal to 30. As 50 is not less than or equal to 30, so the non-leaf nodes and leaf nodes that come under the root node 30 are completely bypassed.

- Then it will go to the next node i.e. 50 and check whether 50 is less than or equal to 50. And the condition satisfies here. Then it goes to the non-leaf nodes (40, 50) which are under the root node 50. It will check whether 50 is less than or equal to 40 and the condition fail, so, it will bypass all the leaf nodes which come under the non-leaf node 40. Then it will check the other non-leaf node i.e. 50 and here the condition satisfies as 50 equals 50 and it goes to scan the leaf node sequentially. That is, it approximately scans 10 records.


- **Non Clustered Key :** In the Non-Clustered Index, the arrangement of data in the index table will be different from the arrangement of data in the actual table. The data is stored in one place and the index table is stored in another place. Moreover, the index table will have pointers to the storage location of the actual data.

  - In order to understand SQL Server Non-Clustered Indexes in a better way, please have a look at the following diagram which shows the pictorial representation of the B-Tree structure of a non-clustered index in SQL Server. Both clustered and non-clustered index has the same B-Tree structure (i.e. having the Root Node, Intermediate Node (Non-Leaf Node), and Leaf Nodes). The only difference between the clustered and non-clustered index is how the leaf nodes are worked. In the case of a Clustered Index, the leaf node holds the actual data. On the other hand, the non-clustered index leaf node has a Row Identifier (RID), and this Row ID points to different things based on the situation.
 
    ![image](https://github.com/jil1710/readmedemo/assets/125335932/9636720d-e280-42d4-b16a-3ea8730e0fc6)

  - If your table has a clustered index, then the Row Identifier (RID) of the non-clustered index will point to the clustered index key and that indexed key is used to search the data. If your table does not have a clustered index, then the Row Identifier (RID) of the non-clustered index points to the heap table. A heap table is nothing but a table without indexes. In the heap table, it will search the record row by row until it finds the data.
 
  - As the non-clustered indexes are created separately from the actual data, so, a table can have more than one non-clustered index in SQL Server. In the index table, the data is stored either in the ascending or descending order of the index key which does not make any effect or changes to the actual data stored in the table. In SQL Server, a maximum of 999 non-clustered indexes are created per table
 
    ```sql
      CREATE TABLE tblOrder
      (
          Id INT,
          CustomerId INT,
          ProductId Varchar(100),
          ProductName VARCHAR(50)
      )
      GO
    ```
  - Now insert some data into the table. In order to do this, here, we are inserting some mock data using a loop. Please execute the following query.

    ```sql
      DECLARE @i int = 0
      WHILE @i < 3000 
      BEGIN
          SET @i = @i + 1
          IF(@i < 500)
          Begin
              INSERT INTO tblOrder VALUES (@i, 1, 'Product - 10120', 'Laptop')
          END
          ELSE IF(@i < 1000)
          Begin
              INSERT INTO tblOrder VALUES (@i, 3, 'Product - 1020', 'Mobile')
          End
          Else if(@i < 1500)
          Begin
              INSERT INTO tblOrder VALUES (@i, 2, 'Product - 101', 'Desktop')
          End
          Else if(@i < 2000)
          Begin
              INSERT INTO tblOrder VALUES (@i, 3, 'Product - 707', 'Pendrive')
          End
          Else if(@i < 2500)
          Begin
              INSERT INTO tblOrder VALUES (@i, 2, 'Product - 999', 'HD')
          End
          Else if(@i < 3000)
          Begin
              INSERT INTO tblOrder VALUES (@i, 1, 'Product - 100', 'Tablet')
          End
      END
    ```

  - Now, execute the following query to get the data by ProductId.
    ```sql
     SELECT * FROM tblOrder WHERE ProductId = ‘Product – 101’;
    ```
    After executing the above SQL Query, let us click on the Display Estimated Execution Plan button or simply click (Ctrl + L) which will open the Display Estimated Execution Plan window as shown below.

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/eb89fbd7-a1c3-4353-99d9-3f0eaf831168)

  - Creating Non-clustered Index in SQL Server:
 
    ```sql
       CREATE NONCLUSTERED INDEX IX_tblOrder_ProductId
       ON dbo.tblOrder (ProductId)
       INCLUDE ([Id],[CustomerId],[ProductName])
       GO
    ```

    - Once you created the non-clustered index, now execute the following query and check the execution plan.
      ```sql
       SELECT * FROM tblOrde WHERE ProductId = ‘Product – 101’;
      ```
      Following is the execution plan. As you can see, it now uses a non-clustered index to get the required data.

      ![image](https://github.com/jil1710/readmedemo/assets/125335932/83f006c2-765d-4fb2-9fe3-641114dc9eb3)

  - **Clustered Index in Sql Server :** The Clustered Index in SQL Server defines the order in which the data is physically stored in a table. That is the leaf node (Ref to the B-Tree Structure diagram) stores the actual data. As the leaf nodes store the actual data, a table can have only one clustered index. The Clustered Index by default was created when we created the primary key constraint for that table. That means the primary key column creates a clustered index by default.

    - When a table has a clustered index then the table is called a clustered table. If a table has no clustered index, its data rows are stored in an unordered structure.
   
    - **Default Clustered Indexes**

      - Let’s create a dummy table with primary key column to see the default clustered index. Execute the following script:
      ```sql
         CREATE DATABASE Hospital

         CREATE TABLE Patients
         (
         id INT PRIMARY KEY,
         name VARCHAR(50) NOT NULL,
         gender VARCHAR(50) NOT NULL,
         age INT NOT NULL
         )
      ```
      - The above script creates a dummy database Hospital. The database has 4 columns: id, name, gender, age. The id column is the primary key column. When the above script is executed, a clustered index is automatically created on the id column. To see all the indexes in a table, you can use the “sp_helpindex” stored procedure.
        ```sql
          EXECUTE sp_helpindex Patients
        ```

        ![image](https://github.com/jil1710/readmedemo/assets/125335932/cab929bb-2d14-4ff8-8933-5870a2c059e7)

      - You can see the index name, description and the column on which the index is created. If you add a new record to the Patients table, it will be stored in ascending order of the value in the id column. If the first record you insert in the table has an id of three, the record will be stored in the third row instead of the first row since clustered index maintains physical order.

     - **Custom Clustered Indexes :** You can create your own clustered indexes. However, before you can do that you have to create the existing clustered index. We have one clustered index due to primary key column. If we remove the primary key constraint, the default cluster will be removed. The following script removes the primary key constraint.

       ```sql
           ALTER TABLE Patients
           DROP CONSTRAINT PK__Patients__3213E83F3DFAFAAD
           GO
       ```

       - The following script creates a custom index “IX_tblPatient_Age” on the age column of the Patients table. Owing to this index, all the records in the Patients table will be stored in ascending order of the age.

         ```sql
            CREATE CLUSTERED INDEX IX_tblPatient_Age ON Patients(age ASC)
         ```

       - Now insert data into `Paitents` table
         ```sql
           INSERT INTO Patients 
           VALUES
           (1, 'Sara', 'Female', 34),
           (2, 'Jon', 'Male', 20),
           (3, 'Mike', 'Male', 54),
           (4, 'Ana', 'Female', 10),
           (5, 'Nick', 'Female', 29)
         ```
         - In the above script, we add 5 dummy records. Notice the values for the age column. They have random values and are not in any logical order. However, since we have created a clustered index, the records will be actually inserted in the ascending order of the value in the age column. You can verify this by selecting all the records from the Patients table.

           ![image](https://github.com/jil1710/readmedemo/assets/125335932/2139388f-c537-4c76-9394-23104c8361a3)

   - **When to Use Clustered or Non-Clustered Indexes**

     - Number of Indexes

       This is pretty obvious. If you need to create multiple indexes on your database, go for non-clustered index since there can be only one clustered index.

     - SELECT Operations

       If you want to select only the index value that is used to create and index, non-clustered indexes are faster. For example, if you have created an index on the “name” column and you want to select only the name, non-clustered indexes will quickly return the name. However, if you want to select other column values such as age, gender using the name index, the SELECT operation will be slower since first the name will be searched from the index and then the reference to the actual table record will be used to search the age and gender. On the other hand, with clustered indexes since all the records are already sorted, the SELECT operation is faster if the data is being selected from columns other than the column with clustered index.

     - INSERT/UPDATE Operations

       The INSERT and UPDATE operations are faster with non-clustered indexes since the actual records are not required to be sorted when an INSERT or UPDATE operation is performed. Rather only the non-clustered index needs updating.

     - Disk Space
       
       Since, non-clustered indexes are stored at a separate location than the original table, non-clustered indexes consume additional disk space. If disk space is a problem, use a clustered index.


# Execution Plan caching

- Whenever a query is run for the first time in SQL Server, it is compiled and a query plan is generated for the query. Every query requires a query plan before it is actually executed. This query plan is stored in SQL Server query plan cache. This way when that query is run again, SQL Server doesn’t need to create another query plan; rather it uses the cached query plan which improved database performance.

- The duration that a query plan stays in the plan cache depends upon how often a query is executed. Query plans that are used more often, stay in the query plan cache for longer durations, and vice-versa.

- Now somehow, if we ensure that the first two steps (i.e. Syntax Checked and Plan Selected) are executed only once, would not it be great. In other words, the first time the SQL is executed, the syntaxes are checked, the execution plan is selected and the execution plan is cached in memory. So, if the same SQL statements are fired again, then these two steps are not going to be executed, rather the execution plan is taken from the cache and executed and that will definitely increase the performance of the application which is shown in the below image.

  ![image](https://github.com/jil1710/readmedemo/assets/125335932/c966463a-d81e-4165-b0de-fd3e32ecedac)


- **How to view the SQL Server query plan cache**

- SQL Server provides the following dynamic management views and functions that can be used to find out what is in the plan cache at any given time.

  - sys.dm_exec_cached_plans
  - sys.dm_exec_sql_text
  - sys.dm_exec_query_plan

 - Let us use these functions and views to see what is in the SQL Server cached query plan. Execute the following query on your SSMS (SQL Server Management Studio):

   ![image](https://github.com/jil1710/readmedemo/assets/125335932/cff92b5e-acff-4cd2-9945-8093fa3163b0)

   ![image](https://github.com/jil1710/readmedemo/assets/125335932/1c951a6b-4f20-4c89-9147-9c602a26bfa8)

 - Here the usecount column contains a count for the number of times a query has been executed. The objtype column contains information about the object through which a query is executed. It is important to mention that up till SQL Server 6.5 only stored procedure queries were stored in the cached plan. From SQL Server 7.0 onwards, dynamic and ad-hoc queries are also stored in the cached plan. The text column contains the text of the query and finally, the query_plan column contains the XML representation of the query. Click any row in the query_plan column to see the detailed XML representation of the plan.

 - **Clearing the plan cache**

   - To clear the plan cache, execute the following:
     ```sql
       DBCC FREEPROCCACHE
     ```

  - **Undestand Stored procedure query plan**

     - Now let’s execute a simple stored procedure and see what we get in our SQL Server query plan cache. First let’s create a dummy database and a table inside that database:

       ![image](https://github.com/jil1710/readmedemo/assets/125335932/455f0341-8705-4c8f-b840-1fa2109fdee7)

       ![image](https://github.com/jil1710/readmedemo/assets/125335932/68825b03-ff3e-46b7-bb30-b89ddf05ae01)

    - Finally, let’s create a simple stored procedure that retrieves all the records from the department table of the company database:
   
      ![image](https://github.com/jil1710/readmedemo/assets/125335932/43f689ff-497a-431f-a0ca-98b5473f402e)

    - Now, execute the newly created getdepartment stored procedure:
      ```sql
       exec getdepartment
      ```

    - Once the stored procedure is executed, try to retrieve the information about all the query plans in the plan cache, again execute the following query:

      ![image](https://github.com/jil1710/readmedemo/assets/125335932/06d96756-a4d1-4ef0-b653-379184523f85)
   
    - When the above query is executed, you will see that two query plans will be retrieved: One query plan for the stored procedure and the other for the query that retrieves the query plan. The output looks like this:

      ![image](https://github.com/jil1710/readmedemo/assets/125335932/ee7bcccb-60b1-42da-bb85-45e255ffaade)

    - In the first row, you can see the object type Proc. This refers to the stored procedure query plan that we just executed. Inside the text column, you can also see the information about the stored procedure and the actual query that we executed inside the stored procedure. Here the usecount column displays the number of times query is executed. Execute the stored procedure once more and then retrieve the query plan cache, and you will see following results:
   
      ![image](https://github.com/jil1710/readmedemo/assets/125335932/4450abce-55f1-4080-94b1-d9cf5d473f83)

   - The Stored Procedures are pre-compiled and their execution plan is cached and used again when the same stored procedure is executed again. The Stored Procedure reduces network traffic. When we execute a stored procedure we need to send the procedure name and parameters so only these things are passed on the network but if we are not using the stored procedure then we need to write the ad-hoc queries and we need to execute them which may contain many numbers of lines. So the stored procedure reduces the network traffic as a result performance of the application increase.


- **The query plan depends upon the query text**

  - SQL Server generates a query plan using a hash value that is calculated from the query text. When a query is run, SQL Server calculates its hash value and checks if a plan with the same hash value exists in the plan cache. If a plan with same hash value exists, that plan is executed. However, if a plan with the newly calculated hash value doesn’t exist, a new query plan is generated and stored in the cache plan.

  - The hash value for the query plan is generated from the text. Therefore if there is even a slight change in the query text (e.g. a change of case, comma or space) a new hash value will be generated and thus a new query plan. Let’s see an example of this.
 
    Clear the query plan cache and execute the following query twice:
    ```sql
      SELECT * FROM department where dep_name = 'Sales'
    ```

  - When you retrieve the query plan cache, you can see that usecount for the Adhoc query that we just executed twice is 2:

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/a2604dea-17be-4d65-af85-74879050337b)

    - Now let’s make a slight change to our query:
      ```sql
       SELECT * FROM department Where dep_name = 'Sales'
      ```

    - Here we have replaced small case w of the where clause from the previous query to capital case W. This is a minor change, but when the above query is executed a new hash value is calculated and hence a new query plan is generated for the query. If you look at the query cash plan now, you will see following output:
   
      ![image](https://github.com/jil1710/readmedemo/assets/125335932/4afb2999-3149-47cc-946a-fedca6f213d2)


- **Review of Execution Plan Reuse**

  - SQL query optimization is both a resource and time intensive process. An execution plan provides SQL Server with instructions on how to efficiently execute a query and must be available prior to execution and is the product of the query optimization process whenever a query is executed.
  
  - Because it takes significant resources to generate an execution plan, SQL Server caches plans in memory in the query plan cache for later use. If the same query is executed multiple times, then the cached plan can be reused over and over, without the need to generate a new plan. This saves time and resources, especially for common queries that are executed frequently.
  
  - Execution plans are cached based on the exact text of the query. Any differences, even those as minor as a comment or capital letter, will result in a separate plan being generated and cached. Consider the following two queries:

    ![image](https://github.com/jil1710/readmedemo/assets/125335932/69efe4be-98f6-4cbc-9e86-7a3452cda0d2)

  - While the queries are very similar and will likely require the same execution plan, SQL Server will create a separate plan for each. This is because the filter is different, with the OrderDate being May 30th in the first query and May 31st in the second query. As a result, hard-coded literals in queries will result in different execution plans for each different value that is used in the query. If I ran the query above once for every day in the year 2011, then the result would be 365 queries and 365 different cached execution plans.

 - If the queries above are executed very often, then SQL Server will be forced to generate new plans frequently for all possible values of OrderDate. If OrderDate is a DATETIME and can (and will) have lots of distinct values, then we’ll see a very large number of execution plans getting created at a rapid pace.

 - The plan cache is stored in memory and its size is limited by available memory. Therefore, if excessive numbers of plans are generated over a short period of time, the plan cache could fill up. When this occurs, older plans are removed from cache in favor of newer ones. If memory pressure becomes significant, then the older plans being removed may end up being useful ones that we will need soon.

 - The solution to memory pressure in the plan cache is `Parameterization`. For our query above, the DATETIME literal can be replaced with a parameter:

   ![image](https://github.com/jil1710/readmedemo/assets/125335932/a34bed61-384d-4770-a92b-53c9b9c6dd18)

 - When executed for the first time, an execution plan will be generated for this stored procedure that uses the parameter @order_date. All subsequent executions will use the same execution plan, resulting in the need for only a single plan, even if the proc is executed millions of times per day.

 - Parameterization greatly reduces churn in the plan cache and speeds up query execution as we can often skip the expensive optimization process that is needed to generate an execution plan.











     










   

