Prerequisites: docker with cocker-compose

# Create the directory structures


    $ mkdir Hive
    $ cd Hive
    $ touch docker-compose.yml
    $ touch hadoop-hive.env
    $ mkdir employee
    $ cd employee
    $ touch employee_table.hql
    $ touch employee.csv


## Run the server

    $ docker-compose up

## Check Status

    $ docker stats

## Setting the demo / tables

    docker exec -it hive-server /bin/bash
    

### From within the shell

    root@dc86b2b9e566:/opt# ls  
    hadoop-2.7.4  hive
    
    root@dc86b2b9e566:/opt# cd ..
      
    root@dc86b2b9e566:/# ls
    bin  boot  dev employee  entrypoint.sh  etc  hadoop-data  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
    
    root@dc86b2b9e566:/# cd employee/
    

## Create table

    root@dc86b2b9e566:/employee# hive -f employee_table.hql

## Add data to table

    root@dc86b2b9e566:/employee# hadoop fs -put employee.csv hdfs://namenode:8020/user/hive/warehouse/testdb.db/employee

## Validate the setup

    root@df1ac619536c:/employee# hive
    hive> show databases;
    OK  
    default  
    testdb
    
    Time taken: 2.363 seconds, Fetched: 2 row(s)

    hive> use testdb;
    OK
    Time taken: 0.085 seconds

    hive> select * from employee;
    OK
    1 Rudolf Bardin 30 cashier 100 New York 40000 5
    2 Rob Trask 22 driver 100 New York 50000 4
    3 Madie Nakamura 20 janitor 100 New York 30000 4
    4 Alesha Huntley 40 cashier 101 Los Angeles 40000 10
    5 Iva Moose 50 cashier 102 Phoenix 50000 20
    
    Time taken: 4.237 seconds, Fetched: 5 row(s)

## Stop all docker containers

    docker-compose down

## Restart and verify the tables exist

    docker-compose up


## log onto the hive-server and run the select query

    $ docker exec -it hive-server /bin/bash
    root@df1ac619536c:/opt# hive
    
    hive> select * from testdb.employee;
    OK
    1 Rudolf Bardin 30 cashier 100 New York 40000 5
    2 Rob Trask 22 driver 100 New York 50000 4
    3 Madie Nakamura 20 janitor 100 New York 30000 4
    4 Alesha Huntley 40 cashier 101 Los Angeles 40000 10
    5 Iva Moose 50 cashier 102 Phoenix 50000 20
    
    Time taken: 4.237 seconds, Fetched: 5 row(s)
