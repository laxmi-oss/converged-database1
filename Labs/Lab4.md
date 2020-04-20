
# Oracle Spatial  

**Create spatial indexes**


1. **Create Indexes**
   
  ````
    <copy>
   CREATE INDEX customers_sidx ON customers(CUST_GEO_LOCATION) indextype is mdsys.spatial_index; 
   CREATE INDEX warehouses_sidx ON warehouses(WH_GEO_LOCATION) indextype is mdsys.spatial_index;  
   </copy>
    ````


See an issue?  Please open up a request [here](https://github.com/oracle/learning-library/issues).