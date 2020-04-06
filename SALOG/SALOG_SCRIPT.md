# SALOG SCRIPTS :

## Script for Service:
```GROOVY
import java.util.*
import java.lang.*
import java.util.*

import com.kn.salog.customer.entity.Customer

def service = lookup(com.kn.salog.fwcore.customs.bp.service.CustomsDefaultService.class)

List<Customer> allCustomer = fetchDataWithoutParameter()
println "allCustomer Size:"+allCustomer.size()

allCustomer.each { customer ->
if(service.findPoaAvailableForClient(customer.code)) {
    println "==========FOUND==============>"+customer.code
}else{
	println "==========NOPE=============>"+customer.code
}
}

def fetchDataWithoutParameter() {
    def sql = "FROM Customer where id between 62409400 and 69556855"
    return query(sql)
}
```