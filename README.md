<div align="center">


<!-- add technical charcha logo postgres session 3 -->
# PostgresSQL Session 4        
### — Task Documentation by Yash Anand —    
__________________________________________________________________________________

## Contents
</div>

## Contents

  - [**Overview**](#overview)
  - [**Task 1: Create Roles**](#task-1-create-roles)
    - [**1.1. Creating Three Roles**](#11-creating-three-roles)
    - [**1.2. Assigning Passwords**](#12-assigning-passwords)
  - [**Task 2: Grant Privileges**](#task-2-grant-privileges)
    - [**2.1. Creating DB: `ecommerce`**](#21-creating-db-ecommerce)
    - [**2.2 Assigning Privileges To Users**](#22-assigning-privileges-to-users)
      - [**2.2.1 All DB Privileges To `admin`**](#221-all-db-privileges-to-admin)
      - [**2.2.2 Read \& Write Table Privileges To `employee`**](#222-read--write-table-privileges-to-employee)
      - [**2.2.3 Read Table Privileges To `customer`**](#223-read-table-privileges-to-customer)
  - [**Task 3: Host Based Authentication**](#task-3-host-based-authentication)
    - [**3.1. Understanding `pg_hba.conf`**](#31-understanding-pg_hbaconf)
    - [**3.2 Connecting Localhost With All Roles**](#32-connecting-localhost-with-all-roles)
    - [**3.3 Allowing Admin To Connect From All IPs**](#33-allowing-admin-to-connect-from-all-ips)
  - [**Task 4: Table and Schema Creation**](#task-4-table-and-schema-creation)
    - [**4.1. Creating Product Schema**](#41-creating-product-schema)
    - [**4.2. Creating `products` Table**](#42-creating-products-table)
  - [**Task 5: Generate Self-Signed Certificates**](#task-5-generate-self-signed-certificates)
  - [**Task 6: Configure PostgreSQL**](#task-6-configure-postgresql)
  - [**Task 7: Configure pg\_hba.conf**](#task-7-configure-pg_hbaconf)
  - [**Task 8: Testing**](#task-8-testing)
  - [**Conclusion**](#conclusion)


_____________________________________________________________________________________      
<div align="center">
   
# **Overview** 
</div>

While the first three Technical Charcha Sessions on PostgreSQL were about helped the participants better understand querying of data, the fourth session helped us learn more about administration under PostgreSQL.

As per the fourth PostgreSQL Assignment that was assigned by Mr. Vinay Panwar, we worked on mainly on the following topics in order to better understand its administration aspects:
- Privileges
- Configuring PostgreSQL
- SSL/TLS
- Authenticating & Authorisation

In the coming sections, we will be diving into these details as I demonstrate how the assigned tasks were executed by me.
<!-- 
<div align="center">     

[![IMAGE_ALT](https://img.youtube.com/vi/NvrpuBAMddw/maxresdefault.jpg)](https://www.youtube.com/watch?v=NvrpuBAMddw)
   </div> -->

<!-- Adding youtube videos
0 or 1 or 2 or 3 or 4, 0 (big) to 4 (small)
hqdefault.jpg <- high quality
mqdefault.jpg <- medium quality
sddefault.jpg <- standard definition
maxresdefault.jpg <- maximum resolution -->

_____________________________________________________________________________________     
<div align="center">

## **Task 1: Create Roles**
</div>

In PostgreSQL, `Roles` are used to represent user accounts, unlike other databases systems which represent users with users. Any role that is allowed to login is considered as a user in Postgres and the roles without login privileges are considered as a group. The main idea of creating roles is to enhance security and allow access to database.

Before being able to start this task, I needed to PostgreSQL installed on my system, which was done using the [official documentation](https://www.postgresql.org/download/linux/ubuntu/). After that, I entered into Postgres with `sudo su postgres` and started the `psql` terminal for interacting with PostgreSQL. This was done in the following way:
<div align="center">

![image](https://i.imgur.com/ryGiwc7.gif)
</div>

As per this task, we were to create 3 roles and assign passwords to each of them. The demonstration of how this task was executed has been provided below.

## **1.1. Creating Three Roles**
> 1.1. Create three roles: admin, employee, and customer. Use the CREATE ROLE statement

As per the first sub-task, we were asked to create the following three roles:
- admin
- employee
- customer

After ensuring that I had entered into my PostgreSQL server, I started working on the first sub-task and for that, I utilised the 'CREATE ROLE' SQL statement and was able to successfully create these Roles. The go-through of how these commands were created is as follows 
<div align="center">

![image](https://i.imgur.com/JAb4jzv.gif)
</div>

In order to create the three roles that have been mentioned above, I wrote the following queries for creating the respective Roles:
- admin 
  ```
  CREATE ROLE admin;
  ```
- employee
  ```
  CREATE ROLE employee;
  ```
- customer 
  ```
  CREATE ROLE cutomer;
  ```

The output of all the above queries was `CREATE ROLES`, which meant that these roles had been created. 

Having run these queries for creating Roles in PostgreSQL, I needed to also ensure that these Roles had truly been created and added to to the database. For this, I ran the `\du` command and through this, I was able to successfully list all the Roles that had just been created. The output of running `\du` entire command was as follows:
<div align="center">

![image](https://i.imgur.com/4TMBBfw.png)
</div>

Note how `CANNOT LOGIN` is mentioned under the attributes column. In order to allow login from these roles, their privileges would have to be changed. We will be doing this in the second task, when granting privileges to these roles . 

## **1.2. Assigning Passwords**
> 1.2. Assign a password to each role    

Having created the three required roles of `admin`, `employee` and `customer`, I proceeded with assigning password to each of these roles. In order to do so, I had to run the following queries for adding passwords to the following roles: 

- admin 
  ```
  ALTER ROLE admin WITH PASSWORD '123'
  ;
  ```
- employee
  ```
  ALTER ROLE employee WITH PASSWORD '456'
  ;
  ```
- customer 
  ```
  ALTER ROLE customer WITH PASSWORD '789'
  ;
  ```
The output of running all these queries has been provided below, where `ALTER ROLE` represents the successful updation of the role with the new password:

<div align="center">

![image](https://i.imgur.com/swjmSPU.gif)
</div>

_____________________________

<div align="center">

## **Task 2: Grant Privileges**
</div>

In postgreSQL, privileges can be considered as permissions assigned to `Roles`. These permissions or privileges help define which user would get which permission, allowing various users to interact with a database with a specified level of permissions.            

The resources utilised for working on this task included the following posts from StackExchange and StackOverflow:

- [How to GRANT SELECT by default to B, on tables created by A?](https://dba.stackexchange.com/questions/238898/how-to-grant-select-by-default-to-b-on-tables-created-by-a)
- [Is there a way to grant permissions on tables that don't exist yet (or that get recreated)?](https://stackoverflow.com/questions/56624506/is-there-a-way-to-grant-permissions-on-tables-that-dont-exist-yet-or-that-get)
- [Grant privileges on future tables in PostgreSQL?](https://stackoverflow.com/questions/22684255/grant-privileges-on-future-tables-in-postgresql)
- [Granting access to all tables for a user](https://dba.stackexchange.com/questions/33943/granting-access-to-all-tables-for-a-user)

## **2.1. Creating DB: `ecommerce`**
> Create a database named ecommerce.

As per the first sub-task under the `Task 2`, I created a database called `ecommerce`. A go through of how this task was done is as follows:
<div align="center">

![image](https://i.imgur.com/k9PfrRw.gif)
</div>

In order to create such a database, I wrote the following query:
```
CREATE DATABASE ecommerce;
``` 
The output of the above query was `CREATE DATABASE`, which meant that the database had been successfully created. To ensure ths database's creation, I ran the `\l` command to list all the databases in my PostgreSQL. The output of this command included the newly created database of `ecommerce` and therefore proved that the database had been created:
<div align="center">

![image](https://i.imgur.com/GCXmVXt.png)
</div>

____________
## **2.2 Assigning Privileges To Users**
Using this [TablePlus](https://tableplus.com/blog/2018/04/postgresql-how-to-grant-access-to-users.html) article as a reference, I was able to assign specific privileges to each role of `admin`, `employee` and `customer`. Under the sub-tasks provided below, the assignment of these privileges to their respective roles was done.  

<div align="center">

### **2.2.1 All DB Privileges To `admin`**
</div>

> The admin role should have full access to the ecommerce database.

Under this sub-task, all privileges related to the `ecommerce` database were to be assigned to the role of `admin`. I was able to assign the required privileges through the following steps:
<div align="center">

![image](https://i.imgur.com/Znb2SKb.gif)
</div>

Before assigning the privileges, I ran the `\l` command to check the current assigned privileges to this database. This information was to be displayed under the  `Access Privileges` column of the displayed output, which was currently empty.

To assign all privileges to this database to the user `admin`, I ran the following query:

```
GRANT ALL PRIVILEGES ON DATABASE ecommerce TO admin;
```

The output of this query was `GRANT`, which helped me understand that the privilege had been granted successfully. To ensure this, I ran the `\l` command again and its output was as follows:
<div align="center">

![image](https://i.imgur.com/Xd8ZQ6Q.png)
</div>

In the above output, the `admin=CTc/postgres` explained that the `admin` user had been granted all privileges to the `ecommerce` database along with the default postgres user. `CTc` here means that the admin role can connect to this database (`C`) and create temprary objects (`Tc`) in `ecommerce` database.

<div align="center">

### **2.2.2 Read & Write Table Privileges To `employee`**
</div>

> The employee role should have read and write access to the products table
> 
As per this sub-task, I was to assign `Read` and `Write` privileges to the `employee` role for a table called `products` that had not been created yet. Initially, I had considered creating this table for the sake of performing this task but after researching and going through forum posts such as the following, I was able to perform this task:
- [Is there a way to grant permissions on tables that don't exist yet (or that get recreated)?](https://stackoverflow.com/questions/56624506/is-there-a-way-to-grant-permissions-on-tables-that-dont-exist-yet-or-that-get)
- [Grant privileges on future tables in PostgreSQL?](https://stackoverflow.com/questions/22684255/grant-privileges-on-future-tables-in-postgresql)

A go-through of how this entire sub-task was executed, has been provided below for reference:
<div align="center">

![image](https://i.imgur.com/i2b4ZKN.gif)
</div>

Using the resources mentioned before as reference, I wrote the following query for assigning the `employees` role with `Read` and `Write` privileges for the `products` tables:
```
ALTER DEFAULT PRIVILEGES FOR ROLE admin IN SCHEMA public GRANT DELETE, INSERT, UPDATE, SELECT ON TABLES TO employees;
```

As per my research, `ALTER DEFAULT PRIVILEGES` is used to assign privileges to objects - like tables - that have not been created yet. The output of the running the previously mentioned query was `ALTER DEFAULT PRIVILEGES`, which was meant to explain that the `Read` and `Write` privileges had been assigned to the Role of `employee` for the `products` table.

In order to ensure the table's creation had been completed, I ran the `\ddp` command, which is used to explain display all the default privileges. The output of this command proved that the specified privileges had been successfully assigned to the `employee` role. The output of this command was:
<div align="center">

![image](https://i.imgur.com/wUwLP9B.png)
</div>

Here, the meaning of the variables associated with the `employee` role meant the following:
- a: For allowing `INSERT` privilege to add data to table
- r: For allownig `SELECT` privilege to view the data of table
- w: For allowing `UDPATE` privilege to modify exisitng data in the table
- d: For allowing `DELETE` privilege to delete data from table

<div align="center">

### **2.2.3 Read Table Privileges To `customer`**
</div>

>The customer role should have read-only access to the products table.

In addition to the resouces mentioned previously for the second task, I also utilised this [Article by CommandPrompt](https://www.commandprompt.com/education/how-to-create-a-read-only-user-in-postgresql/#:~:text=Conclusion-,To%20create%20a%20read%2Donly%20user%20in%20the%20PostgreSQL%20database,schema%2C%20and%20tables%20in%20it.) for creating granting a Read-Only privilege to the `Customer` user/role but on a table that would be created in the future.

I was able to successfully grant `Read Only` priviliges to the `customer` role and execute this task using the following steps:
<div align="center">

![image](https://i.imgur.com/QFuY0L0.gif)
</div>

In order to grant `Read Only` priviliges to the `Customer` role, I wrote the following query:
```
ALTER DEFAULT PRIVILEGES FOR ROLE admin IN SCHEMA public GRANT SELECT ON TABLES TO customer;
```

As per this query, only the `SELECT` privilege was granted to the `customer` role since it was to only have `Read Only` access. In order to ensure that the specified privilege had been successfully, I executed the `\ddp` command to view the assigned permissions of the `customer` role. The output of this command was:
<div align="center">

![image](https://i.imgur.com/ACezK3m.png)
</div>

In the above output, the meaning of `customer=r/admin` was that the customer had been assigned the `READ` privilege, whic is associated with admin. 

Additionally, in order to allow these roles to also access the database, I used the following queries to allow them to login using [this resource](https://stackoverflow.com/a/55428943/21819272) as a reference:
- admin 
  ```
  ALTER ROLE admin WITH LOGIN;
  ```
- employee
  ```
  ALTER ROLE employee WITH LOGIN;
  ```
- customer 
  ```
  ALTER ROLE customer WITH LOGIN;
  ```

  Running the above queries successfully gave these roles the privilege to login and the output of running the above queries was as follows:
<div align="center">

![image](https://i.imgur.com/fZ2BuWr.png)
</div>

___________________

<div align="center">

## **Task 3: Host Based Authentication**
</div>

In postgreSQL, privileges can be considered as permissions assigned to `Roles`. These permissions or privileges help define which user would get which permission, allowing various users to interact with a database with a specified level of permissions.          

The resources utilised for working on this task included the following articles:
- [pg_hba.conf Explained](https://yuweisung.medium.com/postgresql-pg-hba-conf-explained-part1-3792de3d64c2)
- [Securing PostgreSQL Using HBA](https://itsmetommy.com/2021/08/06/securing-postgresql-using-host-based-authentication-hba/)


## **3.1. Understanding `pg_hba.conf`**

>Explain the configuration of PostgreSQL's pg_hba.conf file to enable host-based authentication

The `pg_hba.conf` file is a configuration file for PostgreSQL, which stands for `PostgreSQL Host-Based Authentication`. The responsibilities of this configuration file include:
- Which hosts are allowed to connect through which role
- How clients are to be authenticated, using an authentication type
- Which postgresql username they can use and database they can access 

The included fields where the records and data related to host-based authentications are as follows:
- **Type of connection**: This can be local or host. Here local refers to the local domain socket, which can be only accessed by local processes and Host is used for forming TCP connections
- **Database Name**: The second field includes the database that a role would be allowed to access. This can be a specific database or `all` for allowing access to all databases
- **Role name**: The third field is where roles are assigned to for a host-based connection. This can also be a specific role or `all` for every existing role.
- **Address**: The fourth field is for adding the Host Address, which can include an IPv4 or IPv6 address, as an example. Loopback addresses are also mentioned here but in the case of peer connection, this field can be left blank.
- **Authentication method**: The last field defines the type of authentication that is to be allowed before connecting with a host. For example, an `md5` encrypted password can be set or `trust` can be used for allowing connection without any password.
  
This configuration file can be accessed from the following path:
```
  /etc/postgresql/<version>/main/pg_hba.conf   
``` 
In my case, this `pg_hba.conf` file was found in `/etc/postgresql/16/main/pg_hba.conf` and the default configuration file looked like as follows:

<div align="center">

![image](https://i.imgur.com/T3YjEvM.png)
</div>

____________
## **3.2 Connecting Localhost With All Roles**
>Allow connections from localhost for all roles.

As per my interpretation of this task, we were supposed to allow all of the roles such as `admin`, `employee` and `customer`, to be accessed from Localhost. However, in the `pg_hba.conf` file, all of the roles are already allowed to be connected from the localhost address of `127.0.0.1`. 

Therefore, in order allow an even secure authentication type, I changed the `scram-sha-256` authentication type with `md5`. This change allowed the user to login using all roles, using the `127.0.0.1` address. Additionally, I also assigned my own Host address to the admin. The output of this was:
<div align="center">

![image](https://i.imgur.com/zUb7XeH.png)
</div>

After this change, I was able to login from the `127.0.0.1` address using all the roles using the following queries, after exiting PostgreSQL:
- admin 
  ```
  psql -h 127.0.0.1 -U admin -d ecommerce
  ```
- employee
  ```
  psql -h 127.0.0.1 -U employee -d ecommerce
  ```
- customer 
  ```
  psql -h 127.0.0.1 -U customer -d ecommerce
  ```
In the above queries, `-h` was used to define the localhost address, `-U` was used to define the role that we wished to enter and `-d` was used to tell which database we wished to enter. The output of running these commands was as follows:
<div align="center">

![image](https://i.imgur.com/4D5M6zT.png)
</div>

_________________

## **3.3 Allowing Admin To Connect From All IPs**

>Restrict connections from other IP addresses to the admin role only.

Though I faced issues with interpreting this task, I concluded that since `admin` role was supposed to have full access over the database, it would not make sense to give it access to only a single host address. From my understanding, the admin would have the privilege to access the database from all hosts.

In order to execute this understanding, I utilised this StackOverflow post titled `How to configure PostgreSQL to accept all incoming connections`. According this post, `0.0.0.0/0` would have to be added to the `pg_hba.conf` file. This was done by adding the following line to the configuration file:
<div align="center">

![image](https://i.imgur.com/csGlOzp.png)
</div>

It should be noted that in order to make this connection work, we would need to also configure the `postgresql.conf` file to listen on addresses other than `localhost`. This was done by replacing `#listen_addresses = 'localhost'` under the `connection settings` with `listen_addresses = '*' `.
__________________

<div align="center">

## **Task 4: Table and Schema Creation**
</div>

Having given the required Host-Based Authentication to all of the created roles, including `admin`, `employee` and `customer`, I logged in as the `admin` role and connected with the `ecommerce` database from outside of the PostgreSQL server using the following command:
```
psql -U admin -d ecommerce -h 127.0.0.1
```

The above query allowed me to enter the `ecommerce` database as the `admin` and though it was not mentioned explicitly in the assignment, I believed that since `admin` was already given full access to `ecommerce`, allowing it to create tables and schemas would reduce confusion when commands like `\dn` for listing schemas and `\dt` for listing tables would be run, as the columns for owner would have mentioned `postgres` had I not logged in as `admin`. 

## **4.1. Creating Product Schema**
> Create a schema named product.

As specified in this sub-task, I was to create a schema called `product`. In order to do so, I first granted the access of the `public` schema to admin, which was earlier only granted the access to the `ecommerce` database. The go-through of this sub-task is as follows:
<div align="center">

![image](https://i.imgur.com/gKFCC1v.gif)
</div>

This was done by running the following query:
```
GRANT ALL PRIVILEGES ON SCHEMA public TO admin;
``` 
The output of the above query was `GRANT ALL PRIVILEGES`, which showed that the privilege had been granted successfully. In order to create the `product` schema, I wrote the following query:
```
CREATE SCHEMA product;
```
The output of the above was `CREATE SCHEMA` helped me understand that the schema had been created but to ensure, I ran the `\dn` command and its output proved that the `product` schema had truly been created:
<div align="center">

![image](https://i.imgur.com/J92EGMN.png)
</div>

__________________

## **4.2. Creating `products` Table**
> Create a table named products in the public schema with columns such as
product_id, product_name, price, and stock_quantity.

In order to create a table called `products` with the `product_id`, `product_name`, `price` and `stock_quantity` included as the columns, I had to write a query that involved the `CREATE TABLE` SQL keywords. This sub-task was completed in the following way:
<div align="center">

![image](https://i.imgur.com/l9WB0sP.gif)
</div>

The query that was written to create a table called `products` with the specified columns included, was as follows:
```
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR,
    price numeric INT,
    stock_quantity INT);
```
In order to ensure this table's creation, I ran the `\dt` command for displaying all the existing tables. Since the output included the `products` table, I was able to conclude that the table had been successfully been created:
 
<div align="center">

![image](https://i.imgur.com/hoKh5yL.png)
</div>

_____________

## **Task 5: Generate Self-Signed Certificates**
</div>

SSL stands for Secured Sockets Layer and is also known as TLS or Transport Layer Security. Regardless of what it is called, SSL is a technology which is used for creating encrypted connection between servers and clients.

In the context of PostgreSQL, SSL can be used for creating a similar secure environment between the Postgres server and the role/user trying to access it. Doing so would reduce the chances of data being stolen or the security of PostgreSQL server being compromised.

As per this task, we were asked to generate a self-signed certificates, which is a digital document that is used to authenticate the identty of the user tying to access the server. It also helps with ensuring that the data transfered between the server and user would not get altered mid-way.

## **5.1. Generating Self-Signed SSL certificates**
> Generate self-signed SSL certificates for the PostgreSQL server. You can
use the openssl command or an SSL certificates generation tool.

In order to generate a self signed SSL certificates, I utilised [this guide](https://dev.to/yugabyte/how-to-enable-ssl-for-postgres-connections-5321) from Dev.To and I was able to successfully complete generate a key using the mentioned steps. The mentioned steps included:

- Generating certificates Authority (Ca)
  ```
  openssl genrsa 2048 > ca.key
  openssl req -new -x509 -nodes -days 365000 -key ca.key -out ca.cert
  ```
  The output of running these commands is being provided below. After running this CA Generating command, I was asked to enter the required details, which are as follows:
  <div align="center">

  ![image](https://i.imgur.com/KFSOGNV.png)
  </div>

- Generating Server Key
  ```
  openssl req -newkey rsa:2048 -nodes -days 365000 -keyout server.key -out server.csr
  ```
  The output of the above command helped me successfully generate the Server Key. Here, I was asked once again to fill in the required details and once done, I was asked to enter a password that would be sent with the certificates request:
  <div align="center">

  ![image](https://i.imgur.com/hgYEEUw.png)
  </div>

- Generating certificates Signing Request
  ```
  openssl x509 -req -days 365000 -set_serial 01 -in server.csr -out server.cert -CA ca.cert -CAkey ca.key
  ```
  The output of this specific command for generating the certificates Signing Request was successful, as the certificates was created correctly as follows:
  <div align="center">

  ![image](https://i.imgur.com/mFdTo5R.png)
  </div>  

With this all done, this task came to a conclusion as I was able to successfully generate the following Self-Signed certificates:
- ca.cert: Certificate authority certificate
- server.key: Key file
- server.cert: Certificate file

For their better security, I ran the following command to give them only `rw` or Read & Write permissions. The command ran was:
```
chown 600 server.key ca.cert server.cert
```
______

## **Task 6: Configure PostgreSQL**
</div>

## **6.1. Enabling TLS In `postgresql.conf`**
> Generate self-signed SSL certificates for the PostgreSQL server.  

For being able to initiate SSL for our PostgreSQL server, it was important to enable TLS and specify the path of the SSL certificates in the postgres.conf file. For being able to do this, I refered to [this tutorial](https://www.cherryservers.com/blog/how-to-configure-ssl-on-postgresql) by CherryServers.

To start, I entered into the directort which stored the `postgresql.conf` file, that is stored in `/etc/postgresql/16/main/`. For users with a different version of Postgres, the `16` in this file path would be replaced with their specific Postgres version. Regardless, I was able to make changes to this configuration file after entering it using the following command:
```
sudo nano postgresql.conf
```

Inside the configuration file, I ensured that under the `connection settings`, the `listen_addresses` was allowed to be connected with all the external hosts, including localhost as well. I was able to ensure this because I had replaced `#listen_addresses = 'localhost'` with the following:
```
listen_addresses = '*'
```

Additionally, I also made the following changes to the SSL section in the same configuration file, for enabling SSL/TLS and also for specifying the certificate paths. This was done by uncommenting and adding the following lines to the SSL section:
```
ssl = on
ssl_ca_file = '/root.crt'
ssl_cert_file = '/server.crt'
ssl_key_file = '/server.key'
ssl_ciphers = 'HIGH:MEDIUM:+3DES:!aNULL' # allowed SSL ciphers
ssl_prefer_server_ciphers = on
``` 
The addition of the above lines appeared as followed, in the configuration file:
  <div align="center">

  ![image](https://i.imgur.com/SBydYnq.png)
  </div>  

The reason for why the file path for the certificates was chosen as above was that I had generated them in my `/home` directory. In the following task, I continued with configuring another configuration file: `pg_hba.conf`. 

______

## **Task 7: Configure pg_hba.conf**
</div>

## **7.1. Allowing TLS In `pg_hba.conf`**
> Generate self-signed SSL certificates for the PostgreSQL server. You can

For this task, I again refered to [this tutorial](https://www.cherryservers.com/blog/how-to-configure-ssl-on-postgresql) by CherryServers. Inside the `/etc/postgresql/16/main/` directory, I ran the `sudo nano pg_hba.conf` command for being able to modify this file. With the SSL enabled in `postgresql.conf`, it was also necessary to have `pg_hba.conf` to allow SSL connections with my PostgreSQL server. 

This was done by making the following changes to the `pg_hba.conf` file, which is found in the same directory as `postgresql.conf`:
```
host    all customer 192.168.1.130/32 md5
hostssl all customer 192.168.1.130/32 md5
```
The output of adding these lines to the `pg_hba.conf` file was as follows:
  <div align="center">

  ![image](https://i.imgur.com/4cUHGVQ.png)
  </div>  

The first line was added to allow connection from specifc IPv4 local address of `192.168.1.130/32` for a specific role of `customer`. The second line was added for enabling SSL and allowing connection from the previously mentioned specific address. 

After the entire changes done on `postgresql.conf` and `pg_hba.conf` files had been saved, I restarted PostgreSQL using the following command:
```
service postgresql restart
```
___________________

_____

## **Task 8: Testing**
</div>

## **8.1. Connecting To `ecommerce` Using Roles**
> Test the setup by connecting to the ecommerce database using the roles you've created.

As already checked in Sub-Task [**3.2 Connecting Localhost With All Roles**](#32-connecting-localhost-with-all-roles), I was able to access the `ecommece` database using the following commands that allowed me to login as the following roles:
- admin 
  ```
  psql -h 127.0.0.1 -U admin -d ecommerce
  ```
- employee
  ```
  psql -h 127.0.0.1 -U employee -d ecommerce
  ```
- customer 
  ```
  psql -h 127.0.0.1 -U customer -d ecommerce
  ```
The output of these mentioned commands allowed me to successfuly enter the `ecommerce` database:
<div align="center">

![image](https://i.imgur.com/4D5M6zT.png)
</div>

## **8.2. Testing Privileges Of Roles**
> Ensure that admin can perform any action, employee can insert and update products, and customer can only read products.

Given that we had already entered assigned specific privileges to `admin`, `employee` and `customer`, it was necessary to ensure that these roles would be able to perform the tasks that they did had been provided the privilege to. To ensure that this was the case, I ran some tests for on the following roles:
- admin - testing all privileges
  ```
  psql -h 127.0.0.1 -U admin -d ecommerce
  ```
____________________

<div align="center">

## **Conclusion**
</div>

After the completion of these 4 tasks, which greatly depended on building relationships between tables, I was able to learn and understand the concept of primary keys, foreign keys, constraints, joins and alters. Through this document, I was also able to demonstrate and perform the assigned tasks, which helped me gain a lot of practical understanding of PostgreSQL and SQL.

--------
