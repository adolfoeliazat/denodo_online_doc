=================================================================================
Input Parameters of the RESTful Web Service
=================================================================================

This section lists the parameters you can add to the URL of the web service to filter the results and the customize the output.

-  For view resources, you can add conditions "<field name> = <value>" by adding parameters to
   the URL with the following format:
   
   ``field_name1=value_1&...&fieldNameN=valueN``.
   
   For example,
   ``http://localhost:9090/denodo-restfulws/support/views/customer?name=Denodo``
   
   When you filter by a datetime field, you have to provide the value with a specific format, depending on the value of the field:
   
   -  ``localdate``: provide the value with the format "<yyyy>-<MM>-<dd>" (see the meaning of each component on the table below). For example:
   
      \http://.../views/employee?hire_date=2018-05-30

   -  ``time``: provide the value with the format "<HH>:<mm>:<ss>", optionally followed by nanoseconds. For example:

      \http://.../views/rate_conversion?instant=23:31:59.1234
    
   -  ``timestamp``: provide the value with the format "<yyyy>-<MM>-<dd>T<HH>:<mm>:<ss>", optionally followed by nanoseconds. For example:
   
      \http://.../views/sale?sale_date=2017-12-30T18:59:59

   -  ``timestamptz``: provide the value with the format "<yyyy>-<MM>-<dd>T<HH>:<mm>:<ss>Z". Optionally, you can put nanoseconds between the seconds and the time zone. For example:
   
      \http://.../views/sale?sale_date=2017-12-30T18:59:59.123456-0800
      
   -  ``date`` (deprecated): "<yyyy>-<MM>-<dd>T<HH>:<mm>:<ss>Z". For example:
   
      \http://.../views/sale?sale_date=2017-12-30T18:58:59-0800

.. csv-table:: Meaning of the patterns for datetime values
   :header: "Pattern", "Meaning of the Pattern"
   
   "yyyy", Year"
   "MM", "Month in year"
   "dd", "Day in month"
   "HH", "Hour in day (0-23)"
   "mm", "Minute in hour"
   "ss", "Second in minute"
   "S", "Nanosecond (up to 6 digits). This is optional"
   "Z", "Time zone: Sign (+ or -)<Hours><Minutes>. For example: -0800."

.. 

   .. note:: In REST Web services, the fields that have been marked as
      “No searchable” cannot be used to filter the results.
   

-  For view resources, you can obtain a row by specifying its primary key.

   For example, ``http://.../support/views/customer/1``
   returns the row of the view ``customer`` whose primary key has value
   ``1``. If the primary key is formed by more than one field, you have to
   separate its values with comma. For example
   ``http://.../support/views/viewA/1,value1``.
   
   When one of the fields is of one the datetime types, provide the value with the same format as when you enter a condition "<field name> = <value>" (see above).

-  For view resources, to traverse an association you have to specify the
   primary key of a row and the name of the endpoint of the association.
   E.g. ``http://.../support/views/customer/1/orders``


