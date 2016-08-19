---
layout: post
title: MongoDB Basics
categories: mongodb
tags: []
---

* TOC
{:toc}

![MongoDB Logo](/img/mongo-db-logo.png)

# 1. Introduction

[MongoDB](https://www.mongodb.com/)는 Document-Oriented  [NoSQL](https://ko.wikipedia.org/wiki/NoSQL) 데이터베이스이다. 오픈 소스이며 엔진은 C++로 작성되었다.

MongoDB는 RDMS의 record와 유사한 개념의 [document](https://docs.mongodb.com/manual/core/document/) 라고 하는 JSON objects 형태의 key-value의 쌍으로 이루어진 데이터 구조로 구성된다.

value field에는 다른 document, array, document array가 포함될 수 있다.

![MongoDB Document](/img/mongodb-document.png)
{: style="max-width:450px; margin: 20px auto 10px;"}

MongoDB Document
{: style="color:gray; font-size: 80%; text-align: center; margin-top: 5px;"}


# 2. 특징

- Schema-less하다. 이는 RDMS처럼 고정 Schema가 존재하지 않는다는 의미로 같은 Collection 내에 있더라도 다른 Schema를 가질 수 있다.

- RDMS와 같은 JOIN이 없어 Table JOIN은 효과적이지 않지만(불가능하지는 않다) CRUD Query는 고속으로 동작한다.

- Scalability(규모 가변성, 확장성)이 우수하며 Sharding(여러 개의 데이터베이스에 데이터를 분할하는 기술) 클러스터 구축도 가능하다.

![mongodb sharding](/img/mongodb-sharding.gif)
{: style="max-width:550px; margin: 20px auto 10px;"}

MongoDB Sharding Clustering
{: style="color:gray; font-size: 80%; text-align: center; margin-top: 5px;"}

# 3. RDMS와 MongoDB의 비교

**Terms**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <td>Database</td>
    <td>Database</td>
  </tr>
  <tr>
    <td>Table</td>
    <td>Collection</td>
  </tr>
  <tr>
    <td>Tuple / Row</td>
    <td>Document</td>
  </tr>
  <tr>
    <td>Column</td>
    <td>Key / Field</td>
  </tr>
  <tr>
    <td>Table Join</td>
    <td>Embedded Documents</td>
  </tr>
  <tr>
    <td>Primary Key</td>
    <td>Primary Key (&#95;id)</td>
  </tr>
</table>

**Database Server**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <td>mysqld</td>
    <td>mongod</td>
  </tr>
</table>

**Database Client**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <td>mysql</td>
    <td>mongo</td>
  </tr>
</table>

**SQL**

<table>
  <tr>
    <th style="width:220px;">RDB(MySQL)</th>
    <th style="width:220px;">MongoDB</th>
  </tr>
  <tr>
    <th colspan="2">Insert</th>
  </tr>
  <tr>
    <td>insert into users ("name", "city") values("lee", "seoul")</td>
    <td>db.users.insert({name: "lee", city: "seoul"})</td>
  </tr>
  <tr>
    <th colspan="2">Select</th>
  </tr>
  <tr>
    <td>select * from users where name="lee"</td>
    <td>db.users.find({name: "lee"})</td>
  </tr>
  <tr>
    <th colspan="2">Update</th>
  </tr>
  <tr>
    <td>update users set city="busan" where name="lee"</td>
    <td>db.users.update({name: "lee"}, {$set: {city: "busan"}})</td>
  </tr>
  <tr>
    <th colspan="2">Delete</th>
  </tr>
  <tr>
    <td>delete from users where name="lee"</td>
    <td>db.users.remove({name: "lee"})</td>
  </tr>
</table>

# 4. Install

## 4.1 Windows

**1. download & install**

[MongoDB download](https://www.mongodb.com/download-center?jmp=homepage#community)에서 자신의 사양에 맞는 버전을 다운로드하여 install한다.

Community Server 3.2.9을 기준으로 설명한다.

설치가 완료되면 CMD 창을 열고 MongoDB가 설치된 디렉터리로 이동한다. 설치 디렉터리를 별도 지정하지 않았다면 `C:\Program Files\MongoDB\Server\3.2\bin\`에 설치된다.

```
cd C:\Program Files\MongoDB\Server\3.2\bin
```

**2. 기본 데이터베이스 디렉터리 생성**

기본 데이터베이스 디렉터리(C:\\data\\db)를 생성한다. 기본 데이터베이스 디렉터리에 데이터가 store된다.

```
C:\Program Files\MongoDB\Server\3.2\bin>mkdir C:\data\db
```

로그 파일이 저장될 디렉터리를 생성한다.

```
C:\Program Files\MongoDB\Server\3.2\bin>mkdir C:\mongodb\log
```

**3. 환경설정 파일 생성**

mongod.cfg 환경설정 파일을 생성하여 C:\\mongodb 내에 저장한다.

```
##Which IP address(es) mongod should bind to.
bind_ip = 127.0.0.1

##Which port mongod should bind to.
port = 27017

##I set this to true, so that only critical events and errors are logged.
quiet = true

##store data here
dbpath=C:\data\db

##The path to the log file to which mongod should write its log messages.
logpath=C:\mongodb\log\mongo.log

##I set this to true so that the log is not overwritten upon restart of mongod.
logappend = true

##log read and write operations
diaglog=3

##It ensures write durability and data consistency much as any journaling scheme would be expected to do.
##Only set this to false if you don’t really care about your data (or more so, the loss of it).
journal = true

##For mongodb 32 bit
storageEngine = mmapv1
```

**4. DB 기동**

MongoDB 서버를 기동한다.

```
C:\Program Files\MongoDB\Server\3.2\bin>mongod --config c:\mongodb\mongod.cfg
2016-08-18T16:41:10.289+0900 W CONTROL  [main] --diaglog is deprecated and will be removed in a future release
2016-08-18T16:41:10.295+0900 I COMMAND  [main] diagLogging level=3
2016-08-18T16:41:10.303+0900 I COMMAND  [main] diagLogging using file C:\data\db/diaglog.57b56696
```

새로운 CMD 창에서 MongoDB 클라이언트를 기동한다.

```
C:\Program Files\MongoDB\Server\3.2\bin>mongo
MongoDB shell version: 3.2.9
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user
Server has startup warnings:
2016-08-18T16:57:15.720+0900 W CONTROL  [main] --diaglog is deprecated and will be removed in a future release
2016-08-18T16:57:16.095+0900 I CONTROL  [initandlisten]
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten] ** WARNING: This 32-bit MongoDB binary is deprecated
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten]
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten]
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2016-08-18T16:57:16.096+0900 I CONTROL  [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2016-08-18T16:57:16.097+0900 I CONTROL  [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2016-08-18T16:57:16.097+0900 I CONTROL  [initandlisten]
>
```

**5. DB 중지**

MongoDB 클라이언트를 기동시킨 CMD창에서 admin 데이터베이스로 변경한 후 DB를 중지시킨다. 이때 MongoDB 서버 또한 중지된다.

```
> show dbs
local  0.078GB
> use admin
switched to db admin
> db.shutdownServer()
server should be down...
2016-08-18T17:08:07.726+0900 I NETWORK  [thread1] trying reconnect to 127.0.0.1:27017 (127.0.0.1) failed
2016-08-18T17:08:08.731+0900 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, reason: errno:10061 대상 컴퓨터에서 연결을 거부했으므로 연결하지 못했습 니다.
2016-08-18T17:08:08.734+0900 I NETWORK  [thread1] reconnect 127.0.0.1:27017 (127.0.0.1) failed failed
> quit()

C:\Program Files\MongoDB\Server\3.2\bin>
```


## 4.2 Mac

**1. download & install**

Mac의 경우, Homebrew를 사용하여 install할 수 있다.

```
$ brew update && brew install mongodb

$ mongo -version
MongoDB shell version: 3.2.8
```

**2. 기본 데이터베이스 디렉터리 생성**

기본 데이터베이스 디렉터리(/data/db)를 생성한다. 기본 데이터베이스 디렉터리에 데이터가 store된다.

```
$ mkdir -p /data/db
```

**3. DB 기동**

MongoDB 서버를 기동한다.

```
$ mongod
```

새로운 터미널에서 MongoDB 클라이언트를 기동한다.

```
$ mongo
```

**4. DB 중지**

MongoDB 클라이언트를 기동시킨 터미널에서 DB를 중지시킨다. 이때 MongoDB 서버 또한 중지된다.

```
> use admin
switched to db admin
> db.shutdownServer()
server should be down...
2016-08-19T14:15:10.057+0900 I NETWORK  [thread1] trying reconnect to 127.0.0.1:27017 (127.0.0.1) failed
2016-08-19T14:15:10.058+0900 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, reason: errno:61 Connection refused
2016-08-19T14:15:10.058+0900 I NETWORK  [thread1] reconnect 127.0.0.1:27017 (127.0.0.1) failed failed
> quit()

$ ps wuax | grep mongo
leeungmo    15609   0.0  0.0  2454296    820 s001  S+    2:16PM   0:00.00 grep mongo
```

# 5. MongoDB Shell

MongoDB Shell을 사용하여 CRUD의 기본을 알아본다.

## 5.1 Create

`use <database_name>` 명령어를 사용하여 database를 생성한다.

만약 동일한 이름의 Database가 존재할 경우, 그 database를 사용하며 존재하지 않을 경우, database를 생성한다.

```
> use mongo-example
switched to db mongo-example
```

`db` 명령어를 사용하여 현재 사용중인 database를 확인한다.

```
> db
mongo-example
```

`show dbs` 명령어를 사용하여 데이터베이스 리스트를 확인한다.

```
> show dbs
local             0.000GB
```

이 시점에서 mongo-example database는 아직 생성되지 않았다. 최소 한개 이상의 document를 추가하여야 database가 생성된다.

`db.<collection_name>.insert(<documents>)`로 document를 추가한다. 이때 RDBMS의 Table 개념의 [collection](https://docs.mongodb.com/manual/reference/glossary/#term-collection) books도 생성된다.

```
> db.books.insert({"title": "MongoDB Example", "author": "Lee", price: 100});
WriteResult({ "nInserted" : 1 })
```

한번에 여러개의 document를 추가할 수 있다.

```
> db.books.insert([
  {"title": "Example1", "author": "Lee", price: 200},
  {"title": "Example2", "author": "Lee", price: 300},
  {"title": "Example3", "author": "Lee", price: 400}
  ])
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 3,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
```

database list를 확인하면 mongo-example database가 생성된 것을 확인할 수 있다.

```
> show dbs
local             0.000GB
mongo-example     0.000GB
```

## 5.2 Read

`db.<collection_name>.find()`을 사용하여 collection 내의 document를 select한다.

다음 예제는 books collection 내의 모든 document를 select한다.

```
> db.books.find()
{ "_id" : ObjectId("57b6f590370997928bbee55d"), "title" : "MongoDB Example", "author" : "Lee", "price" : 100 }
{ "_id" : ObjectId("57b6f5a3370997928bbee55e"), "title" : "Example1", "author" : "Lee", "price" : 200 }
{ "_id" : ObjectId("57b6f5a3370997928bbee55f"), "title" : "Example2", "author" : "Lee", "price" : 300 }
{ "_id" : ObjectId("57b6f5a3370997928bbee560"), "title" : "Example3", "author" : "Lee", "price" : 400 }
```

조건을 부여하여 document를 select할 수 있다.

```
> db.books.find({title: "MongoDB Example"})
```

[Query operator(연산자)](https://docs.mongodb.com/manual/reference/operator/query/)를 사용하여 조건을 지정할 수 있다.

```
db.books.find({price:{$gte:150}})
{ ・・ "_id" : 2, "title" : "JavaScriptリファレンス", "price" : 300 }
```


db.<collection_name>.find( <query filter>, <projection> )

```
> db.books.find({title: "MongoDB Example"})
{ "_id" : ObjectId("57b6ac0e370997928bbee55b"), "title" : "MongoDB Example", "author" : "Lee", "price" : 100 }
```

## 5.3 Update


## 5.4 Delete





# Reference

* [The MongoDB 3.2 Manual](https://docs.mongodb.com/manual/)

* [Robomongo](https://robomongo.org/)