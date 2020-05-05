
# Oracle JSON  
 

## Steps:

**Find all customers who purchased an items tagged with a specific UPC (json_exists)**

1. Find Row Count   

    ````
    <copy>
    SELECT po.po_document.PONumber,po.po_document.Requestor
    FROM purchase_orderpo
    WHERE json_exists(po.po_document, '$?(@.LineItems.Part.UPCCode == 85391628927)'
    );

      </copy>
    ````
  
   ![](./images/count_po_document.PNG " ")
    
   **Note:** The JSON-EXISTS operator is used in the WHERE clause of a SQL statement. It is used to test whether or not a JSON document contains content that matches the provided JSON path expression.

   The JSON-EXISTS operator takes two arguments, a JSON column and a JSON path expression. It returns TRUE if the document contains a key that matches the JSON path expression, FALSE otherwise. JSON-EXISTS provides a set of modifiers that provide control over how to handle any errors encountered while evaluating the JSON path expression.

   [UPC, short form for  Universal Product Code, is a type of code printed on retail product packaging to aid in identifying a particular item. It consists of two parts – the machine-readable barcode, which is a series of unique black bars, and the unique 12-digit number beneath it.]


Seean issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).