类型对照表:
| **SQL** | **Java** | **`ResultSet.get(...)`** | **SQL常量示例** |
|:--------|:---------|:-------------------------|:--------------------|
| VARCHAR | String | getString | 'abc' |
| BIT | boolean | getBoolean | true |
| INT | int | getInt | 123 |
| BIGINT | long | getLong | 123456 |
| FLOAT | float | getFloat | 123.3 |
| DOUBLE | double | getDouble | 1234.432 |
| DECIMAL | `BigDecimal` | getBigDecimal | 123.2 |
| DATE | Date | getDate | '2012-01-02' |
| TIMESTAMP | Timestamp | getTimestamp  | '2012-01-02 13:30:20' |
| VARBINARY | `byte[]` | getBytes | _不支持_ |
| `x[]` | `x[]` | `(x[])getObject` | ['abc','cba','ddd'] |

<br />
FlashDB数据类型的特殊之处:
  * 所有类型都不能指定长度;
  * VARCHAR 最多存储2^31个字节, UTF-8编码存储
    1. 保留末尾空格. 因此 'abc ' != 'abc'
    1. 区分大小写. 因此 'aBc' != 'abc'
    1. 若字段TOKENIZED分词，检索WHERE/GROUP时，是根据其中每个词进行单独计算的
    1. FlashDB的字符串常量，使用单引号或双引号均可。双引号中可嵌单引号，无需转义。
  * BIT 相应的SQL常量为true或false
  * INT/BIGINT 作为单字段主键时,值不能为0
  * DECIMAL 最多存储4位小数, 取值范围 ±2^59
  * VARBINARY 最多存储2^31个字节，不支持索引
    1. 当存储的为Java对象时，在客户端工具SQuirreL SQL Client中，使可使用Binary2ObjString函数转换后查看。例如
```
SELECT Binary2ObjString(col_name) FROM tbl_name
```
  * 支持多值字段，如 `VARCHAR[]` , `INT[]`
    1. 字段值的Java数据类型为相应的数组类型
    1. 检索WHERE/GROUP时，是根据其中每个值进行单独计算的
    1. 相应的SQL常量以`[]`括起来,逗号间隔
    1. 客户端工具`SQuirreL SQL Client`无法正常查看多值字段的值，可使用`Array2String`函数转换后查看。例如
```
SELECT Array2String(col_name) FROM tbl_name
```