## Installation of MySQL on Microsoft Azure:

1)	Created a students account on Microsoft Azure.
2)	Created Ubuntu Virtual Machine by undergoing basic configuration settings such as filling in username, password, virtual machine size and availability details.
3)	Connected with the Virtual Machine by using Putty. We use public IP address to connect with the VM.
4)  After we start installing MySQL using the following commands:
+ sudo apt-get update
+ sudo apt-get install mysql-server
5) After installation we make change in the bind address of configuration file. We open the configuration file by using following commands:
+ sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
6) After this we restart MySQL
+ sudo systemctl restart mysql.service

## Code For SQL queries:
+ 1
```
SELECT sts.name_stop, 
       Count(sts.stop_id) 
FROM   stops sts 
       JOIN stoptimes stt 
         ON ( sts.stop_id = stt.stop_id ) 
       JOIN trips tp 
         ON stt.trip_id = tp.trip_id 
GROUP  BY sts.name_stop 
ORDER  BY 2 DESC 
LIMIT  3;
```
+ 2
```
SELECT DISTINCT stt.stop_sequence, 
                sts.name_stop, 
                tp.route_id, 
                tp.trip_headsign, 
                tp.trip_id 
FROM   ((trips tp 
         JOIN stoptimes stt 
           ON tp.trip_id = stt.trip_id) 
        JOIN stops sts 
          ON sts.stop_id = stt.stop_id) 
WHERE  tp.trip_headsign LIKE '7 GOTTINGEN' 
       AND tp.route_id LIKE '7-114';
```
+ 3
```
SELECT DISTINCT trip_headsign 
FROM   trips tp 
       JOIN stoptimes stt 
         ON tp.trip_id = stt.trip_id 
WHERE  arrival_time >= '10:17:00' 
       AND departure_time <= '12:34:00';
```
+ 4
```
SELECT DISTINCT tp.trip_headsign AS Buses_list 
FROM   trips tp 
WHERE  tp.trip_id IN (SELECT stt.trip_id 
                      FROM   stops sts 
                             JOIN stoptimes stt 
                               ON sts.stop_id = stt.stop_id 
                      WHERE  sts.name_stop LIKE 
                             "barrington St [southbound] before Blowers St");
```

## Installation of Elastic Search on Microsoft Azure:

1)	Created a students account on Microsoft Azure.
2)	Created Ubuntu Virtual Machine by undergoing basic configuration settings such as filling in username, password, virtual machine size and availability details.
3)	Connected with the Virtual Machine by using Putty. We use public IP address to connect with the VM.
4)	Now we first install oracle java8 by using following commands:

+ sudo apt-add-repository -y ppa:webupd8team/java

+ sudo apt-get update


+ sudo apt-get -y install oracle-java8-installer
5)	 Then started the elastic search installation by using the following commands:

+ wget -qO -https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key  add –
sudo apt-get install apt-transport-https

+ echo “deb https://artifacts.elastic.co/packages/6.x/apt stable main” | sudo tee – a /etc/apt/sources.list.d/elastic-6.x.list

+ sudo apt-get update

+ sudo apt-get -y install elasticsearch
6)	 After installation we make few changes in the configuration file such as set and uncomment cluster name, node name, port and also network host. To open the configuration file we use the following command:

+ Sudo nano /etc/elasticsearch/elasticsearch.yml
7)	After this we run elastic search

+ Sudo service elasticsearch start
8)	After this we change the jvm allocated memory

+ Sudo nano/etc/elasticsearch/jvm.options
9)	After we restart the elastic search

+ Sudo service elasticsearch restart

# Code of the Elastic Search

+ 1
```
{{"query":{"match_phrase":{"name_stop":"barrington St [southbound] before Blowers St"}},"_source":["stop_id"]}

{"query":{"match_phrase":{"stop_id":"6102"}},"_source":["trip_id"]}

{"query":{"match_phrase":{"trip_id":"6511132-2012_05M-1205BRwd-Weekday-02"}},"_source":["trip_headsign"]}

```
+ 2
```
{   "query": {  
    "range" : {
        "arrival_time" : {
            "gte": "10:16:00", 
            "lte": "12:34:00",  
            "format": "HH:mm:ss"
        }
    }}
}

{"query":{"match_phrase":{"trip_id":"6514296-2012_05M-1205BRwd-Weekday-02"}},"_source":["trip_headsign"] }
```

+ 3

```
{"query":{"match_phrase":{"name_stop":"baker Dr opposite Lindenwood"}},"_source":["stop_id"]}

{"query":{"match_phrase":{"stop_id":"9066"}},"_source":["stop_sequence"]}

{
 "sort": [{
   "stop_sequence": {
     "order": "asc"
   }
 }],
 "query": {
   "match_phrase": {
     "stop_sequence": "3"
   }
 }
}

```
+ 4

```
{
 "size": 0,
 "aggs": {
   "countof_stop_id": {
     "terms": {
       "field": "stop_id",
       "order": {
         "_count": "desc"
       },
       "size": 3
     }
   }
 }
}

{"query":{"match_phrase":{"stop_id":"8643"}},"_source":["name_stop"]}

{"query":{"match_phrase":{"stop_id":"6105"}},"_source":["name_stop"]}

{"query":{"match_phrase":{"stop_id":"6108"}},"_source":["name_stop"]}

```












