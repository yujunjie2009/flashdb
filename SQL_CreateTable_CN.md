
```
CREATE TABLE tbl_name ( column_definition ..., PRIMARY KEY(..) ) [table_options]
```

_column\_definition_:
```
col_name type [INDEXED] [TOKENIZED] [NOSTORED] [AUTO_INCREMENT] [NOT NULL]
              [COMMENT 'string'] [TOKENIZER 'string'] [CODEC 'string']
```

_table\_options_:
```
[ENGINE 'string'] [COMMENT 'string'] [TOKENIZER 'string'] [WRITE_BUFFER_SIZE digit] [TEMP_IN_MEMORY]
```

各部分含义:
  * PRIMARY KEY - 指定主键字段名(若多个逗号间隔). FlashDB的表必须在指定主键
  * column\_definition - 字段定义，各字段逗号间隔
    1. type - <a href='http://code.google.com/p/flashdb/wiki/DataType_CN'>数据类型</a>
    1. INDEXED - 字段建索引，默认不建
      1. 未索引的字段，不可以进行检索。即不可以出现在WHERE/GROUP/ORDER部分
    1. TOKENIZED - 文本字段建分词索引，默认不分
      1. 分词字段检索WHERE/GROUP时，是根据其中每个词进行单独计算的
    1. NOSTORED - 不存储字段值，用于减少数据存储量
      1. 该字段必须建索引，否则没意义
      1. 该字段可以进行检索，但不能读取值。即可以出现在WHERE/GROUP/ORDER部分，但不能出现在SELECT部分。
      1. UPDATE修改数据是，必须指定该字段的值，因为原值已无法获取
    1. AUTO\_INCREMENT - 字段值自增。仅用于INT/BIGINT类型的主键字段
    1. NOT NULL - 字段值非空
    1. COMMENT - 字段备注说明
    1. TOKENIZER - 文本分词时，指定分词器. 未指定时采用默认分词器
    1. CODEC - 二进制存储的为Java对象时，指定相应的Java类名
  * table\_options - 表的选项参数
    1. ENGINE - 表引擎
      1. 指定'shard'时，表示该表数据分片，实际数据存储在各分片服务器的同名表中
    1. COMMENT - 字段备注说明
    1. TOKENIZER - 表的默认分词器
      1. 未指定时，表的默认分词器采用全局配置的默认分词器
      1. 检索时，所有字段使用该分词器
      1. 写入时，若字段上有指定分词器则用字段指定的分词器，若为指定则用表的默认分词器
    1. CODEC - 二进制存储的为Java对象时，指定相应的Java类名
    1. WRITE\_BUFFER\_SIZE - 写入缓冲区大小，以行为单位
      1. 较大的缓冲区可以提高写入性能，但也增加内存溢出的风险
    1. TEMP\_IN\_MEMORY - 整张表的数据存储在内存中
      1. 可有效提高读写性能，但仅适用于数据量较小的表
      1. 服务器关闭后，该表的数据将丢失