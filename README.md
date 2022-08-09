 db2-container-wait-action
---

The [ibm/db2](https://hub.docker.com/r/ibmcom/db2) container takes time from setup to database creation.  
Therefore, you will have to wait to set up the DB, run sql, etc. on Github Action.  
This Action simplifies the process of waiting for DB2 to start.  

[ibm/db2](https://hub.docker.com/r/ibmcom/db2) コンテナーはセットアップからデータベースの作成までに時間がかかります。  
そのため、Github Action上でDBの設定やsqlを実行などを行うためには、待つ必要があります。  
このActionはDB2の起動を待つ作業を簡易化したものになります。  

## Usage

```
name: workflow-example
on: push

jobs:
  simple-db2:
    runs-on: ubuntu-latest
    steps:
      - name: start db2 container
        run: docker run --name mydb2 --privileged=true -e LICENSE=accept -e DBNAME=testdb -dt ibmcom/db2

      - name: wait database create
        uses: amanoese/db2-container-wait-action@master

      - name: check connect database
        run: docker exec -t mydb2 bash -c "sudo -i -u db2inst1 db2 'connect to testdb'"
```

## Usage Advanced Setting

All options are below.

```
- uses: amanoese/db2-container-wait-action@master
  with:
    # ibmcom/db2 container name
    # if you not specifed. Set the first container name found
    CONTEINAR_NAME:  ''
    # set your db2 instance name
    # default 'db2inst1'
    DB2INSTANCE: ''
    # set your database name
    # if you not specifed. Set the first database name found
    # If the database name is not found, action ends with a check to start the DB instance.
    DBNAME: ''
    # default '30'
    WAIT_DB2_TIMEOUT:
    # default '30'
    WAIT_INSTANCE_TIMEOUT:
    # default '30'
    WAIT_CONNECT_TIMEOUT:
```
