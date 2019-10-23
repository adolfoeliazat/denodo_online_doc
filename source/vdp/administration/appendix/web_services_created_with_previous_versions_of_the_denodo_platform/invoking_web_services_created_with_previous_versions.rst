====================================================
Invoking Web Services Created with Previous Versions
====================================================

.. warning:: This section is about the Web services created with previous
   version of the Denodo Platform.

This section describes how to invoke the different types of Web
services.

The **Context Path** column of the list of the Web services container
shows the path to each Service relative to the URL where the embedded
Web container is running (by default: \http://localhost:9090).

This path contains a list of the exported versions for that Service.

The relative paths ``/services``, ``/rest``, ``/json``, ``/html`` and
``/rss`` show the available operations for each Web service type.

**Example**: the following table contains a list of the URLs of the
information pages:

 
.. table:: URLs of Web Service’s information pages
   :name: URLs of Web Service’s information pages

   +----------------------+------------------------------------------------------+
   | Web Services Version | URL                                                  |
   +======================+======================================================+
   | SOAP                 | \http://acme:9090/server/admin/testWS/services       |
   +----------------------+------------------------------------------------------+
   | REST                 | \http://acme:9090/server/admin/testWS/rest           |
   +----------------------+------------------------------------------------------+
   | JSON                 | \http://acme:9090/server/admin/testWS/json           |
   +----------------------+------------------------------------------------------+
   | HTML                 | \http://acme:9090/server/admin/testWS/html           |
   +----------------------+------------------------------------------------------+
   | RSS                  | \http://acme:9090/server/admin/testWS/rss            |
   +----------------------+------------------------------------------------------+

These information pages list the operations of the Web Service.

The SOAP information page contains a link to the ``wsdl`` file of the
service.

The information page of the XML service contains a link to the XML
Schema of the XML output of each operation.

The links to the XML Schemas of the XML Web service’s output have this
format: \http://host:port/server/databaseName/serviceName/rest/opName/xsd

**Example**: If the ``testWS`` Web service has an operation called
``getINTERNET_INC``, the URL of the XML Schema of its response is:

 

.. code-block:: none
   :caption: Obtaining the XSD Schema of a REST operation
   :name: Obtaining the XSD Schema of a REST operation

   http://acme:9090/server/admin/testWS/rest/getINTERNET_INC/xsd

Invoking the Operations of the REST Services
=================================================================================

This section explains how to invoke the operations of the REST services
(XML, JSON, HTML and RSS) of a Web service published from Virtual
DataPort.

Note that the examples of this section assume that you are invoking
operations that do not have a Custom Endpoint (see section :ref:`Defining a
Custom Endpoint`).

**Example**: Let us suppose that the ``testWS`` service has an operation
called ``getINTERNET_INC`` that requires no parameters. This operation
can be invoked as follows:

 
.. table:: Invoking a Web Service without parameters
   :name: Invoking a Web Service without parameters

   +----------------------+----------------------------------------------------------------+
   | Web Services Version | URL                                                            |
   +======================+================================================================+
   | REST                 | http://acme:9090/server/admin/testWS/rest/getINTERNET\_INC     |
   +----------------------+----------------------------------------------------------------+
   | JSON                 | http://acme:9090/server/admin/testWS/json/getINTERNET\_INC     |
   +----------------------+----------------------------------------------------------------+
   | HTML                 | http://acme:9090/server/admin/testWS/html/getINTERNET\_INC     |
   +----------------------+----------------------------------------------------------------+
   | RSS                  | http://acme:9090/server/admin/testWS/rss/getINTERNET\_INC      |
   +----------------------+----------------------------------------------------------------+

The following sections describe the different ways in which you can pass
input parameters to an operation, depending on the type of the
operation:

-  Operation that retrieves data from Virtual DataPort: see section
   :ref:`Invoking Operations that Retrieve Data from the Web Service (SELECT
   Queries)`.
-  Operation that inserts new data in the underlying source: see section
   :ref:`Invoking Operations that Insert New Data (INSERT Queries)`.
-  Operation that updates the data of an underlying source: see section
   :ref:`Invoking Operations that Update Data (UPDATE Queries)`.
-  Operation that deletes data from the underlying source: see section
   :ref:`Invoking Operations that Delete Data (DELETE Queries)`.

Invoking Operations that Retrieve Data from the Web Service (SELECT Queries)
----------------------------------------------------------------------------

There are two ways to pass input parameters to a Web service operation
that executes ``SELECT`` queries:

#. Send an HTTP ``GET`` request with the parameters in the URL.
   The syntax is the following: 
   
   .. code-block:: none
   
      http://host:port/server/databaseName/serviceName/{ rest|json|html|rss }/opName?paramName1=value1&paramNameN=valueN

   This syntax is valid to invoke any of the REST versions of the Web service: 
   ``XML``, ``JSON``, ``HTML`` or ``RSS``.

#. Or, send an HTTP ``POST`` request.
   The parameters of the query are sent in the body of the request.
   
   To invoke an XML, RSS or HTML service, add an XML document with the
   input parameters, to the body of the request and add the HTTP header
   ``Content-type=application/xml``.
   
   To invoke a JSON service, add a JSON document with the input
   parameters, to the body of the request and add the HTTP header
   ``Content-type=application/json``.

**Example**: Let us suppose that the ``testWS`` service has an operation
called ``getINTERNET_INCBYIINCID`` with two input parameters
called ``iinc_id`` and ``taxid``. The operation can be invoked by
sending a request to the following URLs:

 
.. table:: Web service: invoking an operation with two input parameters
   :name: Web service: invoking an operation with two input parameters

   +----------------------+-------------------------------------------------------------------------------------+
   | Web Services Version | URL                                                                                 |
   +======================+=====================================================================================+
   | XML                  | http://acme:9090/server/admin/testWS/rest/getINTERNET_INC?iinc_id=1&taxid=12345678  |
   +----------------------+-------------------------------------------------------------------------------------+
   | JSON                 | http://acme:9090/server/admin/testWS/json/getINTERNET_INC?iinc_id=1&taxid=12345678  |
   +----------------------+-------------------------------------------------------------------------------------+
   | HTML                 | http://acme:9090/server/admin/testWS/html/getINTERNET_INC?iinc_id=1&taxid=12345678  |
   +----------------------+-------------------------------------------------------------------------------------+
   | RSS                  | http://acme:9090/server/admin/testWS/rss/getINTERNET\_INC?iinc\_id=1&taxid=12345678 |
   +----------------------+-------------------------------------------------------------------------------------+

Instead of putting the input parameters in the URL, you can send a
``POST`` request to the URL of the operation:

.. code-block:: none

   http://acme:9090/server/admin/testWS/{ rest | json | rss | html }/getINTERNET_INC

and send the input parameters in the body of the request. If you send a
request to the REST (XML), RSS or HTML services, you can send an XML
document like this one with the HTTP header
``Content-type=application/xml``:

 

.. code-block:: xml
   :caption: Web service: sample XML document sent in the body of a POST request to a SELECT operation
   :name: Web service: sample XML document sent in the body of a POST request to a SELECT operation

   <internet_inc>
       <iinc_id>1</iinc_id>
       <taxid>12345678</taxid>
   </internet_inc>


The root element of the XML document has to be the name of the view,
except when you are invoking an operation that was created from the
**Publish from VQL expression** dialog (see section :doc:`Operations Tab <./publishing_web_services>`).
In this case, the root element has to be ``vql_operations`` and not the
name of the view.

If you invoke the JSON service, you can send a JSON document like this
one with the HTTP header ``Content-type=application/json``:

 

.. code-block:: json
   :caption: Web service: sample JSON document sent in the body of a POST request to a SELECT operation
   :name: Web service: sample JSON document sent in the body of a POST request to a SELECT operation

   {
     "iinc_id": 1,
     "taxid": "12345678"
   }
                                                                

When the Web service receives any of these requests, it will execute the
following query:

 

.. code-block:: sql
   :caption: Web service: example of a SELECT query executed by an operation
   :name: Web service: example of a SELECT query executed by an operation

   SELECT IINC_ID, SUMMARY, TTIME, TAXID, SPECIFIC_FIELD1, SPECIFIC_FIELD2 
   FROM internet_inc 
   WHERE iinc_id = 1 AND taxid = '12345678'
   CONTEXT ('i18n' = 'us_pst') 
                                          

If the type of an input parameter of an operation is compound
(``register`` or ``array``), its value is represented by using the
``ROW`` and ``{}`` VQL constructors (see the section :ref:`Conditions with
compound values` of the VQL Guide for more details
about representing compound).

.. note:: By default, an input parameter is “case insensitive”. That is,
   it does not matter if you pass the parameter ``iinc_id`` or ``IINC_ID``.
   However, if you rename an input parameter, it becomes “case sensitive”.

The format of the result will depend on the Web service version you are
invoking. If you invoke the XML Web service, you will obtain an XML
document, if you invoke the JSON Web service, you will obtain a JSON
document, etc.

 
**Example of invoking a Web service with compound parameters**: Let us
suppose that the ``testWS`` Web service has an operation called
``getREVENUESUM``. This operation publishes the view ``REVENUESUM``
created in the section :ref:`Creating Conditions with the Compound Values
Editor`, which has an input parameter of type array called
``clients``. Each record of the array has one ``text`` field that
represents a company’s tax identifier. The operation returns the sum
of the revenue of the companies in the input parameter. The URL to
invoke the REST version of this operation is:

 

.. code-block:: none
   :caption: Invoking a REST Web service with an array parameter
   :name: Invoking a REST Web service with an array parameter

   http://acme:9090/server/admin/testWS/rest/getREVENUESUM?clients={ROW('B78596011'),ROW('B78596012')}                              

By default, the REST, JSON and HTML Services return an error if a client
passes parameters *in the URL* that do not belong to the published view.
If a client needs to ignore the extra parameters, it must add the
``validateparams`` parameter to the URL.

For example, if a client invokes the following URL, the Service returns
an error because ``made_up_parameter`` does not belong to the published
view.

 

.. code-block:: none

   http://acme:9090/server/admin/testWS/rest/getINTERNET\_INC/?made\_up\_parameter=1

However, if a client invokes the following URL, the Service will ignore
``made_up_parameter`` and return the result of the query.


.. code-block:: none

   http://acme:9090/server/admin/testWS/json/getINTERNET\_INC/validateparams/false?made\_up\_parameter=1

The parameter ``validateparams`` has to be added to the URL after the
name of the operation and is valid for the REST, JSON, HTML and RSS
services.

.. important:: When the parameters are sent in the body of the request,
   instead of in the URL, and the JSON or XML document contains parameters
   that do not belong to the published view, they are ignored and the
   request is processed anyway.



Invoking Operations that Insert New Data (INSERT Queries)
---------------------------------------------------------

There are three ways to pass input parameters to a Web service operation
that executes ``INSERT`` queries:

#. Send a ``GET`` request with the parameters in the URL. The syntax is
   the following:
   
   .. code-block:: none
   
      {rest|json|html|rss}/opName?paramName1=value1&paramNameN=valueN

   This syntax is valid to invoke any of the REST versions of the Web
   service: ``XML``, ``JSON``, ``HTML`` or ``RSS``.
#. Send an HTTP ``POST`` request.
#. Or, send an HTTP ``PUT`` request.

For options 2 and 3 the input parameters are sent in the body of the
request.

The request sent to the XML (REST), RSS and HTML versions of the Web
service must have:

-  The HTTP header ``Content-type=application/xml``
-  A body with an XML document, which contains the input parameters of
   the operation.

The request sent to the JSON version of the Web service must have:

-  The HTTP header ``Content-type=application/json``
-  A body with a JSON document, which contains the input parameters of
   the operation.

.. note:: When invoking an ``INSERT`` operation, you have to provide a
   value for all the input parameters of the operation.

.. note:: ``INSERT`` operations are not supported by the HTML Web
   services.

**Example**

Let us say that we have published a Web service with an operation
``insertINTERNET_INC`` that executes an ``INSERT`` query on the view
``internet_inc``. The parameters of this operation are ``IINC_ID``,
``SUMMARY``, ``TTIME``, ``TAXID``, ``SPECIFIC_FIELD1`` and
``SPECIFIC_FIELD2``.

If you send a ``POST`` or ``PUT`` request with the header
``Content-type=application/xml``, the body of the request has to
contain an XML document like this one:

 

.. code-block:: xml
   :caption: Web service: XML document sent in the body of a POST request to an INSERT operation
   :name: Web service: XML document sent in the body of a POST request to an INSERT operation

   <internet_inc>
       <IINC_ID>100</IINC_ID>
       <SUMMARY>New incident</SUMMARY>
       <TAXID>12345678</TAXID>
       <TTIME>Aug 22, 2011 1:04:22 PM</TTIME>
       <SPECIFIC_FIELD1>3</SPECIFIC_FIELD1>
       <SPECIFIC_FIELD2><![CDATA[specific info]]></SPECIFIC_FIELD2>
   </internet_inc>
                                         

When the operation receives this document, executes the following query:


.. code-block:: sql
   :caption: Web service: example of INSERT query executed by an operation
   :name: Web service: example of INSERT query executed by an operation

   INSERT INTO internet_inc 
       (iinc_id, summary, ttime, taxid, specific_field1, specific_field2)     VALUES 
       (100, 
        'New incident',
        TO_DATE('MMM d, yyyy h:mm:ss a', 'Aug 22, 2011 1:04:22 PM'), 
        '12345678', 
        '3', 
        'specific info') 
   CONTEXT ('i18n' = 'us_pst')   
          

Note that the value of the field ``SPECIFIC_FIELD2`` of the XML document
contains a ``CDATA`` section, which is properly treated as you can see
in the executed query.

If you send a ``POST`` or ``PUT`` request with the header
``Content-type=application/json``, the body of the request has to
contain a JSON document like this one:

 

.. code-block:: json
   :caption: Web service: JSON document sent in the body of a POST request to an INSERT operation
   :name: Web service: JSON document sent in the body of a POST request to an INSERT operation

   {
       "IINC_ID": 100,
       "SUMMARY": "New incident",
       "TTIME": "Aug 22, 2011 1:04:22 PM",
       "TAXID": "12345678",
       "SPECIFIC_FIELD1": "3",
       "SPECIFIC_FIELD2": "specific info "
   }


The type of the parameter ``TTIME`` is ``date``. This means that the
value of the element in the JSON or XML document must have the format
expected by the ``I18N`` of the query associated with the operation of
the Web service. The result of the statement
``DESC VQL WEBSERVICE <web service name>``, contains the query
associated with each operation (parameter ``VQL`` of the parameter
``OPERATION``).



Invoking Operations that Update Data (UPDATE Queries)
-----------------------------------------------------

There are three ways to pass input parameters to a Web service operation
that executes ``UPDATE`` queries:

#. Send an HTTP ``GET`` request with the parameters in the URL. The
   syntax is the following:

   .. code-block:: none

      http://host:port/server/databaseName/serviceName/{ rest|json|html|rss }/opName?paramName1=value1&paramNameN=valueN

#. Send an HTTP ``POST`` request.
#. Or, send an HTTP ``PUT`` request.

For options 2 and 3 the input parameters are sent in the body of the
request.

The request sent to the XML (REST), RSS and HTML services must have:

-  The HTTP header ``Content-type=application/xml``
-  A body with an XML document, which contains the input parameters of
   the operation.

The requests sent to the JSON services must have:

-  The HTTP header ``Content-type=application/json``
-  A body with a JSON document, which contains the input parameters of
   the operation.

.. note:: When invoking an ``UPDATE`` operation, you have to provide a
   value for all the ``New`` input parameters of the operation. These
   parameters are the new values of the cells of the updated rows.

.. note:: ``UPDATE`` operations are not supported by the HTML Web
   services.

**Example**

Let us say that we have published a Web service with an operation
``updateINTERNET_INC`` that executes an ``UPDATE`` query on the view
``internet_inc``. This view has the following fields:

-  ``IINC_ID``
-  ``SUMMARY``
-  ``TTIME``
-  ``TAXID``
-  ``SPECIFIC_FIELD1``
-  ``SPECIFIC_FIELD2``

So, this operation will have the following input parameters:

-  ``IINC_ID``
-  ``SUMMARY``
-  ``TTIME``
-  ``TAXID``
-  ``SPECIFIC_FIELD1``
-  ``SPECIFIC_FIELD2``
-  ``NewIINC_ID``
-  ``NEWSUMMARY``
-  ``NEWTTIME``
-  ``NEWTAXID``
-  ``NEWSPECIFIC_FIELD1``
-  ``NEWSPECIFIC_FIELD2``

As we explained in the section :doc:`Operations Tab <./publishing_web_services>`, the parameters with
the prefix ``NEW`` are the new values of the fields of the selected
rows. The parameters that do not have the prefix ``NEW`` are used to
select the affected rows.

If you send a ``POST`` or ``PUT`` request with the header
``Content-type=application/xml``, the body of the request has to
contain an XML document like the following:

 

.. code-block:: xml
   :caption: Web service: XML document sent in the body of a POST request to an UPDATE operation
   :name: Web service: XML document sent in the body of a POST request to an UPDATE operation

   <internet_inc>
       <IINC_ID>155</IINC_ID>
       <NEWIINC_ID>155</NEWIINC_ID>
       <NEWSUMMARY>Error in ADSL router</NEWSUMMARY>
       <NEWTTIME>Jun 6, 2005 10:19:41 PM</NEWTTIME>
       <NEWTAXID>B78596011</NEWTAXID>
       <NEWSPECIFIC_FIELD1>1</NEWSPECIFIC_FIELD1>
       <NEWSPECIFIC_FIELD2>1</NEWSPECIFIC_FIELD2>
   </internet_inc>
                                      

When the operation receives a request with this XML document, executes
the following query:

 

.. code-block:: sql
   :caption: Web service: example of UPDATE query executed by an operation
   :name: Web service: example of UPDATE query executed by an operation

   UPDATE internet_inc SET 
       iinc_id=155, 
       summary = 'Error in ADSL router', 
       ttime = TO_DATE('MMM d, yyyy h:mm:ss a', 'Jun 6, 2005 10:19:41 PM'), 
       taxid = 'B78596011', 
       specific_field1 = '1', 
       specific_field2 = '1' 
   WHERE 
       iinc_id = 155 
   CONTEXT ('i18n' = 'us_pst')  

Invoking Operations that Delete Data (DELETE Queries)
-----------------------------------------------------

There are three ways to pass input parameters to a Web service operation
that executes ``DELETE`` queries:

#. Send an HTTP ``GET`` request with the parameters in the URL. The
   syntax is the following:


   .. code-block:: none

      http://host:port/server/databaseName/serviceName/{ rest|json|rss }/opName?paramName1=value1&paramNameN=valueN


#. Send an HTTP ``DELETE`` request. The parameters also have to be sent in
   the URL as with the ``GET`` method.


#. Send an HTTP ``POST`` request. The input parameters are sent in the body
   of the request sent to the Web service. The ``POST`` request must have:

   -  If the request is sent to the XML (REST) and RSS versions of the Web
      Service, the HTTP header ``Content-type=application/xml`` and a
      body with an XML document, which contains the input parameters of the
      operation.
   -  If the request is sent to the JSON version of the Web Service it must
      contain the HTTP header ``Content-type=application/json`` and a
      body with a JSON document, which contains the input parameters of the
      operation.


.. note:: ``DELETE`` operations are not supported by HTML published web
   services.

**Example**

Let us say that we have published a Web service with an operation
``deleteINTERNET_INC`` that executes a ``DELETE`` query on the view
``internet_inc``. The parameters of this operation are ``IINC_ID``,
``SUMMARY``, ``TTIME``, ``TAXID``, ``SPECIFIC_FIELD1`` and
``SPECIFIC_FIELD2``.

If you send a ``DELETE`` request, the URL of the request must be like
this: 

.. code-block:: none
   :caption: Web service: example of DELETE request to a DELETE operation
   :name: Web service: example of DELETE request to a DELETE operation

   http://host:port/server/databaseName/serviceName/rest/deleteINTERNET?IINC_ID=1

If you send a ``POST`` request with the header
``Content-type=application/xml``, the body of the request has to
contain an XML document like this:

 

.. code-block:: xml
   :caption: Web service: XML document sent in the body of a POST request to a DELETE operation
   :name: Web service: XML document sent in the body of a POST request to a DELETE operation

   <internet_inc>
       <IINC_ID>1</IINC_ID>
   </internet_inc>


If you send a ``POST`` request with the header
``Content-type=application/json``, the body of the request has to
contain a JSON document like this:

 

.. code-block:: json
   :caption: Web service: JSON document sent in the body of a POST request to a DELETE operation
   :name: Web service: JSON document sent in the body of a POST request to a DELETE operation

   {
       "IINC_ID": 1,
   }
                                                                      

When the operation receives any of these requests, it executes the
following query:


.. code-block:: sql
   :caption: Web service: example of DELETE query executed by an operation
   :name: Web service: example of DELETE query executed by an operation

   DELETE FROM internet_inc
   WHERE IINC_ID = 1
   CONTEXT ('i18n' = 'us_pst')
   

HTML Output Configuration
=================================================================================

The HTML version of the Denodo Web services can be invoked with
additional parameters in order to configure the HTML table that displays
the results of the queries.

.. note:: These options cannot be used when invoking operations that
   have a Custom endpoint (see section :ref:`Defining a Custom Endpoint`).

The configuration parameters are the following:

-  ``shownumresults``. If ``true``, the table will also display the
   number of rows obtained by the query.
-  ``intervalsize``. If present, the results of the query will be
   paginated. The value of the parameter is the number of results in
   each page.
-  ``maxresults``. Maximum number of results to be displayed. If the
   query returns more rows, all excess results will be omitted.
-  ``cellwidth``. Maximum cell width expressed in number of characters.
   If the text of a cell is wider, the text will be divided in several
   lines.
-  ``cellheight``. Maximum number of lines in a cell after having split
   the text according to the ``cellwidth`` parameter. If this is
   exceeded, all the cells of this column will have a scroll bar.
-  ``width``. Maximum width (in pixels) of the table. If the size is
   exceeded, a scroll bar is added.
-  ``height``. Maximum height (in pixels) of the table. If the size is
   exceeded, a scroll bar is added.
-  ``noescapehtml``. List of the names of the columns whose HTML code
   will not be escaped (separate each name with comma). By default, the
   HTML of all the cells is escaped, unless its column name is in this
   list.

These parameters must be indicated in the part of the URL corresponding
to the access path (before the query parameters) in the following
format:

paramName1/value1/…/paramNameN/valueN

For example, the following expression invokes the ``getINTERNET_INC``
operation, limiting the number of results displayed to 50 and setting
the number of rows per page to 10.

 

.. code-block:: none
   :caption: Invoking the HTML Web service with configuration parameters
   :name: Invoking the HTML Web service with configuration parameters

   http://acme:9090/server/admin/testWS/json/getINTERNET_INC/maxresults/50/intervalsize/10/?iinc_id=1/


JSON Output Configuration
=================================================================================

The JSON version of the Web service can return JSON data, prefixed with
the name of a function (also called *JSON with padding* or *JSONP*).
That way, when a browser receives the response, it receives a script
rather than data.

To obtain this, add the parameter ``jsoncallback`` to the parameters of
the URL. E.g.

 

.. code-block:: none
   :caption: Invoking the JSON Web service with padding (JSONP)
   :name: Invoking the JSON Web service with padding (JSONP)

   http://acme:9090/server/admin/testWS/json/getINTERNET_INC?jsoncallback=js_function


This will return the following: 

.. code-block:: javaScript

   js_function(
       <result of the query>
   );


.. note:: To use this feature, the published view cannot have a field
   called ``jsoncallback``.
