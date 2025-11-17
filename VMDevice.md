# 虛擬機配置

* 虛擬機配置
  * 共同配置 
    * ssh
  * VM1
    * IP `192.168.242.11`
    * Database (PostgreSQL)
    * Backend (Python Flask)
  * VM2
    * IP `192.168.242.12`
    * Backend (Python Flask)
  * VM3 (Manager)
    * IP `192.168.242.13` 
    * Load Balance (Nginx)
    * Apache Jmeter (有點問題)

## VM1

### PostgreSQL

1. 安裝

    ```bash
    sudo apt update
    sudo apt upgrade
    sudo apt install postgresql postgresql-contrib
    ```

2. 新增資料庫

    ```bash
    sudo -u postgres psql
    ```

    ```sql
    CREATE DATABASE testdb;
    CREATE USER testuser WITH PASSWORD 'password';
    GRANT ALL PRIVILEGES ON DATABASE testdb TO testuser;
    ```

3. 設定
    3.1 修改`pg_hba.conf`
    檔案位置：`/etc/postgresql/<version>/main/pg_hba.conf`
    給VM2連進來

    ```conf
    host    all             all             192.168.242.12/24           md5
   ```

    ![image](https://hackmd.io/_uploads/HyQN8O8Cel.png)

    3.2 修改`postgresql.conf`

    檔案位置：`/etc/postgresql/<version>/main/postgresql.conf`

    ```conf
    listen_addresses = '*' 
    ```

    ![image](https://hackmd.io/_uploads/SyJRM3EAlg.png)

    3.3 重新啟動PostgreSQL

    ```bash
    sudo systemctl restart postgresql
    ```

4. 創建資料庫

    ```sql
    -- 創建 servers 表
    CREATE TABLE servers (
        id SERIAL PRIMARY KEY,
        hostname VARCHAR(255) NOT NULL,
        ip_address VARCHAR(15) NOT NULL,
        status VARCHAR(50) DEFAULT 'active'
    );

    -- 創建 requests 表
    CREATE TABLE requests (
        id SERIAL PRIMARY KEY,
        timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        status VARCHAR(50) DEFAULT 'pending'
    );

    -- 創建 load_balancer_logs 表
    CREATE TABLE load_balancer_logs (
        id SERIAL PRIMARY KEY,
        timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        action VARCHAR(255),
        server_id INT REFERENCES servers(id) ON DELETE CASCADE
    );

    ```

### Flask

1. 安裝所需套件

    ```bash
    sudo apt update
    sudo apt install python3-flask python3-psycopg2
    ```

## VM2

### Flask

1. 安裝所需套件

    ```bash
    sudo apt update
    sudo apt install python3-flask python3-psycopg2
    ```

## VM3

### Nginx

負載平衡

1. 安裝Nginx

    ```bash
    sudo apt update
    sudo apt install nginx
    ```

2. 安裝python requests套件 (用來發送請求)

    ```bash
    pip install requests
    ```

### Apache Jmeter

壓力測試、數據顯示工具(吞吐量、回應時間等等)

1. 下載jemter:[Apache Jmeter下載](https://jmeter.apache.org/download_jmeter.cgi)

    ![image](https://hackmd.io/_uploads/rk292E4ebe.png)

2. 解壓縮

    ```bash
    tar -xvzf apache-jmeter-5.6.3.tgz
    
    cd apache-jmeter-5.6.3
    ```

3. 安裝java
    jemeter是用java寫的所以需要jdk環境(java 8以上)

    ```bash
    sudo apt update
    sudo apt install openjdk-11-jdk
    ```

4. 設定環境變數

    ```bash
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
    export PATH=$JAVA_HOME/bin:$PATH
    ```

    更新配置文件

    ```bash
    source ~/.bashrc
    ```

5. 安裝圖形介面依賴、java AWT庫(GUI功能)

    因為有出現以下錯誤才安裝的
    `An error occurred: Could not initialize class java.awt.Toolkit`

    ```bash
    sudo apt update
    sudo apt install libx11-dev libxaw7 libxmu6 libxtst6 libpng-dev
    ```

6. 執行jmeter

    ```bash
    ./bin/jmeter
    ```

    jmeter介面
    ![image](https://hackmd.io/_uploads/Sy97WrVl-g.png)
