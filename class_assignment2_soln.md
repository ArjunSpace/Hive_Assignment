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
            
            
 Q4.How can you add a new partition for the month December in the above partitioned table?           
            
       ALTER TABLE transaction_partition ADD PARTITION (month=’Dec’)
       
 Q5.I am inserting data into a table based on partitions dynamically. But, I received an error – FAILED ERROR IN SEMANTIC ANALYSIS: Dynamic partition strict mode requires at least one static partition column. How will you remove this error?
 
  
      To remove this error some hive properties needed to be change
      
      SET hive.exec.dynamic.partition = true;
      SET hive.exec.dynamic.partition.mode = nonstrict;
      
Q6.Suppose, I have a CSV file – ‘sample.csv’ present in ‘/temp’ directory with the following entries:
   id first_name last_name email gender ip_address
   How will you consume this CSV file into the Hive warehouse using built-in SerDe?

         SerDe expands as  serializer/deserializer. A SerDe converts the data into encrypted binary format  
       that we can process using Hive. Hive comes with several built-in SerDes and many other third-party SerDes are also available. 

         Hive provides a specific SerDe for working with CSV files.

         CREATE EXTERNAL TABLE sample

         (id int, 
          first_name string, 
          last_name string, email string,
          gender string, ip_address string) 
          ROW FORMAT SERDE ‘org.apache.hadoop.hive.serde2.OpenCSVSerde’
          with serdeproperties (
          "separatorChar" = ",",
          "quoteChar" = """,
          "escapeChar" = "\"
          STORED AS TEXTFILE LOCATION ‘/temp’;
  
Q7.LOAD DATA LOCAL INPATH ‘Home/country/state/’
   OVERWRITE INTO TABLE address;
   The following statement failed to execute. What can be the cause?
   
         When data loading from local the path should contain 'file:///' so the path would be
         'file:///Home/country/state/' 
         
Q8.Is it possible to add 100 nodes when we already have 100 nodes in Hive? If yes, how?

   
         Yes, we can add the nodes by following the below steps:

         Step 1: Take a new system; create a new username and password
         Step 2: Install SSH and with the master node setup SSH connections
         Step 3: Add ssh public_rsa id key to the authorized keys file
         Step 4: Add the new DataNode hostname, IP address, and other details in /etc/hosts slaves file:

                192.168.1.102 slave3.in slave3  
                
         Step 5: Start the DataNode on a new node
         Step 6: Login to the new node like suhadoop or:

                 ssh -X hadoop@192.168.1.103
                 
         Step 7: Start HDFS of the newly added slave node by using the following command:

                ./bin/hadoop-daemon.sh start data node
                
         Step 8: Check the output of the jps command on the new node
         
        

Hive Practical questions:

Hive Join operations

Create a  table named CUSTOMERS(ID | NAME | AGE | ADDRESS   | SALARY)
Create a Second  table ORDER(OID | DATE | CUSTOMER_ID | AMOUNT
)

Now perform different joins operations on top of these tables
(Inner JOIN, LEFT OUTER JOIN ,RIGHT OUTER JOIN ,FULL OUTER JOIN)



      create table customers(
          > id int,
          > name string,
          > age int,
          > address string,
          > salary int);
          
          
      create table orders(
          > o_id int,
          > o_date string,
          > customer_id int,
          > amount int
          > );
       
       orders table has date column and taken data as string, to cast the data into date datatype 
       creating another table 
       create table orders_casted(
          > o_id int,
          > o_date date,
          > customer_id int,
          > amount int)
          > row format delimited 
          > fields terminated by ','
          > tblproperties("skip.header.line.count" = "1");
          
              hive> select * from customers;
               OK
               customers.id       customers.name  customers.age   customers.address       customers.salary
               1       arjun      22      hyd             23000
               2       Dikshant   24      hyd             24000
               3       Akshat     25      banglore        26000
               4       Dhruv      31      delhi           90000
           
           Load data from original table to transformed table 
           
               from orders insert overwrite table orders_casted select*;

               hive> select * from orders_casted;
               OK
               102     2022-03-21      2       22000
               103     2022-03-21      2       22000
               104     2022-03-01      3       25000
               102     2022-03-21      4       32000
               Time taken: 0.117 seconds, Fetched: 4 row(s)

               hive> describe formatted orders_casted;

               OK
               # col_name              data_type               comment             

               o_id                    int                                         
               o_date                  date                                        
               customer_id             int                                         
               amount                  int                                         

               # Detailed Table Information             
               Database:               default                  
               Owner:                  root                     
               CreateTime:             Fri Mar 03 06:32:47 UTC 2023     
               LastAccessTime:         UNKNOWN                  
               Retention:              0                        
               Location:               hdfs://namenode:9000/user/hive/warehouse/orders_casted   
               Table Type:             MANAGED_TABLE            
               Table Parameters:                
                       COLUMN_STATS_ACCURATE   {\"BASIC_STATS\":\"true\"}
                       numFiles                1                   
                       numRows                 5                   
                       rawDataSize             110                 
                       skip.header.line.count  1                   
                       totalSize               115                 
                       transient_lastDdlTime   1677825376          

               # Storage Information            
               SerDe Library:          org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe       
               InputFormat:            org.apache.hadoop.mapred.TextInputFormat         
               OutputFormat:           org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat       
               Compressed:             No                       
               Num Buckets:            -1                       
               Bucket Columns:         []                       
               Sort Columns:           []                       
               Storage Desc Params:             
                       field.delim             ,                   
                       serialization.format    ,                   
               Time taken: 0.082 seconds, Fetched: 35 row(s)
               
              
INNER JOIN 

      select customers.*, casted_orders.* 
      from customers
      inner join casted_orders on customers.id=casted_orders.customer_id;
    

      customers.id    customers.name  customers.age   customers.address       customers.salary       casted_orders.o_id       casted_orders.o_date                           casted_orders.customer_id      casted_orders.amount
      2       Dikshant        24      hyd        24000   102     2022-03-21      2       22000
      2       Dikshant        24      hyd        24000   103     2022-03-21      2       22000
      3       Akshat          25      banglore   26000   104     2022-03-01      3       25000
      4       Dhruv           31      delhi      90000   102     2022-03-21      4       32000
      
      
      
LEFT OUTER JOIN


       select customers.*, casted_orders.* 
       from customers
       left outer join casted_orders on customers.id=casted_orders.customer_id;

      customers.id    customers.name  customers.age   customers.address  customers.salary casted_orders.o_id  
      casted_orders.o_date casted_orders.customer_id   casted_orders.amount
      1       arjun           22      hyd     23000   NULL    NULL    NULL    NULL
      2       Dikshant        24      hyd     24000   102     2022-03-21      2       22000
      2       Dikshant        24      hyd     24000   103     2022-03-21      2       22000
      3       Akshat          25      banglore26000   104     2022-03-01      3       25000
      4       Dhruv           31      delhi   90000   102     2022-03-21      4       32000
      Time taken: 6.506 seconds, Fetched: 5 row(s)
      
RIGHT OUTER JOIN

      hive> select customers.*, casted_orders.* 
      > from customers
      > right outer join casted_orders on customers.id=casted_orders.customer_id;
    
    
       customers.id   customers.name  customers.age   customers.address  customers.salary  casted_orders.o_id  
       casted_orders.o_date  casted_orders.customer_id       casted_orders.amount
      2       Dikshant        24      hyd     24000   102     2022-03-21      2       22000
      2       Dikshant        24      hyd     24000   103     2022-03-21      2       22000
      3       Akshat  25      banglore        26000   104     2022-03-01      3       25000
      4       Dhruv   31      delhi   90000   102     2022-03-21      4       32000
      Time taken: 6.263 seconds, Fetched: 4 row(s)
      
      
FULL OUTER JOIN 

      hive> select customers.*, casted_orders.* 
      > from customers
      > full outer join casted_orders on customers.id=casted_orders.customer_id;
    
      customers.id    customers.name  customers.age   customers.address customers.salary  casted_orders.o_id
      casted_orders.o_date    casted_orders.customer_id       casted_orders.amount
      1     arjun     22     hyd       23000   NULL    NULL    NULL    NULL
      2     Dikshant  24     hyd       24000   103     2022-03-21      2       22000
      2     Dikshant  24     hyd       24000   102     2022-03-21      2       22000
      3     Akshat    25     banglore  26000   104     2022-03-01      3       25000
      4     Dhruv     31     delhi     90000   102     2022-03-21      4       32000
      Time taken: 1.434 seconds, Fetched: 5 row(s)
      
