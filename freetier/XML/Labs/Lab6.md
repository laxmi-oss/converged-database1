
# Oracle XML  


## Steps:

1. Custer Delivery Priority Instruction  
    
    Ex - Courier, Expidite,Surface Mail,Air Mail etc..

    ExistsNode() checks if xpath-expression returns at least one XML element or text node. If so, existsNode returns 1, otherwise, it returns 0. existsNode should only be used in the where clause of the select statement.


    ````
    <copy>
    SELECT extractValue(OBJECT_VALUE, '/PurchaseOrder/Reference') "REFERENCE"
    FROM purchaseorder WHERE existsNode(OBJECT_VALUE, '/PurchaseOrder[Special_Instructions="Next Day Air"]')=1;



      </copy>
    ````
  
    
    
    ![](./images/xml_m10_a.PNG " ")

     ````
    <copy>
   SELECT extractValue(OBJECT_VALUE, '/PurchaseOrder/Reference') "REFERENCE"
   FROM purchaseorder
   WHERE existsNode(OBJECT_VALUE, '/PurchaseOrder[Special_Instructions="Priority Overnight"]')=1;
 
    </copy>
    ````
    ![](./images/xml_m10_b.PNG " ")
    ![](./images/xml_m10_c.PNG " ")
   

See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).