.. table:: Parameters supported by the Denodo RESTful Web service and published REST Web services
   :name: Parameters supported by the Denodo RESTful Web service and published REST Web services
   
   +------------------+-----------------------------------------------------------------------------+
   | Parameters of the Web Service RESTful and REST Web Services                                    |
   +==================+=============================================================================+
   | $filter          | Filters the rows of a view using any condition. Any expression              |
   |                  | that can appear in the WHERE clause of a VQL query can be used.             |
   |                  | I.e. this parameter can contain VQL functions, any operator, etc.           |
   |                  |                                                                             |
   |                  | As in a VQL query, in the ``$filter`` parameter, the names of the           |
   |                  | fields with non-standard or uppercase characters have to be                 |
   |                  | surrounded with double quotes. E.g.                                         |
   |                  | \http://.../view?$filter=TRIM(%22fieldWithUpperCase%22)%3C%3E%27value%27    | 
   |                  |                                                                             |
   |                  | The last part of the URL is equivalent to                                   |
   |                  | ``TRIM("fieldWithUpperCase")<>'value'``, but URL encoded.                   |
   |                  |                                                                             |
   |                  | The double quotes are only necessary in the $filter parameter               |
   |                  | and not in the parameters like ``<field name>=<value>``                     |
   |                  |                                                                             |
   |                  | To filter by a localdate field, you should use these functions              |
   |                  | :ref:`TO_LOCALDATE`, :ref:`TO_TIMESTAMP`, :ref:`TO_TIME` or                 | 
   |                  | :ref:`TO_TIMESTAMPTZ` to create the value of the appropriate type.          |
   |                  |                                                                             |
   |                  | E.g.,                                                                       |
   |                  | \http://.../view?$filter=date_field%3C%3E\                                  |
   |                  | TO_LOCALDATE%28%22yyyy-MM-dd%22,%222013-06-30%22%29                         |
   |                  |                                                                             |
   |                  | The last part of the URL is equivalent to                                   |
   |                  | ``date_field<>TO_LOCALDATE('yyyy-MM-dd','2013-06-30')``, but URL            |
   |                  | encoded.                                                                    |
   |                  |                                                                             |
   |                  | See more about the function TO_DATE in the appendix                         |
   |                  | :ref:`date_processing_functions_to_date` of the VQL Guide.                  |
   |                  |                                                                             |
   |                  | .. note:: The fields that have been marked as “No searchable”               |
   |                  |    cannot be added to this parameter.                                       |
   +------------------+-----------------------------------------------------------------------------+
   | $select          | The values allowed in this parameter depend on the check box *Process       |
   |                  | functions in $select parameter* (tab *Advanced* of the configuration of the |
   |                  | service):                                                                   |
   |                  |                                                                             |
   |                  | -  *If selected* (default): it is a comma-separated list of expressions.    |
   |                  |    These are the same expressions you can use in the SELECT clause of a     |
   |                  |    query.                                                                   |
   |                  |                                                                             |
   |                  |    Example: \http://.../views/customer?$select=concat(last_name, ', '       |
   |                  |    , first_name) AS full_name, upper(state)                                 |
   |                  |                                                                             |
   |                  |    (for clarity purposes, this URL is not escaped)                          |
   |                  |                                                                             |
   |                  |    Note that you can add an alias with "AS" to the result of a expression.  |
   |                  |                                                                             |
   |                  | -  *If cleared*: it is a comma-separated list of fields of the view.        |
   |                  |                                                                             |
   |                  |    Example: \http://.../support/views/customer?$select=cid,cname            |
   |                  |                                                                             |
   |                  |    This returns the fields "cid" and "cname" fields of the view             |
   |                  |    "customer".                                                              |
   |                  |                                                                             |
   |                  | .. todo: remove the following paragraph for Denodo 8 because it will no     |
   |                  |    longer be true.                                                          |
   |                  |                                                                             |
   |                  | By default, the global RESTful web service only supports field names in     |
   |                  | the parameter $select, not functions. The section :ref:`Configuring         |
   |                  | the RESTful Web Service` explains how to enable this feature.               |
   |                  |                                                                             |
   |                  | If the view has associations, you can project all the fields of the view at |
   |                  | the other side of the associations. To do this,                             |
   |                  | add "<role name> / \*"  to $select and add the parameter                    |
   |                  | $expand=<role name>.                                                        |
   |                  |                                                                             |
   |                  | For example, if the view "order" has an                                     |
   |                  | association whose role is "customer", you can project "<customer> / \*":    |
   |                  |                                                                             |
   |                  | \http://.../customer360/views/order?$select=id,date,\                       |
   |                  | customer%20/%20\*&$expand=customer                                          |
   |                  |                                                                             |
   |                  | You need to put a space (%20) between the role name (customer) and the      | 
   |                  | slash, and between the slash and the asterisk.                              |
   |                  |                                                                             |
   |                  | You can also project only a few fields of the view at the other side of the |
   |                  | association. For example,                                                   |
   |                  | \http://.../customer360/views/order?$select=id,date,\                       |
   |                  | customer/firstname,customer/lastname&$expand=customer                       |
   |                  |                                                                             |
   |                  | .. note:: If the view has primary key and associations, the                 |
   |                  |    output also includes the information about the associations              |
   |                  |    of each row. If you do not want this information, add the parameter      |
   |                  |    ``$displayRESTfulReferences=false`` to the URL or in the configuration   |
   |                  |    of the service, clear the check box *Display RESTful links* (*Settings*  |
   |                  |    tab).                                                                    |
   +------------------+-----------------------------------------------------------------------------+
   | $groupby         | Comma-separated list of fields to group by with.                            |
   |                  |                                                                             |
   |                  | E.g.,                                                                       |
   |                  | \http://.../view?$groupby=Name&$select=Name                                 |
   |                  |                                                                             |
   |                  |                                                                             |
   |                  | Adding a field to this parameter is equivalent to adding it to the GROUP BY |
   |                  | clause of a SQL statement. Therefore, if you add this parameter, you also   |
   |                  | have to add $select and only select fields listed in the parameter          |
   |                  | $groupby.                                                                   |
   |                  |                                                                             |
   |                  | The main use for the GROUP BY is to be able to perform a DISTINCT. I.e. to  |
   |                  | only obtain the rows that are different (like adding the clause DISTINCT to |
   |                  | a SQL statement). To do this, add all the fields of the view to this        |
   |                  | parameter.                                                                  |
   +------------------+-----------------------------------------------------------------------------+
   | $having          | Comma-separated list of fields to add to the HAVING clause of the query.    |
   |                  |                                                                             |
   |                  | E.g.,                                                                       |
   |                  | \http://.../customer?$select=state&$groupby=state&$having=%27CA%27          |
   |                  |                                                                             |
   |                  | %27 is the single quote encoded.                                            |
   +------------------+-----------------------------------------------------------------------------+
   | $expand          | Comma-separated list of role names to add to the EXPAND clause of the       |
   |                  | query sent by the web service to the Denodo server to satisfy this request. |
   |                  | This parameter is required when referencing expanded fields (i.e.           |
   |                  | fields from associated views) in the ``$select`` parameter: all the         |
   |                  | referenced roles must be included here. The section                         |
   |                  | :doc:`Associations<../associations/associations>` provides more details     |
   |                  | about this.                                                                 |
   |                  |                                                                             |
   |                  | E.g.,                                                                       |
   |                  | \http://.../customer?$select=is_from/cityname&$expand=is_from               |
   +------------------+-----------------------------------------------------------------------------+
   | $start_index     | Used for pagination in view resources.                                      |
   | and $count       |                                                                             | 
   |                  | For example,                                                                |
   |                  | \http://.../customer?$start_index=0&$count=10                               |
   |                  | returns the first 10 elements of the result (the first element has          |
   |                  | ``start_index=0``).                                                         |
   +------------------+-----------------------------------------------------------------------------+
   | $orderby         | Sorts the results by one or more fields. It is a comma-separated            |
   |                  | list of fields, each one followed by the modifier ASC (for                  |
   |                  | ascending order) and DESC (for descending order). For example,              |
   |                  | \http://.../support/views/customer?$orderby=cid+ASC                         |
   |                  |                                                                             |
   |                  | .. note:: In REST Web services, the fields that have been marked            |
   |                  |    as “Do not output” cannot be added to this parameter.                    |
   +------------------+-----------------------------------------------------------------------------+
   | $format          | Selects the representation of the data. This parameter is an                |
   |                  | alternative to sending the HTTP header Accept. The value of                 |
   |                  | this parameter overrides the value of the Accept header.                    |
   |                  |                                                                             |
   |                  | Possible values:                                                            |
   |                  |                                                                             |
   |                  | -  json                                                                     |
   |                  | -  xml                                                                      |
   |                  | -  html                                                                     |
   |                  | -  rss (only for published REST web services if configured to do so)        |
   |                  |                                                                             |
   |                  | In REST Web services, the possible values are the “Available                |
   |                  | representations” selected in the “Create REST Web service”                  |
   |                  | dialog (see section :ref:`Selecting the Default / Available                 |
   |                  | Representations`)                                                           |
   +------------------+-----------------------------------------------------------------------------+
   | $jsoncallback    | The JSON representation can return the data of a view prefixed              |
   |                  | with the name of a function. This is called JSON with padding or            |
   |                  | JSONP. That way, when a browser receives the response, it                   |
   |                  | receives a script rather than data.                                         |
   |                  |                                                                             |
   |                  | To obtain JSON with padding, add the parameter ``$jsoncallback``            |
   |                  | to the URL, along with the name of the function.                            |
   |                  |                                                                             |
   |                  | For example:                                                                |
   |                  | \http://localhost:9090/denodo-restfulws/\                                   |
   |                  | support/views/incidents?$format=JSON&$jsoncallback=js_function              |
   |                  | returns the following                                                       |
   |                  |                                                                             |
   |                  | .. code-block:: javascript                                                  |
   |                  |                                                                             |
   |                  |    js_function(                                                             |
   |                  |       <result of the query>                                                 |
   |                  |    )                                                                        |
   |                  |                                                                             |
   |                  |                                                                             |
   |                  | If you request the number of rows of a result set and add this              |
   |                  | parameter, it returns the number of rows as a parameter of a                |
   |                  | function                                                                    |
   |                  |                                                                             |
   |                  | Example:                                                                    |
   |                  |                                                                             |   
   |                  | \http://localhost:9090/denodo-restfulws/support/views/incidents/\           |
   |                  | $count?state=California&$format=JSON&$jsoncallback=js_function              |
   |                  |                                                                             |
   |                  | return this:                                                                |
   |                  |                                                                             |
   |                  | ``js_function(9382)`` being 9382 the number of rows of the view             |
   |                  | incidents that meet the condition state = 'California'.                     |
   |                  |                                                                             |  
   |                  | This parameter is ignored when requesting a representation                  |
   |                  | other than JSON.                                                            |
   +------------------+-----------------------------------------------------------------------------+
   | $displayRESTful\ | By default, the result of requesting a view contains, in each row,          |
   | References       | a link to the row itself and, for each association of the view, a link to   | 
   |                  | traverse the association.                                                   |
   |                  |                                                                             |
   |                  | If you do not want the result to include these links, add the               |
   |                  | parameter displayRESTfulReferences=false.                                   |
   |                  |                                                                             |
   |                  | In REST Web services published with the option “Display RESTful             |
   |                  | links” cleared, this parameter is ignored (see section                      |
   |                  | :ref:`Settings Tab (REST)`).                                                |
   +------------------+-----------------------------------------------------------------------------+
   | $noescapeHTML    | List of comma-separated fields whose values will not be HTML                |
   |                  | escaped.                                                                    |
   |                  |                                                                             |
   |                  | By default, in the HTML representation, all the values of type ``text`` are | 
   |                  | HTML escaped.                                                               |
   |                  |                                                                             |
   |                  | For example, if the value of a value of a field is                          |
   |                  | ``<a href="http://www.denodo.com">link</a>``, the user will see             |
   |                  | this text and not a link to \http://www.denodo.com                          |
   |                  |                                                                             |
   |                  | To avoid this, add the parameter ``$noescapeHTML=fieldname`` to             |
   |                  | not escape the values of the field ``fieldname``.                           |
   |                  |                                                                             |
   |                  | See more about this in the section :ref:`HTML-Escaping the Data`.           |
   +------------------+-----------------------------------------------------------------------------+

