
# Oracle JSON  


## Steps:

**Customers who ordered products from specific Geo location( JavaScript Object Notation)**

1. Find Row Count.   

    ````
    <copy>
    selectj.PO_DOCUMENT.Reference,
    j.PO_DOCUMENT.Requestor,
    j.PO_DOCUMENT.CostCenter,
    j.PO_DOCUMENT.ShippingInstructions.Address.city
    from PURCHASE_ORDER j 
    wherej.PO_DOCUMENT.ShippingInstructions.Address.city = 'South San Francisco'
    /

      </copy>
    ````
  
  ![](./images/select_count.PNG " ")
    
   **Note:** Oracle database allows a simple ‘dotted’ notation to be used to perform a limited set of operations on columns containing JSON.In order to use the dotted notation, a table alias must be assigned to the table in the FROM clause, and any reference to the JSON column must be prefixed with the assigned alias. All data is returned as VARCHAR2(4000).

See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).