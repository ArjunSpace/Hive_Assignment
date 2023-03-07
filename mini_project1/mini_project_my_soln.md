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
    
