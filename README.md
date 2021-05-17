docker mariadb 同步(區分 master 及 slave) 

docker-compose.yml 
master.cnf 
slave.cnf 
 
指令: docker-compose up 
兩台DB啟動後，須執行權限開放及同步 

Master DB(進入DB操作) 
 
CREATE USER 'slave_user'@'%' IDENTIFIED BY 'password'; 
GRANT REPLICATION SLAVE ON *.* TO 'slave_user'@'%'; 
FLUSH PRIVILEGES; 
打SHOW MASTER STATUS; 找到MASTER_LOG_FILE 跟MASTER_LOG_POS 的值 
 
SLAVE DB(進入DB操作) 
CHANGE MASTER TO  
  MASTER_HOST='database_master', 
  MASTER_PORT=3306, 
  MASTER_USER='slave_user', 
  MASTER_PASSWORD='password', 
  MASTER_LOG_FILE='mysql-bin.000001', //剛剛在master看到的值 
  MASTER_LOG_POS=529; //剛剛在master看到的值 

START SLAVE; 
 
SHOW SLAVE STATUS; 顯示的狀態沒錯誤就可以了 
 
嘗試在master內插入值或新增資料庫看有沒有正確同步 



  

