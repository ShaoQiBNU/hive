hive相关用法
===========

# 一. get_json_object和json_tuple用法

> 假设数据表中有一个字段是json格式的数据，如下所示：

```
数据表：test

字段名：data

数据格式：
{
 "store":
        {
         "fruit":[{"weight":8,"type":"apple"}, {"weight":9,"type":"pear"}],  
         "bicycle":{"price":19.95,"color":"red"}
         }, 
 "email":"amy@only_for_json_udf_test.net", 
 "owner":"amy" 
}

```
> 当采用get_json_object函数时，可以这样使用：其中，get_json_object函数的第一个参数为json对象变量，第二个参数使用$表示json变量标识，然后用.或[]读取对象或数组；

```sql

/* 获取单层值 */
select get_json_object(data, '$.owner') from test;

结果如下：
amy

/* 获取多层值 */
select get_json_object(data, '$.store.bicycle.price') from test;

结果如下：
19.95

/* 获取单层值 */
select get_json_object(data, '$.store.fruit[0]') from test;

结果如下：
{"weight":8,"type":"apple"}

```

> 当采用json_tuple函数时，可以一次获取多个对象并且可以组合使用，

```sql

select json_tuple(data,'owner','email') from test;

结果如下:
amy amy@only_for_json_udf_test.net

```

# 二. reflect
> reflect函数可以支持在sql中调用java中的自带函数，秒杀一切udf函数。使用案例如下——所有记录执行相同的java内置函数

```sql
hive中建一张表test_udf:column1(int),column2(int), 表中数据如下：

1,2

2,3

3,4

4,5

5,6


select reflect("java.lang.Math","max",column1,column2) from test_udf

结果：

2

3

4

5

6

select 语句的第一个参数为java依赖包，第二个参数为函数功能，第三个参数为目标对象，可以是某个具体的值，也可以是表中的字段。

```
# 三. 条件判断

> hive中if语句语法格式如下：

```sql

语法: if(boolean testCondition, T valueTrue, T valueFalseOrNull)
返回值: T

说明:  当条件testCondition为TRUE时，返回valueTrue；否则返回valueFalseOrNull

举例：
select if(1=2,100,200) from dual;

200


select if(1=1,100,200) from dual;

100

```
