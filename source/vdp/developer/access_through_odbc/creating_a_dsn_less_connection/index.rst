.. todo: for 8.0, rename the file to match the title of the section

==============================
Creating a DSN-Less Connection
==============================

Some ODBC tools support connecting to an ODBC server without using a data source name (DSN). This is known as a DSN-less connection.

This section lists the parameters you have to use to create a DSN-less connection to Virtual DataPort.

.. code-block:: none
   :caption: DSN-less connection from a 64-bit client. No SSL
   :name: DSN-less connection from a 64-bit client. No SSL
   
   DRIVER={DenodoODBC Unicode(x64)};UID=<user account>;PWD=<password of the user>;SERVER=<host name>;DATABASE=<database name>;PORT=9996;SSLmode=disable;service=;krbsrvname=HTTP;UserAgent=<user agent>;ReadOnly=0;Protocol=7.4-1;FakeOidIndex=0;ShowOidColumn=0;RowVersioning=0;ShowSystemTables=0;ConnSettings=set+i18n+to+us%5fpst%3b;Fetch=100;Socket=4096;UnknownSizes=0;MaxVarcharSize=255;MaxLongVarcharSize=8190;Debug=0;CommLog=0;Optimizer=0;Ksqo=0;UseDeclareFetch=1;TextAsLongVarchar=1;UnknownsAsLongVarchar=0;BoolsAsChar=0;Parse=0;CancelAsFreeStmt=0;ExtraSysTablePrefixes=dd_;LFConversion=1;UpdatableCursors=0;DisallowPremature=0;TrueIsMinus1=0;BI=0;ByteaAsLongVarBinary=0;UseServerSidePrepare=0;LowerCaseIdentifier=0;PreferLibpq=1;GssAuthUseGSS=0;XaOpt=3

.. code-block:: none
   :caption: DSN-less connection from a 32-bit client. No SSL
   :name: DSN-less connection from a 32-bit client. No SSL

   DRIVER={DenodoODBC Unicode};UID=<user account>;PWD=<password of the user>;SERVER=<host name>;DATABASE=<database name>;PORT=9996;PWD=;SSLmode=disable;service=;krbsrvname=HTTP;UserAgent=<user agent>;ReadOnly=0;Protocol=7.4-1;FakeOidIndex=0;ShowOidColumn=0;RowVersioning=0;ShowSystemTables=0;ConnSettings=set+i18n+to+us%5fpst%3b;Fetch=100;Socket=4096;UnknownSizes=0;MaxVarcharSize=255;MaxLongVarcharSize=8190;Debug=0;CommLog=0;Optimizer=0;Ksqo=0;UseDeclareFetch=1;TextAsLongVarchar=1;UnknownsAsLongVarchar=0;BoolsAsChar=0;Parse=0;CancelAsFreeStmt=0;ExtraSysTablePrefixes=dd_;LFConversion=1;UpdatableCursors=0;DisallowPremature=0;TrueIsMinus1=0;BI=0;ByteaAsLongVarBinary=0;UseServerSidePrepare=0;LowerCaseIdentifier=0;PreferLibpq=1;GssAuthUseGSS=0;XaOpt=3

About the examples above:

-  To use the examples above, replace ``<user account>``, ``<password of the user>``, ``<host name>`` and ``<database name>`` and ``<user agent>``. If you changed the default ODBC port, replace 9996 with the actual ODBC port of Virtual DataPort.
-  Note that the value of the property ``DRIVER`` has to be ``{DenodoODBC Unicode}`` or ``{DenodoODBC Unicode(x64)}`` depending on if you are creating the connection from a 32-bit client or a 64-bit one, respectively.
-  To enable SSL, replace ``SSLmode=disable`` with ``SSLmode=require``. If you do this, you have to also enable SSL in the Virtual DataPort server. If you do not, the connection will fail.
-  To use Kerberos authentication:

   1. Remove the parameters ``UID`` and ``PWD``.
   2. The value of the property ``SERVER`` has to be the fully qualified domain name of the Denodo server. That is, 
      if in the Kerberos configuration of the Denodo server the field *Server principal* is 
      ``HTTP/denodo-prod.subnet1.contoso.com@CONTOSO.COM``, enter ``denodo-prod.subnet1.contoso.com``.
   #. *In the settings of the database you are connecting to*, you have to enable Kerberos authentication.

.. note:: Do not leave any space between the names of parameters, the equal symbol, the values, the semicolon, etc.
