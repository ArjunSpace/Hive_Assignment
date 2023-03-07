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
    2023-03-07 04:43:40,312 INFO sasl.SaslDataTransferClient: SASL encryption trust check: localHostTrusted = false, remoteHostTrusted = false

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
      
       > select agent_name, count(agent_name)as Total_working_days from agent_performance
       > group by agent_name
       > order by agent_name;
      
--------------------------------------------------------------------------------------      
      agent_name      total_working_days
      Abhishek        30
      Aditya  30
      Aditya Shinde   30
      Aditya_iot      30
      Amersh  30
      Ameya Jain      30
      Anirudh         30
      Ankit Sharma    30
      Ankitjha        30
      Anurag Tiwari   30
      Aravind         30
      Ashad Nasim     30
      Ashish  60
      Ayushi Mishra   30
      Bharath         30
      Boktiar Ahmed Bappy     30
      Chaitra K Hiremath      30
      Deepranjan Gupta        30
      Dibyanshu       30
      Harikrishnan Shaji      30
      Hitesh Choudhary        30
      Hrisikesh Neogi 30
      Hyder Abbas     30
      Ineuron Intelligence    30
      Ishawant Kumar  30
      Jawala Prakash  30
      Jayant Kumar    30
      Jaydeep Dixit   30
      Khushboo Priya  30
      Madhulika G     30
      Mahak   30
      Mahesh Sarade   30
      Maitry  30
      Maneesh         30
      Manjunatha A    30
      Mithun S        30
      Mukesh  30
      Mukesh Rao      30
      Muskan Garg     30
      Nandani Gupta   30
      Nishtha Jain    30
      Nitin M 30
      Prabir Kumar Satapathy  30
      Prateek _iot    30
      Prerna Singh    30
      Rishav Dash     60
      Rohan   30
      Saif Khan       30
      Saikumarreddy N 30
      Samprit         30
      Sandipan Saha   30
      Sanjeev Kumar   30
      Sanjeevan       30
      Saurabh Shukla  30
      Shiva Srivastava        30
      Shivan K        30
      Shivan_S        30
      Shivananda Sonwane      30
      Shubham Sharma  30
      Sowmiya Sivakumar       30
      Spuri   30
      Sudhanshu Kumar 30
      Suraj S Bilgi   30
      Swati   30
      Tarun   30
      Uday Mishra     30
      Vasanth P       30
      Vivek   30
      Wasim   30
      Zeeshan         30
      Time taken: 2.566 seconds, Fetched: 70 row(s)
      
