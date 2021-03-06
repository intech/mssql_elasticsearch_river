# MSSQL ElasticSearch River #

This is a POC for real time updating of elasticsearch indexes from MSSQL using the service broker and an external C# application. It's far from production ready (obviously), but gives a good idea of how to have a scalable solution to real time indexing. 


## How it Works ##

1. User edits a table. 
2. A trigger fires writing a message to the service broker queue. Message contains an ID and the table that got edited. 
3. A C# application picks up that service broker message (Via long polling), and pulls the entire record from the table, and then sends it to an ElasticSearch index of your choice. 

## Setup ##

### SQL Setup ###
Install scripts for SQL are located in /sql/install

0. This is to install the service broker for your database if you haven't already. This only needs to be run once. 
1. This is to install the messages, contracts and queues. You will only have to do this once. 
2. This is the trigger that puts messages on queues. You will need to do this once per table that you wish to trigger. 

### C# River Setup ###
- Configuration is in the app.config. 
- You can run multiple rivers off the single C# application (It can read/write to multiple databases/indexes at once)
- You will need to also edit the connectionStrings (Obviously)
