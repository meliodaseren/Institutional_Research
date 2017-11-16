## Oracle Database 11g Enterprise Edition

## SQL Developer

```
狀態 : 失敗 -測試失敗: Listener refused the connection with the following error:ORA-12505, TNS:listener does not currently know of SID given in connect descriptor
```

解決：SID 名稱不正確，向 DBA 詢問正確名稱


## SQL Reference

### Database version

```sql
SELECT * FROM v$version;
```

### Table information

```sql
SELECT table_name FROM all_tables;
SELECT COUNT(table_name) FROM all_tables;

-- FROM user_tables
-- FROM dba_tables
```

[ALL_TAB_COLUMNS](https://docs.oracle.com/cd/E11882_01/server.112/e40402/statviews_2103.htm#REFRN20277)

[ALL_CONSTRAINTS](https://docs.oracle.com/cd/E11882_01/server.112/e40402/statviews_1047.htm#REFRN20047)

```sql
SELECT * FROM all_tab_columns;
SELECT * FROM all_constraints;
```

[DUAL Table](https://docs.oracle.com/cd/E11882_01/server.112/e41084/queries009.htm#SQLRF20036)

```sql
SELECT sysdate FROM DUAL;
SELECT sys_context( 'userenv', 'current_schema' ) FROM DUAL;
```

### All tables and describe tables in a specific schema

```sql
SELECT
  C.OWNER, C.TABLE_NAME, C.COLUMN_ID, C.COLUMN_NAME, 
  DATA_TYPE, DATA_LENGTH, DATA_PRECISION, DATA_DEFAULT, 
  NULLABLE, COMMENTS
FROM
  ALL_TAB_COLUMNS C 
JOIN ALL_TABLES T ON 
  C.OWNER = T.OWNER AND C.TABLE_NAME = T.TABLE_NAME
LEFT JOIN ALL_COL_COMMENTS R ON
  C.OWNER = R.Owner AND 
  C.TABLE_NAME = R.TABLE_NAME AND 
  C.COLUMN_NAME = R.COLUMN_NAME
WHERE  
  C.OWNER  = "SCHEMA NAME"
ORDER BY C.TABLE_NAME, C.COLUMN_ID;
```

### Index

```sql
SELECT 
  I.TABLE_OWNER, I.TABLE_NAME, I.INDEX_NAME, I.INDEX_TYPE,
  I.UNIQUENESS, C.COLUMN_POSITION, C.COLUMN_NAME, C.DESCEND
FROM 
  ALL_INDEXES I JOIN ALL_IND_COLUMNS C
ON 
  I.TABLE_OWNER = C.TABLE_OWNER AND
  I.INDEX_NAME = C.INDEX_NAME
WHERE
  C.TABLE_OWNER = "SCHEMA NAME"
ORDER BY I.TABLE_NAME, I.INDEX_NAME, COLUMN_POSITION;
```

#### Primary Key

```sql
SELECT 
  C.OWNER, C.TABLE_NAME, D.POSITION, D.COLUMN_NAME  
FROM 
  ALL_CONSTRAINTS C JOIN ALL_CONS_COLUMNS D
ON
  C.OWNER = D.OWNER AND
  C.CONSTRAINT_NAME = D.CONSTRAINT_NAME
WHERE
  C.CONSTRAINT_TYPE = 'P' AND C.OWNER = "SCHEMA NAME"
ORDER BY C.TABLE_NAME, D.POSITION;
```






## Reference

http://blog.darkthread.net/post-2011-06-18-get-oracle-schema-info.aspx

https://stackoverflow.com/questions/22298005/how-to-find-schema-name-in-oracle-when-you-are-connected-in-sql-session-using