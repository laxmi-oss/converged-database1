
# Oracle XML  


## Steps:

1. Customer purchase history  
    
    XMLTABLE: Convert XML Data into Rows and Columns using SQL. The XMLTABLE operator, which allows you to project columns on to XML data in an XMLTYPE , making it possible to query the data directly from SQL as if it were relational data.

    ````
    <copy>
    SELECT t.object_value.getclobval()
    FROM   purchaseorder p,
    XMLTABLE('for $r in /PurchaseOrder[Reference/text()=$REFERENCE] return $r' passing object_value, 'AHUNOLD-20141130' AS  "REFERENCE") t;  



      </copy>
    ````
  
   ![](./images/xml_m7.PNG " ")
    
   

See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).