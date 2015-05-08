```sql

DROP TABLE IF EXISTS sample_fulltext;
CREATE TABLE sample_fulltext
(
id INT auto_increment,
ftitle VARCHAR tokenized,
ftext VARCHAR tokenized,
fprob double indexed,
PRIMARY KEY (id)
) comment 'sample fulltext search'
;
```

```sql

TRUNCATE TABLE sample_fulltext;
INSERT INTO sample_fulltext (fprob,ftitle,ftext)
VALUES (0.1, 'widely', 'A serious flaw in two widely used security standards could give anyone access to your account information at Google, Microsoft, Facebook');
INSERT INTO sample_fulltext (fprob,ftitle,ftext)
VALUES (0.2, 'guaranteed', 'Google has extended its guaranteed support for Chrome OS on vendors Chromebooks to five years');
INSERT INTO sample_fulltext (fprob,ftitle,ftext)
VALUES (0.1, 'standards', 'Khloe Slammed By Fans : Khloe Kardashian is taking some heat for a shot she posted with rumored new boyfriend French Montana');
INSERT INTO sample_fulltext (fprob,ftitle,ftext)
VALUES (0.2, 'Google, Microsoft', 'Google topped all companies in terms of best pay and benefits in a new survey by online jobs site Glassdoor');
INSERT INTO sample_fulltext (fprob,ftitle,ftext)
VALUES (0.1, 'personal', 'Deleting personal information online is costly and time-consuming for Web companies. Those difficulties are now set to be magnified in Europe for Google, Microsoft and others');
INSERT INTO sample_fulltext (fprob,ftitle,ftext)
VALUES (0.3, 'VirusTotal', 'Google VirusTotal Uploader for OS X comes at the heels of a similar tool for machines running on Microsoft Windows platform');
FLUSH TABLE sample_fulltext;
```

```sql

SELECT id,ftitle,ftext,Highlight('microsoft', ftext) FROM sample_fulltext
where Keyword('microsoft', ftitle,ftext)
order by Keyword('microsoft', ftitle,5,ftext,1);
```

```sql

SELECT id,ftitle,ftext,Highlight('microsoft', ftext) FROM sample_fulltext
where AnyKeyword('microsoft google', ftitle,ftext)
order by Keyword('microsoft google', ftitle,5,ftext,1);
```