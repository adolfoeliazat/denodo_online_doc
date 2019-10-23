===============================================================
Grant Privileges in SAP BW to the Virtual DataPort User Account
===============================================================

Usually, SAP systems are configured to limit the functions a user can
invoke.

If you are going to retrieve data from SAP BW (i.e. create
multidimensional data sources), you have to grant access to the
following functions, via the authorization object ``S_RFC``, to the user
account used by Virtual DataPort to connect to SAP BW:

-  RFCPING
-  RFC\_GET\_FUNCTION\_INTERFACE
-  DDIF\_FIELDINFO\_GET
-  SYSTEM\_RESET\_RFC\_SERVER

Virtual DataPort invokes these BAPIs at introspection time (when opening
the data source to list the SAP BW cubes):

-  BAPI\_MDPROVIDER\_GET\_CUBES
-  BAPI\_MDPROVIDER\_GET\_VARIABLES
-  BAPI\_MDPROVIDER\_GET\_MEASURES
-  BAPI\_MDPROVIDER\_GET\_DIMENSIONS
-  BAPI\_MDPROVIDER\_GET\_LEVELS
-  BAPI\_MDPROVIDER\_GET\_PROPERTIES
-  BAPI\_MDPROVIDER\_GET\_HIERARCHYS
-  RSOBJS\_GET\_NODES\_X

When querying views that involve a multidimensional data source with the
“SAP BW 3.x (BAPI)” adapter, it invokes these:

-  BAPI\_MDDATASET\_CREATE\_OBJECT
-  BAPI\_MDDATASET\_GET\_AXIS\_INFO
-  BAPI\_MDDATASET\_GET\_AXIS\_DATA
-  BAPI\_MDDATASET\_GET\_CELL\_DATA
-  BAPI\_MDDATASET\_SELECT\_DATA
-  BAPI\_MDDATASET\_DELETE\_OBJECT
-  BAPI\_MDPROVIDER\_GET\_MEMBERS

When querying views that involve a multidimensional data source with the
“SAP BI 7.x (BAPI)” adapter, it invokes these:

-  RSR\_MDX\_CREATE\_OBJECT
-  RSR\_MDX\_GET\_AXIS\_INFO
-  RSR\_MDX\_GET\_AXIS\_DATA
-  RSR\_MDX\_GET\_CELL\_DATA
-  BAPI\_MDDATASET\_SELECT\_DATA
-  BAPI\_MDDATASET\_DELETE\_OBJECT
-  BAPI\_MDPROVIDER\_GET\_MEMBERS
