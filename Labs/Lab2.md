
# Oracle Spatial  

## Create tables and spatial metadata: 

 (CUSTOMER Table has 179 rows and WAREHOUSE table has 4 rows)


 We will now create tables and spatial metadata for CUSTOMERS and WAREHOUSES. 
 We first create the CUSTOMERS and WAREHOUSES tables. Notice that each has a column of type SDO_GEOMETRY to store location. 

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

    
2. Create a  user **app**

    ````
    <copy>
    create user app identified by app container=current;
    </copy>
    ````
    
3. Grant dba privilage to **app** user.

    ````
    <copy>
    grant dba to app;
    </copy>
    ````
   
4. Connect Container **SPAGRAPDB** as user **oeuser**

    ````
    <copy>
    sqlplus app/app@SPAGRAPDB
    </copy>
    ````
   
5. Check the user.

    ````
    <copy>
    show user;
     </copy>
   
   ````

6. Create  table **CUSTOMERS**  and **WAREHOUSES** 

    ````
    <copy>
    CREATE TABLE CUSTOMERS
    ( 
    CUSTOMER_ID NUMBER(6, 0),
    CUST_FIRST_NAME VARCHAR2(20 CHAR),
    CUST_LAST_NAME VARCHAR2(20 CHAR), 
    GENDER VARCHAR2(1 CHAR), 
    CUST_GEO_LOCATION SDO_GEOMETRY,
    ACCOUNT_MGR_ID NUMBER(6, 0)
    );
  
    CREATE TABLE WAREHOUSES
    (
    WAREHOUSE_ID    NUMBER(3,0), 
    WAREHOUSE_NAME        VARCHAR2(35 CHAR), 
    LOCATION_ID   NUMBER(4,0), 
    WH_GEO_LOCATION       SDO_GEOMETRY
    );
      </copy>

    ````

  
7. We add Spatial metadata for the CUSTOMERS and WAREHOUSES tables to the USER_SDO_GEOM_METADATA view.   
    Each SDO_GEOMETRY column is registered with a row in USER_SDO_GEOM_METADATA

    ````
    <copy>
    EXECUTE SDO_UTIL.INSERT_SDO_GEOM_METADATA (sys_context('userenv','current_user'), -'CUSTOMERS', 
    'CUST_GEO_LOCATION', -  SDO_DIM_ARRAY(SDO_DIM_ELEMENT('X',-180, 180, 0.05), - SDO_DIM_ELEMENT('Y', -90, 90, 0.05)),-  4326);
  
    EXECUTE SDO_UTIL.INSERT_SDO_GEOM_METADATA (sys_context('userenv','current_user'), - 'WAREHOUSES', 
    'WH_GEO_LOCATION', -  SDO_DIM_ARRAY(SDO_DIM_ELEMENT('X',-180, 180, 0.05), -SDO_DIM_ELEMENT('Y', -90, 90, 0.05)),- 4326);
    </copy>  

    ....

Here is a description of the items that were entered: 
   -	TABLE_NAME: Name of the table which contains the spatial data.
   -	COLUMN_NAME: Name of the SDO_GEOMETRY column which stores the spatial data
   -	MDSYS.SDO_DIM_ARRAY: Constructor which holds the MDSYS.SDO_DIM_ELEMENT object, which in turn stores the extents of 
        the spatial data  in each dimension (-180.0, 180.0), and a tolerance value (0.05). The tolerance is a round-off error value used by Oracle Spatial, and is in meters for longitude and latitude data. In this example, the tolerance is 5 mm.
   -	4326: Spatial reference system id (SRID): a foreign key to an Oracle dictionary table (MDSYS.CS_SRS) tha  contains all the     supported coordinate systems. It is important to associate your customer's location to a coordinate system. In this example, 4326    corresponds to "Longitude / Latitude (WGS 84)."

8. Drop column from table CUSTOMERS
   
   ````
    <copy>
    
     alter table CUSTOMERS drop column GENDER;
     
     </copy>  

    ....


See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).