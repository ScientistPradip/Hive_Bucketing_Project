# Hive ProjectII (Bucketing Data)


## Summary
This hands-on project explores Apache Hive and Hadoop HDFS for organizing and processing large datasets. The project focuses on Hive partitioning and bucketing, including static and dynamic partitioning, external and partitioned tables, and HiveQL queries. It also covers Text-to-Parquet conversion and the differences between row-based and columnar storage. The project was completed in a Docker and Linux environment, providing practical experience with how Hive tables and data are organized and stored in HDFS for efficient Big Data processing.

### Why Bucketing?
To Optimize the query performance it reduces shuffling because it helps you store the common data on common machine. Similar to partitioning that increases performance by reducing data scanning it also optmizes by reducing shuffling. 


## Technologies Used
- Apache Hive
- Hadoop HDFS
- HiveQL (SQL in Hive)
- Docker
- Linux Terminal
- CSV / Text
- Parquet file (compressed file supported in HDFS)

## Project Objectives
- Bucketing data (converting regular non partitioned data to bucketing data)
- Listing the advantage of data bucketing (helps in optimized query performance by reducing data shuffling as it stores data in single machine)
- Work with the Hadoop and Hive environment.
- Create and manage Hive databases.
- Create tables using different Hive data types.
- Use external tables with HDFS data.
- Define row format and field delimiters.
- Load and access data stored in HDFS.
- View table structures and column data types.
- Query data using HiveQL.
- Understand the relationship between Hive and HDFS.


## Step 1: Created input_table
After logging into docker desktop and entering hive console we entered into xyz database and created a input_table where we loaded data.


![Screenshot1](Screenshots_day16/Screenshot_(1).png)

## Step 2: Viewed data of input_table
Used select * command to view top 10 rows of the data contained in input_table

![Screenshot2](Screenshots_day16/Screenshot_(2).png)


## Step 3: Utilized set commands for bucketing and partitioning to activate them respectively

As we know set commands are mandatory as by default bucketing and partitioning data cannot be done. Manually data is stored  in non-partition way

And created table named 'bucket_table' after using set commands both for partitioning and bucketing. 

![Screenshot3](Screenshots_day16/Screenshot_(3).png)

## Step 4: Loading data for bucketing and partitioning
Reminder: Loading data for bucketing and partitioning is not possible without using set commands for both which we did in previous step. 

![Screenshot4](Screenshots_day16/Screenshot_(4).png)

## Step 5: Inserted data with bucket table including partitioned by city

After using this command

insert into table bucket_table partition(city) select street,zip,state,beds,baths,sq_feet,flat_type,price,city from input_table;

Viewed the partitions created (via partitioned by city) in 'bucket_table'.

![Screenshot5](Screenshots_day16/Screenshot_(5).png)

## Step 6: Viewing the created partitions in HDFS where its actually stored

Default storage of data created through hive is in this location /data/hive/warehouse/xyz.db that we just view

In Hive we keep working in data but behind the scene it is stored in Hadoop File System in this file location inside hive warehouse/ shown below. 

![Screenshot6](Screenshots_day16/Screenshot_(6).png)

If we also use below command to view the Antelope the file is stored in multiple folder/subfolders 
hadoop fs -ls /data/hive/warehouse/xyz.db/bucket_table/city=Wilton

![Screenshot6_II](Screenshots_day16/Screenshot_(6_II).png)

We'll view different files where data is partitioned in different files/folders with name 00000_00, 00001_00, 00002_00, 00003_00

How is this happening behind the Scene? (suppose there are 10 to 20 streets address in each city)
Here many partitions are created. We've created folder for the city within this city for every street it'll generate hash code take modulus(%) and % will be 0,1,3,4 because 4 files will be fcreated and according to the reminder data will be written to specific file and we can see that file using the command

Note: Bucketing Introduces extra files thats why its disabled by default to reduce the multiple no. of files creations that will increase the load in the name node

### But the Advantage  We get is that these buckets as multiple files is stored in the same machine so it reduces the data shuffling thats the reason we use the Bucketing


## Step 7: Viewed detailed information about our created bucket table
 Used show create table bucket_table command to see everything about the bucket table that we just created

 ![Screenshot7](Screenshots_day16/Screenshot_(7).png)

 ## Step 8: Viewed the input table, bucket table and partition table 

 Used show create table command to view input table, bucket table and partition table respectively. 

 ![Screenshot8](Screenshots_day16/Screenshot_(8).png)

## Step 9: Created partition table with the help of insert into command without creating temporary table

Let's try different way of creating a partition table. What we did before is we first created input table (non-partition table). From input table we loaded the data into bucketed table (bucketed table is also partitioned table)

But now, we want to create a partition table directly using insert into command

Viewed the partitions of employee table that is 'BIGDATA' and 'HR'

 ![Screenshot 10II](Screenshots_day16/Screenshot(10II).png)


## Step 10: Converted text format data/table to parquet format
While we create table/data using hive and store it in HDFS it is stored in text format by default that is row based storage, csv is another file format which is also row based storage

But now we'll be using parquet format which is compressed file formate that uses less size (its something similar like zip file/ which is also compressed file format).We didnt use zip because its not supported in HDFS but instead parquet is supported. 

Advantage of parquet file format
- small size
- data projection (reduce data scan)

Difference betn row based storage(text, csv) and column based storage (parquet format)
- parquet is not readable as its compressed while txt and csv are redable
- parquet is compressed and uses less size which reduces data scan that are also the advantage of using parquet file format
![Screenshot10v](Screenshots_day16/Screenshot(265).png)


created parquet table and inserted value from employee table(basically converting employee table that is in text format to parque table)

 ![Screenshot10iv](Screenshots_day16/Screenshot(10iv).png)

 Viewing the parque file in HDFS Location 

![Screenshot10vi](Screenshots_day16/Screenshot(266).png)
As you can see its compressed so its not viewable but the advantage we have is that its compressed and need less storage space and also reduce data scan (data projection)

## Step 11: Creating Static Partitioning

![Screenshot10vi](Screenshots_day16/Screenshot(267).png)

Here we created 3 directories: 1st directory we'll be using as a table location

Again we created 2 more folder year=2010 and year=2011 respectively saving/uploading file1 to hdfs (which we created using nano command) in year=2012 directory location.

Again Saving/uploading  file11 in year=2011 directory (in HDFS)

![Screenshot10vii](Screenshots_day16/Screenshot(269).png)

Number of folders depends on no. of records here we can view 2 folders 2010 with rahul and 2010 with null.
The reason we are'nt able to view data using select * from userdata is that external table points to historical data that is data cluld be huge in size cause its stored in multiple folders. So, by default it doesn't allow to read the complete table.

If we want to read we have to provide specific folder using where clause (select * from userdata where year=2010) then we'll only bhe able to see.
The entire data is stored in multiple folders so it could be huge in size thats why by defauld it won't work that why we have to show specific file using where clause. That is the reason why its stops supporting it  

# Conclusion 
This project provided hands-on experience with Hive, Hadoop HDFS, partitioning, bucketing, and Parquet, static partitioning. We learned how bucketing can reduce data shuffling, partitioning can reduce data scanning, and Parquet can reduce storage size through compression and efficient columnar storage. Overall, the project helped build a practical understanding of how Hive organizes and optimizes large datasets in HDFS.



