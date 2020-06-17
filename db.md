# DB Commands
## Logging SQL in Hibernate: 
### Alternative 1: 
```Shell
Configure the server (Hibernate to be exact) to output all SQL queries to the log 

Enable SQL statement logging by setting environment property "hibernate.show_sql=true". This can be done by passing an environment property in server launch configuration -Dhibernate.show_sql=true.
Optionally enable SQL formatting by setting environment property "hibernate.format_sql=true". This can be done by passing an environment property in server launch configuration -Dhibernate.format_sql=true.
Optionally enable SQL hint comments by setting environment property "hibernate.use_sql_comments=true". This can be done by passing an environment property in server launch configuration -Duse_sql_comments=true.
Optionally enable bind parameter logging for Hibernate by editing your logging configuration in config/config-logging/src/main/logging/devtest/log4j2.xml

<!-- Show SQL Bindings -->
<Logger name="org.hibernate.SQL" level="debug"/>
<Logger name="org.hibernate.type" level="trace"/>
The last step is necessary in order to see the parameters bound to the statements.
```
### Alternative 1: 
```Shell
You can look up and filter the recently executed SQL queries directly in the database

select sql_fulltext
from v$sql
where sql_fulltext like '%your_keyword%'
order by last_load_time DESC;
```
