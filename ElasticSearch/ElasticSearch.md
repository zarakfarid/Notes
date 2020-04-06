# Elastic Search and Mongo :

## Elastic: 
```Shell
ana@ip-10-171-67-189:~/zarak$ cat removeRecord.sh

curl -XDELETE 'http://localhost:9200/workindexes/works/_query' -d '{"term":{"works.indexId":"8001603176"}}’

curl -XDELETE 'http://localhost:9200/mfcl-reverse-lookup/lookups/_query' -d '{"term":{“lookups.work_id":"8000000152"}}’
./removeRecord.sh 
```
## From Mongo : 
```Shell
> db.work.remove({"_id":"8000000276”})

> db.displayRecord.remove({"workRefId":"8000000276"})

> db.relatedWorkEntity.remove({"workRefId":"8000000276”})

> db.IngestionEntity.count({$and:[{"insertedTime":{$gte:ISODate("2016-10-01T00:00:00.000Z")}},{"instituteId":10003}]})

> db.work.update({"_id":"8000000002"},{ $push: { "titles.title": { $each: [{_id:125,score:90},{_id:127,score:91}] } } },false,true)


./mongoexport --db ssw17 --collection lookupEntity --csv --fields _id,instituteid,institute name,visiblity,term,legacyrelatedtypeid --out ~/Documents/Work/db.csv 

./mongoexport --db ssw17 --collection relatedWorkEntity --csv --fieldFile fields.txt --out /home/ana/mongoDB/mongodb-linux-x86_64-rhel57-2.2.1/RelatedWorkEntity.csv

./mongoexport --db ssw17stage --collection work  --csv --fields _id --out ~/zarak/db.csv

./mongoexport --db ssw17stage --collection work  --csv --fields _id,contributor --out ~/zarak/db.csv


```
