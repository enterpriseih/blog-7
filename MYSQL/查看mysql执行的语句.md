### 查看mysql执行的语句

- ```mysql
  //查看日志是否启动和记录目录
  SHOW VARIABLES LIKE "general_log%";
  ```

- ```mysql
  //若未启动，启动之
  SET GLOBAL general_log = 'ON';
  ```

- ```mysql
  //自定义日志目录
  SET GLOBAL general_log_file = 'c:\mysql.log';
  ```

  