The following examples illustrate the use of some of these options:

.. table:: RESTful Web service: samples of URLs with parameters
   :name: RESTful Web service: samples of URLs with parameters

   +--------------------------------------+--------------------------------------+
   | Query                                | Result                               |
   +======================================+======================================+
   | /customer?cname=RoadRunner           | Customer with cname = 'RoadRunner'   |
   +--------------------------------------+--------------------------------------+
   | /customer?$select=cid,cname&$filter= | Fields cid and cname of customers    |
   | (cname                               | whose cname is either RoadRunner or  |
   | = 'RoadRunner' or cname =            | Coyote, ordered by cid in descending |
   | 'Coyote')&$orderby=cid DESC'         | order.                               |
   +--------------------------------------+--------------------------------------+
   | /customer?$format=json&$start\_index | Elements 11 to 20 of the customer    |
   | =10&$count=10                        | view, in JSON format.                |
   +--------------------------------------+--------------------------------------+

.. rubric:: Encode the Character Comma

In the parameters whose value is a list of fields separated by commas ($expand, $groupby, $having, $orderby and $noescapeHTML), you need to encode the character "," in each value. That is, replace ``,`` with ``%2C`` and only use the comma to separate field names. For example,
   
   ``http://.../viewA?$orderby=id,fieldname%2Cwith%2Ccommas``

