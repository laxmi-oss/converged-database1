
# Oracle XML  


## Steps:

1.	Get the list of the customer and their purchased information from a geo graphical location 
    
    XMLEXISTS is an SQL/XML operator that you can use to query XML values in SQL, in a regular query I can use the xmlexists function to look if a specific value is present in an xmltype column
    ````
    <copy>
    SELECT t.object_value.getclobval() FROM   purchaseorder t
    WHERE  xmlexists('/PurchaseOrder/ShippingInstructions/Address[city/text()=$CITY]' passing object_value, 'South San Francisco' AS "CITY" );


      </copy>
    ````
  
   ![](./images/xml_m6.PNG " ")
    
   

See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).