CREATING TABLE BASED ON SCHEMA
   
        hive> create table loging_report(
        > s_no int,
        > Agent string,
        > login_Date string,
        > Login_time string,
        > logout_time string,
        > Duration string)
        > row format delimited 
        > fields terminated by ','
        > tblproperties('skip.header.line.count'= '1');
        
DOWNLOAD DATA FROM

Download Dataset 1 - https://drive.google.com/file/d/1WrG-9qv6atP-W3P_-gYln1hHyFKRKMHP/view

Download Dataset 2 - https://drive.google.com/file/d/1-JIPCZ34dyN6k9CqJa-Y8yxIGq6vTVXU/view

COPY DATA TO NAMENODE 

    docker cp AgentLogingReport.csv namenode:AgentLogingReport.csv
    
PUT DATA INTO HDFS LOCATION 

    root@edc538cf5722:/# hdfs dfs -put AgentLogingReport.csv  /data/openbeer/nueron/logingreport
    
   

LOAD DATA INTO TABLE

    hive> load data inpath '/data/openbeer/nueron/logingreport/AgentLogingReport.csv' into table loging_report;
   
    Loading data to table project1.loging_report
    OK
    Time taken: 0.48 seconds
    
CREATING PERFORMANCE TABLE 
   
   
        hive> create table agent_performance(
        > S_No int,
        > login_date string,
        > Agent_name string,
        > Total_chats int,
        > Average_response_time string,
        > Avg_resolution_time string,
        > Avg_rating float,
        > Total_feedback int)
        > row format delimited
        > fields terminated by ','
        > tblproperties('skip.header.line.count'= '1');
        
        
LOAD DATA
    
    hive> load data inpath '/data/openbeer/nueron/agent_performance/Agentperformance.csv' into table agent_performance;
    
   
3. List of all agents' names. 

      hive> select count( distinct agent_name) from agent_performance;
 
---------------------------------------------------------------------  
      agent_name
      
      Abhishek 
      Aditya 
      Aditya Shinde
      Aditya_iot 
      Amersh 
      Ameya Jain
      Anirudh 
      Ankit Sharma
      Ankitjha 
      Anurag Tiwari
      Aravind 
      Ashad Nasim
      Ashish 
      Ayushi Mishra
      Bharath 
      Boktiar Ahmed Bappy
      Chaitra K Hiremath
      Deepranjan Gupta
      Dibyanshu 
      Harikrishnan Shaji
      Hitesh Choudhary
      Hrisikesh Neogi
      Hyder Abbas
      Ineuron Intelligence 
      Ishawant Kumar
      Jawala Prakash
      Jayant Kumar
      Jaydeep Dixit
      Khushboo Priya
      Madhulika G
      Mahak 
      Mahesh Sarade
      Maitry 
      Maneesh 
      Manjunatha A
      Mithun S
      Mukesh 
      Mukesh Rao 
      Muskan Garg
      Nandani Gupta
      Nishtha Jain
      Nitin M
      Prabir Kumar Satapathy
      Prateek _iot 
      Prerna Singh
      Rishav Dash
      Rohan 
      Saif Khan
      Saikumarreddy N
      Samprit 
      Sandipan Saha
      Sanjeev Kumar
      Sanjeevan 
      Saurabh Shukla
      Shiva Srivastava
      Shivan K
      Shivan_S 
      Shivananda Sonwane
      Shubham Sharma
      Sowmiya Sivakumar
      Spuri 
      Sudhanshu Kumar
      Suraj S Bilgi
      Swati 
      Tarun 
      Uday Mishra
      Vasanth P
      Vivek 
      Wasim 
      Zeeshan 
      Time taken: 1.462 seconds, Fetched: 70 row(s)

4. Find out agent average rating.

       > select agent_name, round(avg(avg_rating),3)as avg_rating  from agent_performance 
       > group by agent_name
       > order by agent_name;
      
