
```
SELECT ...
  FROM from_part
  [WHERE ...]
  [GROUP BY ...]
  [ORDER BY ...]
  [LIMIT offset , count]
```

_from\_part_:
```
tbl_main_name [alias_name]
   [ LEFT JOIN tbl_name2 [alias_name2] ON tbl_main.colum_name = tbl2.colum_name ]
   ...
```

各部分含义:
  * select\_expr - 返回的结果列
    1. 可以是字段名或sql函数
      1. 无GROUP BY时，支持<a href='http://code.google.com/p/flashdb/wiki/SQL_function_CN#字段结果函数'>字段结果函数</a>
      1. 有GROUP BY时，支持<a href='http://code.google.com/p/flashdb/wiki/SQL_function_CN#分组聚合函数'>分组聚合函数</a>
    1. 可使用AS指定返回的列名
    1. 可使用 **或 tbl.** 返回所有列
  * FROM - 检索的表
    1. 第一张表为主表
    1. 支持 LEFT JOIN , 但必须与主表关联，且仅支持单字段相等的关联关系
      1. 有GROUP BY时，仅支持用分组字段进行关联
    1. 不支持 INNER JOIN , 但很多情况下可以用 IN (SELECT ... FROM ...) 达到类似效果
    1. 仅主表(第一张表)的字段可作为函数参数
    1. 表可以指定别名，以便在该sql的其他部分使用
  * WHERE - 条件过滤
    1. 未索引的字段，不可以进行检索
    1. 支持常见的<a href='http://code.google.com/p/flashdb/wiki/SQL_Select_CN#WHERE_expression'>条件表达式语法</a>
    1. 支持<a href='http://code.google.com/p/flashdb/wiki/SQL_function_CN#过滤函数'>过滤函数</a>
  * GROUP BY - 分组计算
    1. 仅支持单一字段分组，不支持多字段分组
    1. 若返回分组字段，则该字段必须出现在SELECT部分的最前面
    1. 分词字段或多值字段，每个词/值是单独各算一次的
  * ORDER BY - 结果排序
    1. 支持多重排序，逗号间隔
    1. 支持使用 DESC 按倒序排
    1. 无GROUP BY时，支持<a href='http://code.google.com/p/flashdb/wiki/SQL_function_CN#排序函数'>排序函数</a>
    1. 有GROUP BY时，仅支持结果字段的排序
      1. 聚合函数的结果，可使用结果列的别名进行排序。
  * LIMIT - 结果行数限制
    1. 第一个值表示偏移行数，第二个值表示最多返回的行数
    1. 只有一个值时，表示最多返回的行数

### WHERE expression ###
  * 比较操作符: `= <> != > < >= <=`
    1. 操作符左侧必须为字段名
    1. 操作符右侧必须为sql常量或变量?
    1. 字符串比较区分大小写
  * 布尔操作符: AND OR NOT
  * 空值判断: column\_name IS NULL
  * IN查询:
    1. 常量查询: column\_name IN (value1, value2, ...)
    1. 变量查询(仅用于JDBC编程执行): column\_name IN ?
    1. SELECT子查询: column\_name IN (SELECT column\_name2 FROM tbl\_name2 ...)
  * 过滤函数
  * 支持括号嵌套，以改变表达式计算优先级