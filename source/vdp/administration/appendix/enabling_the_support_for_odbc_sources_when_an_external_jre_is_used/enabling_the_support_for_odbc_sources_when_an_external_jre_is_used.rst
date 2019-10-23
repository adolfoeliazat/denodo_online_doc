====================================================================================
Enabling the Support for ODBC Sources When an External JRE is Used
====================================================================================

Virtual DataPort provides access to ODBC data sources. If you configure the Virtual DataPort server to use an external Java Runtime Environment (JRE) instead of the one included in the Denodo Platform, do the following to be able to connect to ODBC sources:

a. If the Denodo server runs on Windows, copy the file :file:`{<DENODO_HOME>}\\dll\\vdp\\jdbc-odbc\\x64\\JdbcOdbc.dll` to the folder :file:`{<EXTERNAL_JRE_HOME>}\\bin`.
b. If the Denodo server runs on Linux, copy the file :file:`{<DENODO_HOME>}/dll/vdp/jdbc-odbc/x64/libJdbcOdbc.so` to the folder :file:`{<EXTERNAL_JRE_HOME>}/lib/amd64`.

The installation also provides these libraries for the 32-bit architecture, in the folder :file:`{<DENODO_HOME>}/dll/vdp/jdbc-odbc/x32/`. In this case, on Linux, copy the file in that directory to the folder :file:`{<EXTERNAL_JRE_HOME>}/lib/i386`; on Windows copy it to :file:`{<EXTERNAL_JRE_HOME>}\\bin`.