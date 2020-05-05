
# Oracle Spatial  

**Create tables and spatial metadata:**
 

## Steps ##

1. Login to the PDB:
   
   ````
    <copy>
   ps -ef|grep|pmon
   . oraenv
   sqlplus ' / as sysdba'
   alter session set container=SPAGRAPDB;
    </copy>
    ````

2. Check to see who you are connected as. At any point in the lab you can run this script to see who or where you are connected.  
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

    
3. Create a  user **app_test**

    ````
    <copy>
    create user app_test identified by app container=current;
    </copy>
    ````
    
4. Grant dba privilage to **app_test** user.

    ````
    <copy>
    grant dba to app_test;
    </copy>
    ````
   
5. Connect Container **SPAGRAPDB** as user **app_test**

    ````
    <copy>
    sqlplus app_test/app@SPAGRAPDB
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

  
7.    Next we add Spatial metadata for the CUSTOMERS and WAREHOUSES tables
      to the **USER-SDO-GEOM-METADATA** view. Each **SDO-GEOMETRY** column is registered
      with a row in   **USER-SDO-GEOM-METADATA**.

    ````
    <copy>
     EXECUTE SDO_UTIL.INSERT_SDO_GEOM_METADATA (sys_context('userenv','current_user'), -
    'CUSTOMERS', 'CUST_GEO_LOCATION', -  SDO_DIM_ARRAY(SDO_DIM_ELEMENT('X',-180, 180, 0.05), - SDO_DIM_ELEMENT('Y', -90, 90, 0.05)),-  4326);

     EXECUTE SDO_UTIL.INSERT_SDO_GEOM_METADATA (sys_context('userenv','current_user'), -
     'WAREHOUSES', 'WH_GEO_LOCATION', - SDO_DIM_ARRAY(SDO_DIM_ELEMENT('X',-180, 180, 0.05), - SDO_DIM_ELEMENT('Y', -90, 90, 0.05)),-  4326);
      </copy>
    
       ````

     
     **Here is a description of the items that were entered:**

     -	TABLE-NAME: Name of the table which contains the spatial data.
     -	COLUMN-NAME: Name of the SDO-GEOMETRY column which stores the spatial data.
     -	 MDSYS.SDO-DIM-ARRAY: Constructor which holds the MDSYS.SDO-DIM-ELEMENT object,which in turn stores the extents of the spatial data  in each dimension (-180.0, 180.0), and a tolerance value (0.05). The tolerance is a round-off error value used by Oracle Spatial, and is in meters for longitude and latitude data. In this example, the tolerance is 5 mm.
     -	4326: Spatial reference system id (SRID): a foreign key to an Oracle dictionary table  (MDSYS.CS-SRS) tha  contains all the     supported coordinate systems. It is important to associate your customer's location to a coordinate system. In this example, 4326    corresponds to "Longitude / Latitude (WGS 84).".
 

See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).