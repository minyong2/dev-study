```sql
		SELECT '	@Column(name = "'||ORG_COLUMN_NAME||'") '||CHR(10)||'	private '||DATA_TYPE||' '||COLUMN_NAME||';' AS DATA
  FROM(
        SELECT REPLACE(SUBSTRING(INITCAP('z'||a.COLUMN_NAME),2),'_','') AS COLUMN_NAME
             , CASE WHEN A.DATA_TYPE LIKE '%character%' THEN 'String'
                    WHEN A.DATA_TYPE LIKE '%int%' THEN 'int'
                    WHEN A.DATA_TYPE = 'timestamp with time zone' THEN 'Timestamp'
                    ELSE 'String'
                END DATA_TYPE
             , LOWER(A.COLUMN_NAME) AS ORG_COLUMN_NAME
             , CONSTRAINT_TYPE
             , A.ORDINAL_POSITION
          FROM INFORMATION_SCHEMA.COLUMNS A
          LEFT OUTER JOIN(
                 SELECT A.TABLE_SCHEMA, A.TABLE_NAME, COLUMN_NAME, CONSTRAINT_TYPE
                   FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS A
                   LEFT OUTER JOIN INFORMATION_SCHEMA.CONSTRAINT_COLUMN_USAGE B
                     ON A.TABLE_CATALOG = B.TABLE_CATALOG
                    AND A.TABLE_SCHEMA = B.TABLE_SCHEMA
                    AND A.TABLE_NAME = B.TABLE_NAME
                    AND A.CONSTRAINT_NAME = B.CONSTRAINT_NAME
                  WHERE A.TABLE_SCHEMA = 'rca'
                    AND A.TABLE_NAME = 'tb_alarm_cur_mst_elk'
                    AND A.CONSTRAINT_TYPE = 'PRIMARY KEY'
          ) B
            ON A.COLUMN_NAME = B.COLUMN_NAME
         WHERE A.TABLE_NAME = 'tb_alarm_cur_mst_elk'
) X
 WHERE CONSTRAINT_TYPE is not NULL
 ORDER BY ORDINAL_POSITION
 ```

 ==> 복합키 조회하는 쿼리

 ```sql
  WHERE CONSTRAINT_TYPE is NULL
  로 고쳐주면 복합키 외에 키를 조회해줌
 ```
