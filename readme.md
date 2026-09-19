# Hive ProjectII (Bucketing Data)
Why Bucketing?
To Optimize the query performance it reduces shuffling because it helps you store the common data on common machine. Similar to partitioning that increases performance by reducing data scanning it also optmizes by reducing shuffling. 

## Technologies Used
- Apache Hive
- Hadoop HDFS
- HiveQL (SQL in Hive)
- Docker
- Linux Terminal
- CSV / Text
- Files

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

![Screenshot6](Screenshots_day16/Screenshot_(6).png)
