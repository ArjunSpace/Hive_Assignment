Scenario Based questions:

Q1.Will the reducer work or not if you use “Limit 1” in any HiveQL query?
   
     No, reducer will not worked for select query having limit 1
     
Q2.Suppose I have installed Apache Hive on top of my Hadoop cluster using default metastore configuration. Then, what will happen if we have multiple clients trying to access Hive at the same time? 

         The default metastore configuration allows only one Hive session to be opened at a time for accessing the metastore. 
      Therefore, if multiple clients try to access the metastore at the same time, they will get an error. One has to use a 
      standalone metastore, i.e. Local or remote metastore configuration in Apache Hive for allowing access to multiple clients concurrently. 
        
Q3.Suppose, I create a table that contains details of all the transactions done by the customers: CREATE TABLE transaction_details (cust_id INT, amount FLOAT, month STRING, country STRING) ROW FORMAT DELIMITED FIELDS TERMINATED BY ‘,’ ;
Now, after inserting 50,000 records in this table, I want to know the total revenue generated for each month. But, Hive is taking too much time in processing this query. How will you solve this problem and list the steps that I will be taking in order to do so?

      
      Query time in hive can be reduced by making the data partition by relevent column. in this case month would be ideal as 
      we want monthly generated revenue
      
      steps followd for partitioning:
         1.creating partition table
           create table transaction_partition
            (
            cust_id int,
            amount float,
            month string,
            country string
            )
            partitioned by (month string);
            
         2. Set hive properties for dynamic partitioning 
            set hive.exec.dynamic.partition.mode=nonstrict;
            
         3. Load data into partition table
         
            insert overwrite transaction_partition partition(month) select cust_id,amount,month,country from transaction_details; 
  
