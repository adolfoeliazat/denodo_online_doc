======================================================
After Importing the Metadata into the New Installation
======================================================

After installing the new version of the Denodo Platform and migrating
the metadata, follow these steps:

#. If SSL is enabled on the previous version, enable it in the new one as
   well, following the instructions of the section :ref:`Enable SSL/TLS in the Denodo Platform`
   of the Installation Guide. Consider the
   following:

   -  Do this for all the servers and their administration
      tools.
   -  If you are upgrading from a version *prior* to Denodo 6.0, the process to enable SSL changed since then. The section :ref:`Enable SSL/TLS in the Denodo Platform` (Installation Guide) explains how to do this.
   -  Copy the *KeyStore* file (the file of the property
      ``com.denodo.security.ssl.keyStore``) to the new installation to
      continue using the same private key to secure communications.
   -  Import the public key of this certificate into the *TrustStore* of
      the new installation (:file:`{<NEW_DENODO_HOME>}/jre/lib/security/cacerts`).
      
      We do not recommend replacing the cacerts file with the cacerts of a previous version because it will lack the certificates of the public Certification Authorities (CA) added to the cacerts of Denodo 7.0.  
      
.. csantos@2017/11/15: good idea: explain how to export the current certificate so we use the new public certificates of Java 8.

2. If the connection to any data source or to the cache database is
   performed using SSL and the certificate used by the source is not signed
   by a known certification authority, import into the Denodo’s
   *TrustStore* (usually,
   :file:`{<NEW_DENODO_HOME>}/jre/lib/security/cacerts`) the public key of the
   certificate used by that source.


#. In the clients that will connect to Denodo |version|, upgrade the JDBC driver
   they use to connect to Denodo.

   -  Starting with Denodo 6.0, the JDBC driver is located at the directory
      :file:`{<DENODO_HOME>}/tools/client-drivers/jdbc`.

   -  The driver is not compatible between major versions. I.e. you cannot use
      the JDBC driver of the |version| version to connect to previous major versions of
      Denodo nor use the JDBC driver of a previous version to connect to
      Denodo |version|.

#. Whenever is possible, configure the ODBC clients to use the Denodo
   ODBC driver instead of the PostgreSQL driver. The performance of the new
   driver is better.

#. If you have JDBC data sources that connect to a Teradata database, make
   sure that the default collation of that Teradata database is binary. The
   section :ref:`Access to Teradata` explains why this is necessary.

#. If you are using the support to store metadata in a Version Control
   System (VCS), change the VCS settings to point to the new branch or repository. This is the branch/repository you created when following the steps of the section :ref:`Before Beginning the Migration`. 
   
#. Carefully read the section :ref:`Backward Compatibility`,
   which lists the changes introduced in the versions 7.0, 6.0, 5.5 and 5.0 that
   may affect the applications that interact with Denodo.

|

.. rubric:: Changes to the Virtual DataPort Administration tools:

1. If in the administration tools of the previous version you increased the
   query timeout, do the same in the administration tools of the new
   version.

#. If you enabled Kerberos authentication, you have to enable it in the administration tools as well.

   After importing the metadata to the new version of the Server, if the previous
   installation is configured to use Kerberos to authenticate users, the
   new installation will be configured to do that as well.

|

.. rubric:: If upgrading from Denodo 4.7

When upgrading from Denodo 4.7, update the “driver class path” of each JDBC data source that points to
an Oracle database. If you do not do this, the data source will *not*
use the latest version of the Oracle JDBC driver included with the
Denodo Platform. To change this, follow these steps:

-  Edit the data source.
-  Select the value of the field **Database URI** and press **Ctrl+C** to
   copy the value to the clipboard.
-  Select a different adapter and select again the adapter of the
   appropriate Oracle version.
-  Paste back (**Ctrl+V**) the URI you copied.
-  Save the changes.

Migrate Salesforce Base Views to Use the New Data Source
========================================================

For versions of the Denodo Platform prior to 7.0, Denodo provides a *Denodo Connect* component to connect to Salesforce. This component is provided by Denodo, but not included in the product. Starting with Denodo 7.0, Denodo includes a :ref:`native data source to Salesforce.com<Salesforce Sources>`.

Users that retrieve data from Salesforce are advised to stop using the *Denodo Connect* component and use the new data source because the *Denodo Connect* component will not be actively maintained.

|

This section explains how to migrate the existing Salesforce base views to use the new data source.

