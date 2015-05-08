| **SQL** | **Java** | **`ResultSet.get(...)`** | **SQL const example** |
|:--------|:---------|:-------------------------|:----------------------|
| VARCHAR | String | getString | 'abc' |
| BIT | boolean | getBoolean | true |
| INT | int | getInt | 123 |
| BIGINT | long | getLong | 123456 |
| FLOAT | float | getFloat | 123.3 |
| DOUBLE | double | getDouble | 1234.432 |
| DECIMAL | `BigDecimal` | getBigDecimal | 123.2 |
| DATE | Date | getDate | '2012-01-02' |
| TIMESTAMP | Timestamp | getTimestamp  | '2012-01-02 13:30:20' |
| VARBINARY | `byte[]` | getBytes | _unsupport_ |
| `x[]` | `x[]` | `(x[])getObject` | ['abc','cba','ddd'] |