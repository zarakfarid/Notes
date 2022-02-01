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

## Schema Size
```SQL

-- Schema Sizes
select '''' || OWNER || ''',', round(sum(BYTES)/1024/1024,2) as SIZE_MB
from DBA_SEGMENTS
group by OWNER
order by SIZE_MB DESC
;

```
```SQL
SELECT tablespace_name,
       size_mb,
       free_mb,
       max_size_mb,
       max_free_mb,
       TRUNC((max_free_mb/max_size_mb) * 100) AS free_pct,
       RPAD(' '|| RPAD('X',ROUND((max_size_mb-max_free_mb)/max_size_mb*10,0), 'X'),11,'-') AS used_pct
FROM   (
        SELECT a.tablespace_name,
               b.size_mb,
               a.free_mb,
               b.max_size_mb,
               a.free_mb + (b.max_size_mb - b.size_mb) AS max_free_mb
        FROM   (SELECT tablespace_name,
                       TRUNC(SUM(bytes)/1024/1024) AS free_mb
                FROM   dba_free_space
                GROUP BY tablespace_name) a,
               (SELECT tablespace_name,
                       TRUNC(SUM(bytes)/1024/1024) AS size_mb,
                       TRUNC(SUM(GREATEST(bytes,maxbytes))/1024/1024) AS max_size_mb
                FROM   dba_data_files
                GROUP BY tablespace_name) b
        WHERE  a.tablespace_name = b.tablespace_name
       )
ORDER BY tablespace_name;
```
## Schema Users
```SQL

-- Schema Users
select '''' || username || ''',' from ALL_USERS where username like '%SALOG%' order by username;

```
## Removing schemas from local XE:
```SQL
-- drop schemas
ALTER SESSION SET RECYCLEBIN=OFF;

declare
v_array sys.dbms_debug_vc2coll := sys.dbms_debug_vc2coll(
'SALOG_3',
'SALOG_317',
'SALOG_317_TEST',
'SALOG_3172_TEST',
'SALOG_316_TEST',
'SALOG_3174',
'SALOG_3172'
);
begin
for r in v_array.first..v_array.last loop
execute immediate 'drop user ' || v_array(r) || ' cascade';
end loop;
end;
/

SELECT r.obj#, u.username, r.original_name FROM recyclebin$ r, dba_users u WHERE r.owner# = u.user_id;
purge recyclebin;
purge dba_recyclebin;

```
## Altering Database file from local XE:
```SQL
ALTER DATABASE DATAFILE 'C:\oraclexe\app\oracle\oradata\XE\USERS.DBF' RESIZE 200M;
```

## Altering Database tablespace file local XE:
```SQL
create undo tablespace UNDO_RBS1 datafile 'C:\ORACLEXE\APP\ORACLE\ORADATA\XE\undorbs1.db' size 1000m;
alter system set undo_tablespace=undo_rbs1;

--get the filename of the old undo tablespace which will be dropped so you can remove the file
SELECT file_name FROM dba_data_files WHERE tablespace_name = 'UNDOTBS1';

drop tablespace UNDOTBS1;
```

## CUSRSUR:
```SQL

-- Create TEMP Table
CREATE TABLE temp_bookingstatus (
    id             NUMBER(28) NOT NULL,
    status         VARCHAR2(15 CHAR),
    is_processed   NUMBER(1)
);

CREATE UNIQUE INDEX temp_bookingstatus_i ON
    temp_bookingstatus (
        id ASC,
        status ASC,
        is_processed ASC );

-- Insert data into TEMP Table
MERGE INTO temp_bookingstatus temp USING ( 
	SELECT bs.id, b.operational_status
	FROM booking b
    JOIN booking_summary bs ON b.id = bs.booking_id
    WHERE b.operational_status IN ('CLO', 'CAN')
    AND   b.operational_status <> bs.operational_status)
source ON ( temp.id = source.id )
WHEN NOT MATCHED THEN INSERT (id, status, is_processed) VALUES (source.id, source.operational_status, 0);

-- Bulk Update TEMP Table and Table to be Updated
DECLARE
  cursor_limit PLS_INTEGER := 1000;
  cursor data_cursor is select ID,STATUS,IS_PROCESSED from temp_bookingstatus where IS_PROCESSED=0;
  type t_data_batch is table of temp_bookingstatus%ROWTYPE;
  bulk_rows t_data_batch;
BEGIN
   open data_cursor;

   loop   
      fetch data_cursor
        bulk collect into bulk_rows
        limit cursor_limit;
      exit when bulk_rows.count = 0;

      forall idx IN 1..bulk_rows.count
       update Booking_summary set Operational_status=bulk_rows(idx).status where id = bulk_rows(idx).ID;
      forall idx IN 1..bulk_rows.count
       update temp_bookingstatus set IS_PROCESSED = 1 where id = bulk_rows(idx).ID;
      commit;
   end loop;
   close data_cursor;
END;
```

