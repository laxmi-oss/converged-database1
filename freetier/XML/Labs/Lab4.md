
# Oracle XML  


## Steps:

1. Listing the product description those unit price matches to ‘$xx’
    
    XMLSERIALIZE is a SQL/XML operator that you can use to convert an XML type to a character type.

    ````
    <copy>
    SELECT XMLSERIALIZE(CONTENT COLUMN_VALUE AS CLOB INDENT SIZE=2) 
    FROM   Purchaseorder p, XMLTable('<Summary> { for $r in 
    /PurchaseOrder/LineItems/Part   return $r/Description }    </Summary>' passingobject_value   )
    WHERE  xmlexists('/PurchaseOrder/LineItems/Part[UnitPrice/text()=$UnitPrice]' passing object_value, '27.95' AS "UnitPrice" ); 
    /

      </copy>
    ````
  
   ![](./images/xml_m8.PNG " ")
    
   

See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).