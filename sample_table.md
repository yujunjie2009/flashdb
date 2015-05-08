```sql

DROP TABLE IF EXISTS sample_table;
CREATE TABLE sample_table
(
fa VARCHAR indexed,
fb INT indexed,
fc BIGINT indexed,
fd FLOAT indexed,
fe DOUBLE indexed,
ff DECIMAL indexed,
fg DATE indexed,
fh TIMESTAMP indexed,
fi BIT indexed,
fwords VARCHAR[] indexed COMMENT 'Multipy value',
ftext VARCHAR indexed tokenized COMMENT 'Fulltext search',
PRIMARY KEY (fa)
) COMMENT 'sample data'
;
```

```sql

TRUNCATE TABLE sample_table;
INSERT INTO sample_table (fa,fb,fc,fd,fe,ff,fg,fh,fi,fwords,ftext)
VALUES ('aaa', 111, 111000, 11.1, 111.001, 11.3, '2011-01-01', '2011-01-01 11:11:11', false
, ['AA','BB','CC'], 'A serious flaw in two widely used security standards could give anyone access to your account information at Google, Microsoft, Facebook');
INSERT INTO sample_table (fa,fb,fc,fd,fe,ff,fg,fh,fi,fwords,ftext)
VALUES ('bbb', 222, 222000, 22.2, 222.002, 22.3, '2012-02-02', '2012-02-02 12:22:22', true
, ['AA','BB','BB'], 'Google has extended its guaranteed support for Chrome OS on vendors Chromebooks to five years');
INSERT INTO sample_table (fa,fb,fc,fd,fe,ff,fg,fh,fi,fwords,ftext)
VALUES ('ccc', 333, 333000, 33.3, 333.003, 33.3, '2013-03-03', '2013-03-03 13:33:33', false
, ['BB','CC'], 'A serious flaw in two widely used security standards could give anyone access to your account information at Google, Microsoft, Facebook');
INSERT INTO sample_table (fa,fb,fc,fd,fe,ff,fg,fh,fi,fwords,ftext)
VALUES ('ddd', 222, 111000, 44.4, 444.004, 44.3, '2014-04-04', '2014-04-04 14:44:44', true
, ['DD','CC','EE'], 'Google topped all companies in terms of best pay and benefits in a new survey by online jobs site Glassdoor');
INSERT INTO sample_table (fa,fb,fc,fd,fe,ff,fg,fh,fi,fwords,ftext)
VALUES ('eee', 333, 222000, 55.5, 555.005, 55.3, '2015-05-05', '2015-05-05 15:55:55', false
, ['AA','BB','CC'], 'Deleting personal information online is costly and time-consuming for Web companies. Those difficulties are now set to be magnified in Europe for Google, Microsoft and others');
INSERT INTO sample_table (fa,fb,fc,fd,fe,ff,fg,fh,fi,fwords,ftext)
VALUES ('fff', 111, 111000, 66.6, 666.006, 66.3, '2016-06-06', '2016-06-06 16:16:16', true
, ['AA','BB','CC'], 'Google VirusTotal Uploader for OS X comes at the heels of a similar tool for machines running on Microsoft Windows platform');
UPDATE sample_table SET fc=222333, fd=22.33
WHERE fa='bbb';
DELETE FROM sample_table WHERE fa='aaa';
FLUSH TABLE sample_table;
```

```sql

SELECT * FROM sample_table;
SELECT fb, count(*) as c FROM sample_table group by fb;
SELECT * FROM sample_table where fa >'a' and fa < 'e' order by fa;
SELECT fa,fb,array2string(fwords) FROM sample_table where fwords = 'AA';
SELECT fa,fb,ftext FROM sample_table where ftext = 'google';
SELECT fa,fb,ftext,Highlight('microsoft', ftext) FROM sample_table where ftext = 'microsoft' order by Keyword('microsoft', ftext);
```