# SQL Commands

### INDEX & CONSTRAINT
```SHELL
select INDEX_NAME, TABLE_OWNER, TABLE_NAME, UNIQUENESS from USER_INDEXES
select constraint_name, constraint_type from user_constraints
```
### UPDATE ONE TABLE FROM ANOTHER
```SQL 
UPDATE table1 t1
   SET (name, desc) = (SELECT t2.name, t2.desc
                         FROM table2 t2
                        WHERE t1.id = t2.id)
 WHERE EXISTS (
    SELECT 1
      FROM table2 t2
     WHERE t1.id = t2.id )
select constraint_name, constraint_type from user_constraints
```
```SQL 
MERGE INTO table1 t1
USING
(
-- For more complicated queries you can use WITH clause here
SELECT * FROM table2
)t2
ON(t1.id = t2.id)
WHEN MATCHED THEN UPDATE SET
t1.name = t2.name,
t1.desc = t2.desc;
```
### Current Session
```SQL
select * FROM V$session where sid = (select sid from v$mystat
where rownum = 1);
```
### Parallel
```SQL
ALTER SESSION FORCE PARALLEL DDL PARALLEL 16;
ALTER SESSION FORCE PARALLEL QUERY PARALLEL 16;
```
```SQL
DECLARE
BEGIN
  for IDX in (SELECT INDEX_NAME FROM USER_INDEXES WHERE TRIM(DEGREE) NOT IN ('0', '1') OR TRIM(INSTANCES) NOT IN ('0', '1') ) LOOP
    EXECUTE IMMEDIATE 'ALTER INDEX ' || IDX.INDEX_NAME || ' noparallel';
    END LOOP;
END;
/

DECLARE
BEGIN
  for TBL in (SELECT TABLE_NAME FROM USER_TABLES WHERE TRIM(DEGREE) NOT IN ('0', '1') OR TRIM(INSTANCES) NOT IN ('0', '1') ) LOOP
    EXECUTE IMMEDIATE 'ALTER TABLE ' || TBL.TABLE_NAME || ' noparallel';
    END LOOP;
END;
/
```
```SQL
ALTER SESSION DISABLE PARALLEL DDL;
ALTER SESSION DISABLE PARALLEL QUERY;
```