--------------------------------------------------------------------------------------------
      agent_name             avg_rating
      
      Abhishek               0.0
      Aditya                 0.0
      Aditya Shinde          1.8
      Aditya_iot             2.345
      Amersh                 0.0
      Ameya Jain             2.22
      Anirudh                0.645
      Ankit Sharma           0.0
      Ankitjha               0.267
      Anurag Tiwari          0.183
      Aravind                2.181
      Ashad Nasim            0.167
      Ashish  0.0
      Ayushi Mishra          3.482
      Bharath                2.984
      Boktiar Ahmed Bappy    3.568
      Chaitra K Hiremath     0.865
      Deepranjan Gupta       2.887
      Dibyanshu              0.0
      Harikrishnan Shaji     2.64
      Hitesh Choudhary       0.0
      Hrisikesh Neogi        3.136
      Hyder Abbas            0.0
      Ineuron Intelligence   0.0
      Ishawant Kumar         3.543
      Jawala Prakash         3.472
      Jayant Kumar           1.069
      Jaydeep Dixit          3.167
      Khushboo Priya         3.704
      Madhulika G            3.499
      Mahak                  0.1
      Mahesh Sarade          2.4
      Maitry                 2.927
      Maneesh                0.167
      Manjunatha A           3.595
      Mithun S               2.359
      Mukesh                 0.31
      Mukesh Rao             0.256
      Muskan Garg            0.712
      Nandani Gupta          2.924
      Nishtha Jain           3.282
      Nitin M                0.0
      Prabir Kumar Satapathy 2.51
      Prateek _iot           2.438
      Prerna Singh           3.233
      Rishav Dash            1.427
      Rohan                  0.0
      Saif Khan              0.0
      Saikumarreddy N        1.98
      Samprit                0.0
      Sandipan Saha          0.429
      Sanjeev Kumar         3.383
      Sanjeevan             0.0
      Saurabh Shukla        0.556
      Shiva Srivastava      0.945
      Shivan K              2.841
      Shivan_S              0.142
      Shivananda Sonwane    4.233
      Shubham Sharma        3.225
      Sowmiya Sivakumar      1.26
      Spuri                  0.0
      Sudhanshu Kumar        0.333
      Suraj S Bilgi         0.312
      Swati                2.424
      Tarun                0.05
      Uday Mishra          0.0
      Vasanth P            0.0
      Vivek                0.501
      Wasim                2.4
      Zeeshan              2.287
      Time taken: 2.52 seconds, Fetched: 70 row(s

      
5. Total working days for each agents 
      
      select agent, count(distinct login_date) as working_days from loging_report
      group by agent
      order by agent;
      
--------------------------------------------------------------------------------------      
      agent   working_days
     
      Aditya Shinde   1
      Aditya_iot      8
      Amersh  2
      Ameya Jain      7
      Ankitjha        2
      Anurag Tiwari   10
      Aravind 7
      Ayushi Mishra   9
      Bharath 8
      Boktiar Ahmed Bappy     9
      Chaitra K Hiremath      7
      Deepranjan Gupta        10
      Dibyanshu       9
      Harikrishnan Shaji      9
      Hrisikesh Neogi 9
      Hyder Abbas     2
      Ineuron Intelligence    1
      Ishawant Kumar  11
      Jawala Prakash  9
      Jaydeep Dixit   7
      Khushboo Priya  8
      Madhulika G     8
      Mahesh Sarade   8
      Maitry  5
      Manjunatha A    7
      Mithun S        8
      Mukesh  2
      Muskan Garg     6
      Nandani Gupta   9
      Nishtha Jain    8
      Nitin M 1
      Prabir Kumar Satapathy  7
      Prateek _iot    11
      Prerna Singh    9
      Rishav Dash     7
      Saikumarreddy N 7
      Sanjeev Kumar   9
      Saurabh Shukla  4
      Shiva Srivastava        8
      Shivan K        8
      Shivananda Sonwane      10
      Shubham Sharma  11
      Sowmiya Sivakumar       8
      Sudhanshu Kumar 6
      Suraj S Bilgi   2
      Swati   4
      Tarun   1
      Wasim   9
      Zeeshan 9
      
6. Total query that each agent have taken

       select agent_name, sum(total_chats) as total_chats from agent_performance
       group by agent_name;
      
-------------------------------------------------------------------------     
      agent_name      total_chats
      
      Abhishek        0
      Aditya  0
      Aditya Shinde   277
      Aditya_iot      231
      Amersh  0
      Ameya Jain      322
      Anirudh         81
      Ankit Sharma    0
      Ankitjha        5
      Anurag Tiwari   4
      Aravind         366
      Ashad Nasim     18
      Ashish  0
      Ayushi Mishra   514
      Bharath         369
      Boktiar Ahmed Bappy     452
      Chaitra K Hiremath      64
      Deepranjan Gupta        493
      Dibyanshu       1
      Harikrishnan Shaji      381
      Hitesh Choudhary        1
      Hrisikesh Neogi 578
      Hyder Abbas     0
      Ineuron Intelligence    0
      Ishawant Kumar  338
      Jawala Prakash  439
      Jayant Kumar    127
      Jaydeep Dixit   512
      Khushboo Priya  446
      Madhulika G     469
      Mahak   7
      Mahesh Sarade   364
      Maitry  542
      Maneesh         4
      Manjunatha A    413
      Mithun S        503
      Mukesh  19
      Mukesh Rao      5
      Muskan Garg     56
      Nandani Gupta   560
      Nishtha Jain    373
      Nitin M 0
      Prabir Kumar Satapathy  299
      Prateek _iot    190
      Prerna Singh    401
      Rishav Dash     409
      Rohan   0
      Saif Khan       0
      Saikumarreddy N 364
      Samprit         1
      Sandipan Saha   30
      Sanjeev Kumar   507
      Sanjeevan       0
      Saurabh Shukla  16
      Shiva Srivastava        53
      Shivan K        357
      Shivan_S        7
      Shivananda Sonwane      441
      Shubham Sharma  510
      Sowmiya Sivakumar       206
      Spuri   0
      Sudhanshu Kumar 2
      Suraj S Bilgi   28
      Swati   524
      Tarun   22
      Uday Mishra     0
      Vasanth P       0
      Vivek   44
      Wasim   433
      Zeeshan         542
      Time taken: 1.398 seconds, Fetched: 70 row(s)
      
   
7. Total Feedback that each agent have received

      select agent_name, sum(total_feedback) as total_feedback from agent_performance
      group by agent_name;

-----------------------------------------------------------------------
      agent_name      total_feedback
      
      Abhishek        0
      Aditya  0
      Aditya Shinde   153
      Aditya_iot      131
      Amersh  0
      Ameya Jain      228
      Anirudh         39
      Ankit Sharma    0
      Ankitjha        3
      Anurag Tiwari   3
      Aravind         233
      Ashad Nasim     9
      Ashish  0
      Ayushi Mishra   329
      Bharath         247
      Boktiar Ahmed Bappy     311
      Chaitra K Hiremath      37
      Deepranjan Gupta        312
      Dibyanshu       0
      Harikrishnan Shaji      231
      Hitesh Choudhary        0
      Hrisikesh Neogi 367
      Hyder Abbas     0
      Ineuron Intelligence    0
      Ishawant Kumar  202
      Jawala Prakash  250
      Jayant Kumar    70
      Jaydeep Dixit   305
      Khushboo Priya  289
      Madhulika G     281
      Mahak   5
      Mahesh Sarade   216
      Maitry  347
      Maneesh         3
      Manjunatha A    254
      Mithun S        364
      Mukesh  17
      Mukesh Rao      5
      Muskan Garg     37
      Nandani Gupta   308
      Nishtha Jain    257
      Nitin M 0
      Prabir Kumar Satapathy  222
      Prateek _iot    107
      Prerna Singh    235
      Rishav Dash     264
      Rohan   0
      Saif Khan       0
      Saikumarreddy N 290
      Samprit         0
      Sandipan Saha   18
      Sanjeev Kumar   311
      Sanjeevan       0
      Saurabh Shukla  8
      Shiva Srivastava        46
      Shivan K        243
      Shivan_S        4
      Shivananda Sonwane      263
      Shubham Sharma  300
      Sowmiya Sivakumar       141
      Spuri   0
      Sudhanshu Kumar 2
      Suraj S Bilgi   15
      Swati   302
      Tarun   6
      Uday Mishra     0
      Vasanth P       0
      Vivek   20
      Wasim   284
      Zeeshan         335
      Time taken: 1.272 seconds, Fetched: 70 row(s)
      
8. Agent name who have average rating between 3.5 to 4 

     
         > select * from (
         > select agent_name, avg(avg_rating) as tavg_rating from agent_performance
         > where avg_rating != 0
         > group by agent_name)tmp
         > where tavg_rating between 3.5 and 4;

-------------------------------------------------------------

         tmp.agent_name  tmp.tavg_rating
         Ankitjha        4.0
         Mithun S        3.931666705343458    
      
 
      
9. Agent name who have rating less than 3.5 

       > select * from (
       > select agent_name, avg(avg_rating) as tavg_rating from agent_performance
       > where avg_rating != 0
       > group by agent_name)tmp
       > where tavg_rating < 3.5;
       
       
-----------------------------------------------------------
      
      tmp.agent_name  tmp.tavg_rating
      Anirudh         3.224999984105428
      Anurag Tiwari   2.75
      Ashad Nasim     2.5
      Mahak   3.0
      Maneesh         2.5
      Mukesh Rao      2.5566666523615518
      Tarun   1.5
      Vivek   3.0039999961853026
      Time taken: 1.343 seconds, Fetched: 8 row(s)
      

10. Agent name who have rating more than 4.5 

          > select * from (
          > select agent_name, avg(avg_rating) as tavg_rating from agent_performance
          > where avg_rating != 0
          > group by agent_name)tmp
          > where tavg_rating > 4.5;
    

-------------------------------------------------------------------
         tmp.agent_name  tmp.tavg_rating

         Aditya Shinde   4.500833352406819
         Aravind         4.674285752432687
         Bharath         4.711052618528667
         Jaydeep Dixit   4.524285759244647
         Mukesh  4.644999980926514
         Prateek _iot    4.571874976158142
         Saikumarreddy N 4.570000024942251
         Shivananda Sonwane      4.534999992166247
         Shubham Sharma  4.607619081224714
         Sudhanshu Kumar 5.0
         Suraj S Bilgi   4.680000066757202
         Wasim   4.500000029802322
         Time taken: 1.407 seconds, Fetched: 12 row(s)

11. How many feedback agents have received more than 4.5 average


         > select count(distinct agent_name) as total_agents from agent_performance
         > where avg_rating > 4.5;

         total_agents
         47
 

14. Find the number of chat on which they have received a feedback 

          > select count(*) from agent_performance
          > where avg_rating = 0;
          1429

16. Perform inner join, left join and right join based on the agent column and after joining the table export that data into your local system.

             > insert overwrite directory '/output_file/inner_join' row format delimited fields terminated by ','  
             > select l.*,a.* from loging_report l
             > inner join agent_performance a on l.agent = a.Agent_name 
             > limit 20;


      root@edc538cf5722:/# hdfs dfs -cat /output_file/inner_join/*

         2023-03-08 14:06:45,317 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false
         85,Nandani Gupta,29-Jul-22,15:41:31,21:02:48,05:21:17,2,7/30/2022,Nandani Gupta,11,0:01:15,0:28:25,3.14,7
         3,Nandani Gupta,30-Jul-22,15:04:24,17:31:07,02:26:42,2,7/30/2022,Nandani Gupta,11,0:01:15,0:28:25,3.14,7
         985,Prerna Singh,20-Jul-22,13:06:26,13:09:26,00:03:00,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         971,Prerna Singh,20-Jul-22,14:58:25,20:57:36,05:59:11,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         878,Prerna Singh,21-Jul-22,13:01:46,13:17:03,00:15:17,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         876,Prerna Singh,21-Jul-22,14:01:37,14:05:15,00:03:38,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         746,Prerna Singh,22-Jul-22,15:03:29,21:07:20,06:03:51,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         660,Prerna Singh,23-Jul-22,15:03:12,21:09:19,06:06:06,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         514,Prerna Singh,25-Jul-22,15:01:23,18:23:58,03:22:34,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         496,Prerna Singh,25-Jul-22,18:28:21,21:03:43,02:35:22,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         427,Prerna Singh,26-Jul-22,15:05:30,21:02:08,05:56:38,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         426,Prerna Singh,26-Jul-22,15:05:30,15:07:27,00:01:56,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         393,Prerna Singh,26-Jul-22,21:25:27,21:33:57,00:08:30,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         391,Prerna Singh,26-Jul-22,21:55:51,22:03:21,00:07:29,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         388,Prerna Singh,26-Jul-22,22:17:56,22:19:56,00:02:00,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         336,Prerna Singh,27-Jul-22,13:11:06,20:58:35,07:47:29,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         110,Prerna Singh,29-Jul-22,12:08:23,12:11:35,00:03:11,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         91,Prerna Singh,29-Jul-22,15:08:22,17:20:49,02:12:27,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         75,Prerna Singh,29-Jul-22,17:47:06,21:03:44,03:16:37,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9
         16,Prerna Singh,30-Jul-22,12:32:28,14:10:08,01:37:40,1,7/30/2022,Prerna Singh,11,0:00:38,0:04:20,4.11,9


             > insert overwrite directory '/output_file/left_join' row format delimited fields terminated by ','  
             > select l.*,a.* from loging_report l
             > left join agent_performance a on l.agent = a.Agent_name 
             > limit 20;
      
             > insert overwrite directory '/output_file/right_join' row format delimited fields terminated by ','  
             > select l.*,a.* from loging_report l
             > right join agent_performance a on l.agent = a.Agent_name 
             > limit 20;
    
            root@edc538cf5722:/# hdfs dfs -ls /output_file/         
            Found 3 items
            drwxr-xr-x   - root supergroup          0 2023-03-08 14:05 /output_file/inner_join
            drwxr-xr-x   - root supergroup          0 2023-03-08 14:10 /output_file/left_join
            drwxr-xr-x   - root supergroup          0 2023-03-08 14:11 /output_file/right_join

PARTITIONING & BUCKETING


      create table loging_report_partition(
      s_no int,
      login_Date string,
      Login_time string,
      logout_time string,
      Duration string)
      partitioned by (Agent string)
      ;


      
      hive> insert overwrite table loging_report_partition partition(Agent)select s_no,login_Date,login_time,logout_time,Duration,Agent from loging_report;
      
      
      WARNING: Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez)or           using Hive 1.X releases.
      Query ID = root_20230308143543_e21e525b-d3b0-4567-86ef-0fd11ca8b3f5
      Total jobs = 3
      Launching Job 1 out of 3
      Number of reduce tasks is set to 0 since there's no reduce operator
      Job running in-process (local Hadoop)
      2023-03-08 14:35:45,053 Stage-1 map = 0%,  reduce = 0%
      2023-03-08 14:35:50,060 Stage-1 map = 100%,  reduce = 0%
      Ended Job = job_local340124104_0006
      Stage-4 is selected by condition resolver.
      Stage-3 is filtered out by condition resolver.
      Stage-5 is filtered out by condition resolver.
      Moving data to directory hdfs://namenode:9000/user/hive/warehouse/
      project1.db/loging_report_partition/.hive-staging_hive_2023-03-08_14-35-43_586_2330934402172339967-1/-ext-10000
      Loading data to table project1.loging_report_partition partition (agent=null)

      Loaded : 49/49 partitions.
               Time taken to load dynamic partitions: 8.549 seconds
               Time taken for adding to write entity : 0.005 seconds
      MapReduce Jobs Launched: 
      Stage-Stage-1:  HDFS Read: 84023 HDFS Write: 56392 SUCCESS
      Total MapReduce CPU Time Spent: 0 msec
      OK
      Time taken: 28.618 seconds


      root@edc538cf5722:/# hdfs dfs -ls /user/hive/warehouse/project1.db/loging_report_partition
      Found 49 items
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Aditya Shinde
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Aditya_iot
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Amersh
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Ameya Jain
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Ankitjha
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Anurag Tiwari
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Aravind
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Ayushi Mishra
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Bharath
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Boktiar Ahmed Bappy
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Chaitra K Hiremath
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:36 /user/hive/warehouse/project1.db/loging_report_partition/agent=Deepranjan Gupta
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Dibyanshu
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Harikrishnan Shaji
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Hrisikesh Neogi
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Hyder Abbas
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Ineuron Intelligence
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Ishawant Kumar
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Jawala Prakash
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Jaydeep Dixit
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Khushboo Priya
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:36 /user/hive/warehouse/project1.db/loging_report_partition/agent=Madhulika G
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Mahesh Sarade
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Maitry
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Manjunatha A
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Mithun S
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:36 /user/hive/warehouse/project1.db/loging_report_partition/agent=Mukesh
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Muskan Garg
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Nandani Gupta
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Nishtha Jain
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Nitin M
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Prabir Kumar Satapathy
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Prateek _iot
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Prerna Singh
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Rishav Dash
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Saikumarreddy N
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Sanjeev Kumar
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Saurabh Shukla
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Shiva Srivastava
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Shivan K
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Shivananda Sonwane
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Shubham Sharma
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Sowmiya Sivakumar
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Sudhanshu Kumar
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Suraj S Bilgi
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Swati
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Tarun
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Wasim
      drwxrwxr-x   - root supergroup          0 2023-03-08 14:35 /user/hive/warehouse/project1.db/loging_report_partition/agent=Zeeshan



      create table loging_partition_bucketed(
      s_no int,
      login_Date string,
      Login_time string,
      logout_time string,
      Duration string)
      partitioned by (Agent string)
      clustered by (s_no)
      into 3 buckets
      row format delimited
      fields terminated by ','


      insert overwrite table loging_partition_bucketed partition(Agent) select s_no,login_Date,Login_time,logout_time,Duration,Agent from loging_report;
      
      
      WARNING: Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or           using Hive 1.X releases.
      Query ID = root_20230308145757_d57cb7aa-6b1a-42c9-b8b4-7db8a5256325
      Total jobs = 1
      Launching Job 1 out of 1
      Number of reduce tasks determined at compile time: 3
      In order to change the average load for a reducer (in bytes):
        set hive.exec.reducers.bytes.per.reducer=<number>
      In order to limit the maximum number of reducers:
        set hive.exec.reducers.max=<number>
      In order to set a constant number of reducers:
        set mapreduce.job.reduces=<number>
      Job running in-process (local Hadoop)
      2023-03-08 14:57:59,586 Stage-1 map = 100%,  reduce = 0%
      2023-03-08 14:58:05,619 Stage-1 map = 100%,  reduce = 33%
      2023-03-08 14:58:17,660 Stage-1 map = 100%,  reduce = 67%
      2023-03-08 14:58:27,691 Stage-1 map = 100%,  reduce = 100%
      Ended Job = job_local1605565131_0001
      Loading data to table project1.loging_partition_bucketed partition (agent=null)

      Loaded : 49/49 partitions.
               Time taken to load dynamic partitions: 7.967 seconds
               Time taken for adding to write entity : 0.007 seconds
      MapReduce Jobs Launched: 
      Stage-Stage-1:  HDFS Read: 221404 HDFS Write: 101431 SUCCESS
      Total MapReduce CPU Time Spent: 0 msec
      OK
      Time taken: 45.296 seconds