The main difference when creating the Salesforce base views with the new connector is that now, you only need one Salesforce data source; in previous versions you may need more. In addition, you do not need to enter the connection details for every base view you create.

|

.. rubric:: Part #1: Create the Data Source

First, create the Salesforce data source, copying the connection settings from one of the existing base views. To do this, follow these steps:

1. On the menu **File**, click **New** > **Data source** > **Salesforce**.
2. Edit one of the "old" Salesforce base views and click **Source refresh** to show the input parameters of the base view.

   -  If the parameters ``USERNAME`` and ``PASSWORD`` are *not empty*, in the dialog of the *new data source*, select **User name and password** in the list **Authentication flow**.
   
   -  Otherwise, in the list **Authentication flow**, select **Web server**.

The two tables below list the parameters you have to copy from the "old base view" to the dialog of the new data source. The parameters are different depending on the selected *Authentication flow*.

.. table:: Parameters to copy when using the *Authentication flow Web server*

   +----------------------------------+-----------------------------------------+
   | Parameter of the "Old Base View" | Field in the New Salesforce Data Source |
   +==================================+=========================================+
   | CLIENT_ID                        | Client identifier and enter the         |
   |                                  | Client password                         |
   +----------------------------------+-----------------------------------------+
   | ACCESS_TOKEN                     | Access Token                            | 
   +----------------------------------+-----------------------------------------+
   | REFRESH_TOKEN                    | Refresh Token                           | 
   +----------------------------------+-----------------------------------------+
   | BASE_URL                         | Base URL                                | 
   +----------------------------------+-----------------------------------------+
   | API_VERSION                      | API version                             |
   +----------------------------------+-----------------------------------------+
   | PROXY_HOST, PROXY_PORT           | If not empty, click **Configure proxy** |
   |                                  | and enter the connection details to the |
   |                                  | proxy.                                  |
   +----------------------------------+-----------------------------------------+
   
.. table:: Parameters to copy when using the *Authentication flow User name and password*

   +----------------------------------+-----------------------------------------+
   | Parameter of the "Old" Base View | Field in the New Salesforce Data Source |
   +==================================+=========================================+
   | CLIENT_ID                        | Client identifier and enter the         |
   |                                  | Client password                         |
   +----------------------------------+-----------------------------------------+
   | USER_NAME (only if you selected  | User identifier and enter the User      |
   | *User name and password*)        | password                                |
   +----------------------------------+-----------------------------------------+
   | SECURITY_TOKEN (only if you      | Security token                          |
   | selected  *User name and         |                                         |
   | password*)                       |                                         |
   +----------------------------------+-----------------------------------------+
   | API_VERSION                      | API version                             |
   +----------------------------------+-----------------------------------------+
   | PROXY_HOST, PROXY_PORT           | If not empty, click **Configure proxy** |
   |                                  | and enter the connection details to the |
   |                                  | proxy.                                  |
   +----------------------------------+-----------------------------------------+

.. rubric:: Part #2: Create the Base Views

After creating the data source, create the base views over the new data source, *with the same name as the existing ones*. By creating them with the same name, they will replace the "old" ones thus making the migration easier.

-  *For each base view* created over the custom data sources linked to the class "SalesforceRestWrapper" and "SalesforceRestSelectWrapper" (to check the class of a custom data source, open it and check the value of **Class name**):

   1. Copy the value of the parameter ``ENTITY`` of the "old base view".
   2. Open the new data source and click **Create base view**.
   3. In the tree of entities, select the entity of step #1 and click **Create selected**.
   4. In the dialog to create the view, enter the same name as the existing view.
   
   Repeat this for each base view of the custom data source.
   
   With the new data source, you can create base views over several Salesforce objects at once. However, you have to do it one by one because it is the only way of replacing existing views. When you create several views at once, they cannot have the same name as views that already exist.

-  *For each base view* created over the custom data source linked to the class "SalesforcePredefinedSOQLWrapper":

   1. Copy the SOQL query of the base view (parameter ``SOQL_QUERY``).
   #. Open the new data source and click **Create base view**.
   #. Click **Create from query**, enter the same name as the existing view, enter the SOQL query and click **Ok**.
   
-  If there are views created over a custom data source linked to the class "SalesforceSOQLWrapper", replace them either with a base view created from an entity or a base view created from a SOQL query. There is no support to create views that allow to execute any SOQL query.
