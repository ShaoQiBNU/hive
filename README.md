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
