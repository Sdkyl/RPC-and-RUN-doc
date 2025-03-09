# **XdagJ Node 0.7.0 Run Document**



## System Environment

```
JDK : v17

Maven : v3.9.1

MySQL :Above v8.0
```



## Operation Steps

### **1. Clone Code**

- ***If the XdagJ project does not exist:***

  ```
  $ git clone https://github.com/XDagger/xdagj.git
  ```

- ***If the XdagJ project already exists:***

  ```
  $ cd xdagj && git pull
  ```

- ***Make sure the XdagJ project is in the master branch:***

  ```
  # View the current branch.
  $ git branch
  
  
  # Perform this operation when the current branch is not master.
  $ git checkout master
  ```

  

### **2. Configure MySQL**

- ***If you are configuring MySQL for the first time:***

  - *Install MySQL*

    ```
    # install MySQL.
    $ sudo apt install mysql-server
    
    
    # Log in to the MySQL database as the root user.
    $ sudo mysql -u root
    
    
    # change Password.
    mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_new_password';
    mysql> FLUSH PRIVILEGES;
    mysql> EXIT;
    ```

  - *Create a database for saving transaction history*

    ```
    # Log in to MySQL as the root user, using the password you just set.
    $ mysql -u root -p
    
    
    # Create a database named your_store_transaction_history_database_name.
    mysql> CREATE DATABASE your_store_transaction_history_database_name;
    
    
    # Check if the database was created successfully.
    mysql> SHOW DATABASES;
    ```

  - *Create a table*

    ```
    # Select the database you just created.
    mysql> USE your_store_transaction_history_database_name;
    
    
    ###
    1. Find the mysql_create_table.sql script, under the "xdagj/script" path.
    2. Create a table under this database, source [mysql_create_table.sql file path].
    ###
    mysql> source absolute_path_of_your mysql_create_table.sql script;
    
    # For example: 
    mysql> source /home/ubuntu/xdagj/script/mysql_create_table.sql;
    ```

- ***If you have already configured MySQL and have run nodes before, please perform the following steps:***

  - *Login to MySQL*

    ```
    $ mysql -u root -p
    ```

  - *Back up previous versions of transaction history*

    ```
    # Select the database where you previously stored transaction history
    mysql> USE your_store_transaction_history_database_name;
    
    
    # Back up previous versions of transaction history data
    mysql> RENAME TABLE t_transaction_history TO transaction_history_v_0_7_2;
    ```

  - *Re-create an empty t_transaction_history table*

    ```
    # Select the database where you previously stored transaction history.
    mysql> USE your_store_transaction_history_database_name;
    
    
    ###
    1. Locate the mysql_create_table.sql script in the xdagj/script folder.
    2. Create table t_transaction_history under this database, source [mysql_create_table.sql file path].
    ###
    mysql> source absolute_path_of_your mysql_create_table.sql script;
    
    # For example:
    mysql> source /home/ubuntu/xdagj/script/mysql_create_table.sql;
    
    
    # Check whether the table is created successfully.
    mysql> SHOW TABLES;
    ```



### **3. Execute mvn**

```
$ cd xdagj
$ mvn clean package -Dmaven.test.skip=true
```



### **4. Create a "run" Folder and Modify the Configuration Files**

- ***If you have not created a "run" folder before, please follow the steps below:***

  - *Create a "run" folder and copy the necessary files:*

    ```
    # Create a run folder, which is usually in the same directory as the xdagj project.
    $ mkdir run
    ```

  - *Copy the necessary files to the "run" folder:*

    ```
      Copy "xdag-mainnet.conf", "druid.properties", "log4j2.xml", "xdag.sh", "xdagj-0.8.0-shaded.jar" to "run" folder. Following is the path of the above files:
    
    - "xdag-mainnet.conf", "druid.properties", "log4j2.xml" are located in the xdagj/src/main/resources
    
    - "xdag.sh" is located in the xdagj/script
    
    - "xdagj-0.8.0-shaded.jar" is located in the xdagj/target
    ```

  - *Modify the configuration files:*

    - Modify druid.properties

      ```
      - Modify the url in the druid.properties file to: jdbc:mysql://localhost:3306/your_store_transaction_history_database_name?
      autoReconnect=true&useUnicode=true&characterEncoding=utf-8&&serverTimezone=UTC
      
      - The user name is root, and the password is the password (your_new_password) set in the mysql configuration before.
      ```

    - Modify xdag-mainnet.conf

      ```
      - "node.whiteIPs": Determines which nodes, under which IP addresses, can communicate with this node (Please ask the community if "node.whiteIPs" needs to be updated, added or deleted).
      
      - "fund.address": Set fund.address = "PKcBtHWDSnAWfZntqWPBLedqBShuKSTzS" (Required: Without this address, miner rewards cannot be distributed).
      
      - "node.generate.block.enable": Set node.generate.block.enable = true for mining nodes and node.generate.block.enable = false for exchanges.
      
      - "randomx.flags.fullmem": Please set randomx.flags.fullmem = false.
      
      - "fund.ration" and "node.ration" are the ratios of foundation reward and block node reward, respectively.
      
      - node.reject.transaction.address is the address to be denied service.
      
      - pool.whiteIPs is the pool whitelist. If set to ["0.0.0.0"], any pool can access the node. Otherwise, only specified IPs are allowed.
      ```

    - Modify xdag.sh

      ```
      - XDAG_VERSION: Modify XDAG_VERSION to "0.8.0".
      ```

- ***If you have already created the "run" folder and run the node before, follow these steps:***

  ```
  - Copy xdagj-0.8.0-shaded.jar to the run folder.
  
  - Modify the XDAG_VERSION in xdag.sh to XDAG_VERSION="0.8.0".
  
  - In xdag-mainnet.conf, set randomx.flags.fullmem = false.
  
  - Please ask the community if "node.whiteIPs" needs to be updated, added or deleted.
  ```

  

### **5. Download the SNAPSHOT File**

```
1. Create the /main/rocksdb/xdagdb directory under the run folder

2. SNAPSHOT download address: https://storage.xdagpool.com/.

3. Unzip the compressed package to get a SNAPSHOT folder.

4. Delete all files except SNAPSHOT files in the run/mainnet/rocksdb/xdagdb directory.

5. Copy the unzipped SNAPSHOT file to mainnet/rocksdb/xdagdb in the root directory, overwriting the existing SNAPSHOT folder.
```



### **6. Make Snapshot (Developer node steps: Ignore if not relevant)**

```
###
1. If you use an existing snapshot file, please ignore this step.
2. Please make sure to update file "xdagj-0.8.0-shaded.jar" before performing this step.
3. Snapshot file "SNAPSHOT" will be generated in directory mainnet/rocksdb/xdagdb.
###

# Run this command in the /run directory to make the snapshot.
$ sh xdag.sh --makesnapshot convertxamount
```



### 7. Verify the Version of xdag.sh

```
# Run this command in the /run directory to verify the version.
$ sh xdag.sh --version  // return: 0.8.0
```



### 8. Start XdagJ



### **9. Verify SNAPSHOT and MySQL Data**



