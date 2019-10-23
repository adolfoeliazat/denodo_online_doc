==================================
Defining Data Sources and Wrappers
==================================

A *wrapper* must be created and assigned to a base relation in order to
be able to obtain data from a specific source. Each *wrapper* should
provide access to the data forming a base relation in a source, so that
they are structured in a manner similar to a Relational Database table
with regard to the Virtual DataPort server. More specifically, each *wrapper*
should provide an overall view of the source according to the *base
relation* model provided in the previous section.

The process of generating *wrappers* for source types such as JDBC/ODBC
Databases, SOAP and REST Web Services, LDAP servers, XML documents,
delimited text files or unstructured information indices may be
undertaken quickly and simply with the help of the Denodo Virtual
DataPort administration tool. It is normally
necessary to previously create a *data source* for the source that will
encapsulate data to access it that is reusable by the different
*wrappers* acting on it.

Wrappers can be created for semi-structured Web sources (WWW) in a fully
graphic manner with Denodo ITPilot. It is also
possible to create *wrappers* for specific applications using the
*CUSTOM* wrapper type.

Should you wish to create *data sources* and *wrappers* directly using
VQL, without the help of the administration tool, section :ref:`Generating
Wrappers and Data Sources` of this document describes how to do so. In
general, use of the administration tool is strongly recommended for
these tasks, as the process is much simpler and automatic.

Once the *wrapper* has been created for a source, all that needs to be
done is link it to the adequate search method or methods from the base
relation that represents said data in the system (see section :ref:`Creating
a Base View`). You can now make queries on the base relation.