If you disable the option *Process functions in $select parameter* of the service (tab *Advanced* of the service configuration), this rule also applies to the parameter $select.

You can configure all REST web services to decode the value of these parameters before processing them. That way, the service will convert the "%2C" back to "," before processing these values. To enable this option, follow these steps:

#. Log into Denodo with an administrator user
#. Execute this command from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.wsgenerator.restws.decodeQueryParametersBeforeProcessing'='true';

3. Redeploy the web service.

   All the web services deployed after changing this property will decode the values before processing them. You do not need to restart the Virtual DataPort server to apply the changes to this property.

To go back to the previous behavior, execute the following and redeploy the REST web services:

.. code-block:: vql

   SET 'com.denodo.wsgenerator.restws.decodeQueryParametersBeforeProcessing'='false';

The drawback with this option is that you will not be able to use, in these parameters, a field name whose name has the character ",".

Generally, you do not need to enable this option. However, it is necessary if the client applications always send the character "," encoded with "%2C".

Tunnel HTTP Methods inside another HTTP Method (X-HTTP-Method-Override)
=======================================================================

The RESTful web service and published REST web services support the HTTP header ``X-HTTP-Method-Override``. This header allows applications to "tunnel" other HTTP methods inside POST requests. For example, a POST request with the header ``X-HTTP-Method-Override: GET`` will be processed as a GET request. In addition, you can send the query parameters in the body of the request instead of in the query of the URL.

