# SALOG Properties :

## salog.startup.properties.xml
```Properties
bpa.cache.prefill.enabled=false
bpa.bo.processCompleted.enabled=false
bpa.businessDataCleanUpPrepare.enabled=false
bpa.businessDataCleanUp.enabled=false
bpa.engine.auth.enabled=false
bpa.housekeeping.processInstancesAndJobsCleanUp.enabled=false
bpa.jdbcBatchProcessing.enabled=false
bpa.taskAssignmentCleanUp.enabled=false
persistentClientCache.installServerCache=false
```

## DEV_filter.properties.xml
```Properties
ws.esb.useBasicAuth=true
persistentClientCache.installServerCache=false
```

## Context.xml
```Properties
 <Resource name="jdbc/salogDS" auth="Container" factory="oracle.ucp.jdbc.PoolDataSourceImpl" type="oracle.ucp.jdbc.PoolDataSource" description="Tomcat Access SALOG" connectionFactoryClassName="oracle.jdbc.pool.OracleDataSource" minPoolSize="1" maxPoolSize="30" inactiveConnectionTimeout="20" abandonedConnectionTimeout="660" timeToLiveConnectionTimeout="660" user="SALOG_44" password="SALOG_44" url="jdbc:oracle:thin:@localhost:1521:XE" connectionPoolName="UCPPool" validateConnectionOnBorrow="true" sqlForValidateConnection="select 1 from DUAL"/>

    <!-- Datasource for Camunda / BPA -->
    <Resource name="jdbc/processApplication" auth="Container" factory="oracle.ucp.jdbc.PoolDataSourceImpl" type="oracle.ucp.jdbc.PoolDataSource" description="Tomcat Access BPA" connectionFactoryClassName="oracle.jdbc.pool.OracleDataSource" minPoolSize="1" maxPoolSize="30" inactiveConnectionTimeout="20" abandonedConnectionTimeout="660" timeToLiveConnectionTimeout="660" user="SALOG_44" password="SALOG_44" url="jdbc:oracle:thin:@localhost:1521:XE" connectionPoolName="UCPPool" validateConnectionOnBorrow="true" sqlForValidateConnection="select 1 from DUAL"/>
   
```