========================================
Grant Privileges on SAP for BAPI Sources
========================================

If you are going to retrieve data from SAP (i.e. create BAPI data
sources), you have to grant access to the following functions, via the
authorization object ``S_RFC``, to the user account used by Virtual
DataPort to connect to SAP:

-   RFCPING
-   RFC_GET_FUNCTION_INTERFACE
-   DDIF_FIELDINFO_GET
-   SYSTEM_RESET_RFC_SERVER (executed after running the BAPI of a
    base view)

In addition, grant access to the BAPI invoked by each BAPI base view you
create.
