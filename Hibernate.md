# Showing SQL Bindings

```Java
System.out.println("============================================================== QUERIES - AG ===========================================================================");
String query = ((org.hibernate.internal.QueryImpl) ((org.hibernate.jpa.criteria.compile.CriteriaQueryTypeQueryAdapter) typedQuery).jpqlQuery.query).queryString.toString();
String queryWithParameters = query;
System.out.println("ORIGINAL QUERY\t->\t" + query);
System.out.println("PARAMETERS:");
Iterator<Entry<String, org.hibernate.engine.spi.TypedValue>> iterator =((org.hibernate.internal.QueryImpl) ((org.hibernate.jpa.criteria.compile.CriteriaQueryTypeQueryAdapter) typedQuery).jpqlQuery.query).namedParameters.entrySet().iterator();
while(iterator.hasNext()){
    Entry<String, org.hibernate.engine.spi.TypedValue> next = iterator.next();
    System.out.println(next.getKey() + "\t:\t"+next.getValue());
    queryWithParameters = queryWithParameters.replace(next.getKey(), "\""+next.getValue()+"\"");
}
queryWithParameters = queryWithParameters.replace(":=", "\"");
System.out.println("QUERY WITH PARAMETERS:\t->\t" + queryWithParameters);
System.out.println("=============================================================== END OF QUERIES =============================================================================");
return false;
```
