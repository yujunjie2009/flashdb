### 分组聚合函数 ###
| COUNT() | 计数 |  |
|:--------|:-------|:-|
| `COUNT(*)` | 计数 |  |
| COUNT(column\_name) | 字段去重计数 |  |
| SUM(column\_name) | 数值求和 |  |
| AVG(column\_name) | 数值平均值 |  |
| MAX(column\_name) | 最大值 |  |
| MAX(column\_cmp, column\_ret) | 最大值 | 取column\_cmp字段值最大的那行对应的column\_ret字段的值 |
| MIN(column\_name) | 最小值 |  |
| MIN(column\_cmp, column\_ret) | 最小值 | 取column\_cmp字段值最小的那行对应的column\_ret字段的值 |
| ARRAY(column\_name) | 汇总成多值 | 将指定字段的值,汇总成多值 |
| `ArrayDistinct(column_name)` | 汇总并去重 | 汇总成多值,并去重 |

示例:
```sql

SELECT fwords,COUNT(),COUNT(fc),MIN(fc),MIN(fc,fa),ARRAY(fb),ArrayDistinct(fb) FROM sample_table GROUP BY fwords```

### 字段结果函数 ###
| Array2String(column\_name) | 多值字段转为字符串 | 主要用于方便查看分析 |
|:---------------------------|:----------------------------|:-------------------------------|
| Binary2ObjString(column\_name) | 二进制字段值转为字符串 | 先反序列化为Java对象，再返回对象`.toString()，以方便查看分析` |
| Highlight(keywords, column\_name [, max\_length]) | 检索词高亮 | 将文本字段中的检索词,以标签`<em>...</em>`形式进行标注 |

示例:
```sql

SELECT fa,fwords,Array2String(fwords) FROM sample_table
SELECT id,ftext,Highlight('microsoft google', ftext) FROM sample_fulltext```

### 过滤函数 ###
| `LuceneQuery('query_string', default_field)` | 基于<a href='http://lucene.apache.org/core/4_7_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Overview'>Lucene查询语法</a>的检索过滤 |  |
|:---------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-|
| `Keyword('words_string', column_name [, column_name2 ...])` | 基于关键词的检索过滤 |  |
| `AnyKeyword('words_string', column_name [, column_name2 ...])` | 基于关键词的检索过滤 | 类似Keyword函数,区别是任一检索词出现即可 |

示例:
```sql

SELECT * FROM sample_fulltext WHERE LuceneQuery('ftitle:microsoft OR ftext:microsoft', ftext)
SELECT * FROM sample_fulltext WHERE Keyword('microsoft topped', ftitle, ftext)
SELECT * FROM sample_fulltext WHERE AnyKeyword('microsoft topped', ftitle, ftext)```

### 排序函数 ###
| `Keyword('words_string', column_name, boost [, column_name2, boost2])` | 基于TF/IDF模型的Lucene排序 | 权重越大的字段,匹配的结果排序越靠前 |
|:-----------------------------------------------------------------------|:----------------------------------|:-----------------------------------------------------|
| `AnlyKeyword('words_string', column_name, boost [, column_name2, boost2])` | 基于TF/IDF模型的Lucene排序 | 类似Keyword函数,区别是任一检索词出现即可 |
| `Probability(row_prob, 'words_string', column_name, prob [, column_name2, prob2])`)` | 基于概率模型的排序 | 检索概率越大的行或字段,匹配的结果排序越靠前 |
| `AssignOrder(column_name,front_values,behind_values)` | 指定的特定行的排序 | 指定排在最前或最后若干行的字段值 |

示例:
```sql

SELECT * FROM sample_fulltext ORDER BY LuceneQuery('ftitle:microsoft^5 OR ftext:microsoft', ftext)
SELECT * FROM sample_fulltext ORDER BY Keyword('microsoft', ftitle, 5, ftext, 1)
SELECT * FROM sample_fulltext ORDER BY AnyKeyword('microsoft', ftitle, 1, ftext, 5)
SELECT * FROM sample_fulltext ORDER BY Probability(fprob, 'microsoft', ftitle, 0.5, ftext, 0.5);
SELECT * FROM sample_fulltext ORDER BY Probability(fprob, 'microsoft', ftitle, 0.9, ftext, 0.1);
SELECT * FROM sample_fulltext ORDER BY AssignOrder(id, [2,1], [6,5])```