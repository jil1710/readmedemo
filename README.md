
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





   

