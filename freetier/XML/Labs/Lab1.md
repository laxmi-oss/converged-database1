
# Oracle JSON 

 **Querying XML Content**

## Steps:#


 


1. Getting the number of XML documents. There are many ways, following is one of them
   
       ````
    <copy>
    SELECT Count(*)FROM   purchaseorder p,  XMLTABLE('for $r in /PurchaseOrder return $r' passing object_value) t;
    )
    /
    </copy>
    ````
   
     ![](./images/xml_m1.PNG " ")

2. Retrieving the content of an XML document-usingpseudocolumn OBJECT_VALUE

    
    ````
    <copy>
    SELECT t.object_value.getclobval()FROM   purchaseorder t
    WHERE  rownum = 1;  

 
       </copy>
    ````

     ![](./images/xml_m2.PNG " ")

3.  Accessing fragments or nodes of an XML document
 
    
    ````
    <copy>
    SELECT Xmlquery('/PurchaseOrder/Reference' passing object_value returning
    content)
    FROM   purchaseorder
    WHERE  ROWNUM<= 5
    /
    
    </copy>
    ````

  ![](./images/xml_m3.PNG " ")

4.  Accessing text node value

    ````
    <copy>
    SELECT xmlcast(xmlquery('$p/PurchaseOrder/Reference/text()' passing object_value AS "p" returning content) AS varchar2(30))
    FROM   purchaseorder
    WHER ROWNUM<= 5
    /

    </copy>
    ````

   ![](./images/xml_m4.PNG " ")

5. Searching an xml document
   
       ````
    <copy>
    SELECT t.object_value.getclobval()
    FROM   purchaseorder t
    WHERE  xmlexists('/PurchaseOrder[Reference/text()=$REFERENCE]' passing object_value, 'AsniHUNOLD-20141130' AS "REFERENCE" );
    )
    /
    </copy>
    ````
   
     ![](./images/xml_m5.PNG " ")

    




See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).
