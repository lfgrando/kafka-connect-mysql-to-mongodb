# Kafka Connect with MySQL and MongoDB
This is a simple Kafka Connect transformation that recover `products` data from MySQL and sends it to MongoDB.

We configure Kafka Connect to use:

- Connector `debezium/debezium-connector-mysql:1.2.2` to get data from MySQL;
- Connector `mongodb/kafka-connect-mongodb:1.5.0` to sink data to MongoDB;
- Plugin SMT (Single Message Transformation) to extract only the `after` field, which contains the value inserted in MySQL database.

#How To Run

Follow the steps to see it runing:

1. Execute `docker-compose up -d` and wait to all containers to start up;
2. Setup MySQL database:
   1. Execute `docker exec -it kafka-connect-mysql-to-mongodb-mysql-1 bash`;
   2. Access MySQL client by executing the command `mysql -u root -proot`;
   3. Create database: `create database mysql-source-db;`
   4. Access database: `use mysql-sourcedb;`
   5. Create product table: `create table products (id int auto_increment primary key, name varchar(255));`
   6. Insert one or more products: `insert into products (name) values ('productName');`
3. Access Control Center in `http://localhost:9021/`;
4. Click in your cluster to enter;
5. Click on `Connect` and click in your cluster to enter in it;
6. Click in `Upload connector config file` and upload file `connectors\mysql.properties`;
7. Don't change anything and click in continue;
8. Repeat steps 6-7 to file `connectors\mongodb.properties`;
9. Overview the running process by accessing:
   1. Control Center Connect to see your tasks;
   2. Control Center Topics to see the topic `mysql-server.mysql-source-db.products` that Kafka Connect generated to transfer data;
   3. Mongo Express on `http://localhost:8085` to see the results on MongoDB on database `mongo-sink-database` and collection `mysql-server.mysql-source-db.products`.
10. OPTIONAL: Repeat the step 2.6 to insert more products and watch them be transfered from one database to another.