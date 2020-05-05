
# Oracle JSON  


## Steps:

**DML OPERATIONS**

1. Find  Count   

    ````
    <copy>
    select count(*) from purchase_order;

      </copy>
    ````
     ![](./images/json_lab7_1.PNG " ")


    
2. Find Count

    ````
    <copy>
    select * from purchase_order j where j.po_document.PONumber=10000;

      </copy>
    ````
     ![](./images/json_lab7_2.PNG " ")


3. Find Count
   
    ````
    <copy>
   select * from purchase_order j where j.po_document.PONumber=10001; 
    </copy>
    ````
     ![](./images/json_lab7_3.PNG " ")

4. Insert Records.
    
    ````
    <copy>
    
     INSERT INTO purchase_order
     VALUES (
     SYS_GUID(),
     to_date('05-MAY-2020'),
     '{"PONumber"             : 10001,
      "Reference"            : "SBELL-20141017",
      "Requestor"            : "Sarah Bell",
      "User"                 : "SBELL",
      "CostCenter"           : "A50",
      "ShippingInstructions" : {"name"    : "Sarah Bell",
      "Address" : {"street"  : "200 Sporting Green",
      "city"    : "South San Francisco",
      "state"   : "CA",
      "zipCode" : 99236,
      "country" : "United States of America"},
     "Phone"   : "983-555-6509"},
      "Special Instructions" : "Courier",
      "LineItems"            : [{"ItemNumber" : 1,
      "Part"       : {"Description" : "Making the Grade",
      "UnitPrice"   : 20,
      "UPCCode"     : 27616867759},
      "Quantity"   : 8.0},
      {"ItemNumber" : 2,
      "Part"       : {"Description" : "Nixon",
      "UnitPrice"   : 19.95,
      "UPCCode"     : 717951002396},
      "Quantity"   : 5},
      {"ItemNumber" : 3,
      "Part"       : {"Description" : "Eric Clapton: Best Of 1981-1999",
      "UnitPrice"   : 19.95,
      "UPCCode"     : 75993851120},
      "Quantity"   : 5.0}
      ]}');
       
       </copy>
    ````
    ![](./images/json_lab7_4.PNG " ")
    ![](./images/json_lab7_5.PNG " ")

5. Update Records.

   ````
    <copy>
   update purchase_order set  
   PO_DOCUMENT = json_mergepatch (PO_DOCUMENT,
   '{
        "Requestor" : "MSDhoni"
    }'
    )  where id ='A4E055B4CF4A23A4E0530900000A60C2';


    </copy>
    ````
     ![](./images/json_lab7_6.PNG " ")  

6. Find  Count   

    ````
    <copy>
    select * from purchase_order j where j.po_document.PONumber=10001;

      </copy>
    ````
     ![](./images/json_lab7_7.PNG " ")

7. Find  Count   

    ````
    <copy>
    select count(*) from purchase_order;

      </copy>
    ````
     ![](./images/json_lab7_8.PNG " ")

8. Find  Count   

    ````
    <copy>
    select * from purchase_order where id ='A4E055B4CF4A23A4E0530900000A60C2';

      </copy>
    ````
     ![](./images/json_lab7_9.PNG " ")


9. Delete Records.    

    ````
    <copy>
    delete from purchase_order where id ='A4E055B4CF4A23A4E0530900000A60C2';

      </copy>
    ````
     ![](./images/json_lab7_10.PNG " ")


10. Find Count.    

    ````
    <copy>
    select count(*) from purchase_order;

      </copy>
    ````
     ![](./images/json_lab7_11.PNG " ")
     ![](./images/json_lab7_12.PNG " ")



See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).