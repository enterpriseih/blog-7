### MySql游标

- 存储过程
- 定义
- 作用
- 使用方法



#### 存储过程

**语法**

**CREATE PROCEDURE**  **过程名([[IN|OUT|INOUT] 参数名 数据类型[,[IN|OUT|INOUT] 参数名 数据类型…]]) [特性 ...] 过程体**

```sql
DELIMITER //
  CREATE PROCEDURE myproc(OUT s int)
    BEGIN
      SELECT COUNT(*) INTO s FROM students;
    END
    //
DELIMITER ;
```

- 分隔符： 默认为“；”， 没有分隔符编译器会默认将存储过程当成SQL执行；事先用

  “DELIMITER//”声明当前段分隔符， 让编译器把两个“//”之间的内容当做存储过程，不执行这些代码，“DELIMITER ;”还原分隔符；

- 参数IN OUT INOUT， 类似于函数调用宏；IN 调用存储过程时指定，不可在存储过程内改变， OUT 可改变并返回， INOUT：调用存储过程时指定，可改变返回；
- 过程体： BEGIN和END之间；



**具体使用**

- IN OUT INOUT 使用：OUT不用输入时候指定值， IN INOUT需要；

- 变量使用：

  DECLARE 变量名1[, 变量名2...] 数据类型[默认值]， 数据类型为MySql的数据类型；

  - 变量赋值：

    SET 变量名 = 变量值[,变量名 = 变量值... ]

  - 用户变量：

    以@开头