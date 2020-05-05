
# Oracle Spatial  



**Load data**

## Steps: ##

  First we load CUSTOMERS by copying from the table oeuser.CUSTOMERS

   **Note that we are using two spatial functions in this -**
   -  we use sdo_cs.transform() to convert to our desired coordinate system SRID of 4326.
   -  we use sdo-geom.validate-geometry() to insert only valid geometries. 




 1. Check to see who you are connected as. At any point in the lab you can run this script to see who or where you are connected.  

    ````
    <copy>
    select
      'DB Name: '  ||Sys_Context('Userenv', 'DB_Name')||
      ' / CDB?: '     ||case
        when Sys_Context('Userenv', 'CDB_Name') is not null then 'YES'
          else  'NO'
          end||
      ' / Auth-ID: '   ||Sys_Context('Userenv', 'Authenticated_Identity')||
      ' / Sessn-User: '||Sys_Context('Userenv', 'Session_User')||
      ' / Container: ' ||Nvl(Sys_Context('Userenv', 'Con_Name'), 'n/a')
      "Who am I?"
      from Dual
      /
      </copy>
    ````

    
 2. Insert Data into **CUSTOMERS** Table

    ````
    <copy>
    INSERT INTO CUSTOMERS
    SELECT CUSTOMER_ID, CUST_FIRST_NAME, CUST_LAST_NAME , sdo_cs.transform(CUST_GEO_LOCATION,4326), ACCOUNT_MGR_ID
    FROM oeuser.customers WHERE sdo_geom.validate_geometry(CUST_GEO_LOCATION,0.05)='TRUE';
    
    commit;
    </copy>
    ````
    
 3. Manually load **warehouses** using the **SDO-GEOMETRY** constructor.

    ````
    <copy>
    INSERT INTO WAREHOUSES values (1,'Southlake, TX',1400,SDO_GEOMETRY(2001, 4326, MDSYS.SDO_POINT_TYPE(-103.00195, 36.500374, NULL), NULL, NULL));

    INSERT INTO WAREHOUSES values (2,'San Francisco, CA',1500,SDO_GEOMETRY(2001, 4326, MDSYS.SDO_POINT_TYPE(-124.21014, 41.998016, NULL), NULL, NULL));

    INSERT INTO WAREHOUSES values (3,'Sussex, NJ',1600,SDO_GEOMETRY(2001, 4326, MDSYS.SDO_POINT_TYPE(-74.695305, 41.35733, NULL), NULL, NULL));

    INSERT INTO WAREHOUSES values (4,'Seattle, WA',1700, SDO_GEOMETRY(2001, 4326, MDSYS.SDO_POINT_TYPE(-123.61526, 46.257458, NULL), NULL, NULL));

    COMMIT;
    </copy>
    ````
   



   **The elements of the constructor are:**

   -	2001: SDO-GTYPE attribute and it is set to 2001 when storing a two-dimensional single point such as a customer's location.
   -	4326: This is the spatial reference system ID (SRID): a foreign key to an Oracle dictionary table  (MDSYS.CS-SRS) that contains all the supported coordinate systems. It is important to associate your customer's location to a coordinate system. In this example, 4326 corresponds to "Longitude / Latitude (WGS 84)."
   -	MDSYS.SDO-POINT-TYPE: This is where you store your longitude and latitude values within the SDO_GEOMETRY constructor. 
     Note that you can store a third value also, but for these tutorials, all the customer data is two-dimensional.
   -	NULL, NULL: The last two null values are for storing linestrings, polygons, and geometry collections. 
     For more information on all the fields of the SDO_GEOMETRY object, please refer to the Oracle Spatial Developer's Guide. For this tutorial with point data,  these last two fields should be set to NULL.


See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).