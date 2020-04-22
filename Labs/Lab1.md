
# Oracle Spatial  

**Scenario**

My Company has several major warehouses. It needs to locate its customers who are near a given warehouse, to inform them of new advertising promotions. To locate its customers and perform location-based analysis, My Company must store location data for both its customers and warehouses.
This tutorial uses CUSTOMERS and WAREHOUSES tables. Each table stores location using Oracle's native spatial data type, SDO_GEOMETRY. A location can be stored as a point in an SDO_GEOMETRY column of a table. The customer's location is associated with longitude and latitude values on the Earth's surface—for example, -63.13631, 52.485426.


**Connecting to Schema**

This section looks at how to connect to schema.

The tasks you will accomplish in this lab are:
- Create a schema **oeuser** in the container database **SPAGRAPDB**  

1. Connect to **SPAGRAPDB**  

    ````
    <copy>
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

3. **Connect to Schema**: This section looks at how to connect to schema. 
   
   - Create a schema **oeuser** in the container database **SPAGRAPDB**
  
    ````
    <copy>
    create user oeuser identified by oeuser container=current;
    </copy>
    ````
    
1. Grant dba privilage to **oeuser**.  

    ````
    <copy>
    grant dba to oeuser;
    </copy>
    ````
   
2. Connect Container **SPAGRAPDB** as user **oeuser**

    ````
    <copy>
    sqlplus oeuser/oeuser@SPAGRAPDB
    </copy>
    ````
   
3. Create TYPE **cust_address_typ**  and **phone_list_typ**

    ````
    <copy>
    CREATE TYPE cust_address_typ
    AS OBJECT
    ( street_address     VARCHAR2(40)
    , postal_code        VARCHAR2(10)
    , city               VARCHAR2(30)
    , state_province     VARCHAR2(10)
    , country_id         CHAR(2)
    );
    / 

   CREATE TYPE phone_list_typ
    AS VARRAY(5) OF VARCHAR2(25);
    /
   </copy>
   
   ````

4. Create a table **customers**   

    ````
    <copy>
    CREATE TABLE customers
    ( customer_id        NUMBER(6),
    cust_first_name    VARCHAR2(20) CONSTRAINT cust_fname_nn NOT NULL , 
    cust_last_name     VARCHAR2(20) CONSTRAINT cust_lname_nn NOT NULL,
    cust_address cust_address_typ, 
    phone_numbers      phone_list_typ, 
    nls_language       VARCHAR2(3) , 
    nls_territory      VARCHAR2(30), 
    credit_limit       NUMBER(9,2), 
    cust_email         VARCHAR2(40),
    account_mgr_id     NUMBER(6) ,
    cust_geo_location  MDSYS.SDO_GEOMETRY , 
    CONSTRAINT         customer_credit_limit_max  CHECK (credit_limit <= 5000) ,
    CONSTRAINT         customer_id_min  CHECK (customer_id > 0)
    ) ;
    </copy>

    ````

   

5. Script to load customer data in the schema

    ````
    <copy>
    
    @oe_p_cus.sql

    </copy>  


See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).