This is useful when:

1. There is a firewall between the client application and the Denodo server, and the firewall only allows the methods GET and POST, but not PUT nor DELETE.

#. The application needs to send extremely long input parameters that result in the URL being longer than the maximum length of an HTTP request. In this case, the application can send a POST request with this header and put the input parameters in the body of the request, instead of in the URL.

You can tunnel the following methods inside a POST request: GET, PUT and DELETE.

If a request contains the header X-HTTP-Method-Override and the request is not POST, the header is ignored and the request is processed as it is.

When you tunnel a PUT request inside a POST request, the request is processed as a PUT. In this case, the client has to send the same body as if it was sending a PUT request.

When you tunnel a GET or DELETE request inside a POST request, you can put the parameters in the body of the request instead of in the URL. The parameters you can put in the body are the same you can put in the URL: field names, $filter, $select, $orderby, etc. They can be put in an XML document, a JSON or an HTML form, and the request has to have the header ``Content-Type``. This header is only used to process the body of the request; the output is still determined by the ``Accept`` header or the parameter ``$format`` of the URL.

.. rubric:: Sending Input Parameters in an XML Document

To send the input parameters in the body of the request, as an XML document, add the HTTP header ``Content-Type: application/xml``. We recommend setting the encoding of the input so the non-ASCII characters are processed correctly. E.g. ``application/xml;charset=UTF-8``.

The format of the XML has to be:

.. code-block:: xml

   <request>
      <parameter name="name of parameter 1">value of parameter 1</parameter>
      <parameter name="name of parameter 2"> value of parameter 2</parameter>
      <!-- ... -->
      <!-- ... -->
   </request>

For example:

.. code-block:: http
   :emphasize-lines: 2,3

   POST /server/customer360/customer HTTP/1.1
   X-HTTP-Method-Override: GET
   Content-Type: application/xml;charset=UTF-8
   Accept: application/xml

   <request>
      <parameter name="country">Mexico<parameter>
      <parameter name="$select">client_id,name,company_name</parameter>
      <parameter name="$orderby">client_id</parameter>
   </request>

This is equivalent to sending a GET request to 

   /server/customer360/customer?country=Mexico&$select=client_id,name,company_name&order_by=client_id

The service will return the information about customers in Mexico, sorted by client_id.

.. rubric:: Sending Input Parameters in a JSON Document

To send the input parameters in the body of the request, in a JSON document, add the HTTP header ``Content-Type: application/json``. We recommend setting the encoding of the input so the non-ASCII characters are processed correctly. E.g. ``application/json;charset=UTF-8``.

The format of the JSON has to be:

.. code-block:: json

   {
        "name of parameter 1": "value of parameter 1"
      , "name of parameter 2": "value of parameter 2"
   }

For example:

.. code-block:: http
   :emphasize-lines: 2,3

   POST /server/customer360/customer HTTP/1.1
   X-HTTP-Method-Override: GET
   Content-Type: application/json;charset=UTF-8
   Accept: application/json

   {
        "country": "Mexico"
      , "$select": "client_id,name,company_name"
      , "$orderby": "client_id"
   }

This is equivalent to sending a request to 
   
   /server/customer360/customer?country=Mexico&$select=client_id,name,company_name&order_by=client_id

.. rubric:: Sending Input Parameters in an HTML Form

To send the input parameters in the body of the request, in an HTML form, add the HTTP header ``Content-Type: application/x-www-form-urlencoded``.  We recommend setting the encoding of the input so the non-ASCII characters are processed correctly. E.g. ``application/x-www-form-urlencoded;charset=UTF-8``.

The values are encoded in key-value tuples separated by "&", with a "=" between the key and the value. Non-alphanumeric characters are percent encoded. E.g. if a value has the character "%", put "%25".

For example:

.. code-block:: http
   :emphasize-lines: 2,3

   POST /server/customer360/customer HTTP/1.1
   X-HTTP-Method-Override: GET
   Content-Type: application/x-www-form-urlencoded;charset=UTF-8
   Accept: text/html

   state=New%20Jersey&$select=client_id,name,company_name&$orderby=client_id

This is equivalent to sending a request to 
   
   /server/customer360/customer?country=New%20Jersey&$select=client_id,name,company_name&order_by=client_id
   
Because of the header Accept, the service will return the HTML representation of the data.