============
IDU Requests
============

Besides providing different representations of the data of the views,
the RESTful Web service can execute Insert, Delete and Update (IDU)
operations.

Note that to execute IDU operations over a view, the view has to be
“updateable”. The section :ref:`Inserts, Updates and Deletes over Views` of
the VQL Guide explains when a view is “updateable”.

Following the RESTful conventions, the statement sent to Virtual
DataPort by the Service, depends on the HTTP method of the request (see
:ref:`RESTful Web service: HTTP methods and their equivalent VQL
statements`).

The ``POST`` (``INSERT``) and ``PUT`` (``UPDATE``) requests have to
contain the data that you want to insert in the view, in the body of the
request. You can represent these data as a JSON or an XML document.
Depending on the type of document, the HTTP Header ``Content-Type`` 
has to be either ``application/json`` or ``application/xml``.

The XML document sent in the body of the request has to meet the
following conditions:

-  The name of the root element has to be the name of the view
-  If you want to set the value of a field to ``NULL``, you have to
   define the ``xmlns:xsi=http://www.w3.org/2001/XMLSchema-instance``
   namespace and add the attribute ``xsi:nil=true`` to the field (see
   `RESTful Web service: POST request with XML body`_)

In both types of documents, the values of type ``date`` have to be
expressed with the following format: ``yyyy-MM-dd'T'hh:mm:ssZ``. E.g.
"``2005-06-29T17:19:41+0000``".

To execute a ``PUT`` (``UPDATE``) or ``DELETE`` request over a view, the
view must have its primary key defined. That is because the row to
update/delete is identified by the values of its primary key.



INSERT Queries (POST)
=================================================================================

To execute an ``INSERT`` query over a view, you have to send a ``POST``
request to a URL that represents a view.

The syntax of the URL to execute ``INSERT`` queries is
``http://localhost:9090/denodo-restfulws/<database name>/views/<view name>``

E.g.
``http://localhost:9090/denodo-restfulws/admin/views/internet_inc``.

If the Service processes the request correctly, it returns the HTTP code
201 (“Created”). If the target view has primary key, the response will
also include the HTTP header “Location” with an URL that points to the
new row.

|

Below, there are two equivalent examples of a ``POST`` (``INSERT``)
request. The first one sends the data in an XML document and the second
one, in a JSON document.

**Example 1 (XML)**

URL = ``http://localhost:9090/denodo-restfulws/admin/views/internet_inc``

HTTP Method = ``POST``

HTTP Header ``Accept`` = ``application/xml``

HTTP Header ``Content-Type`` = ``application/xml``

Body of the request:

.. code-block:: xml
   :name: RESTful Web service: POST request with XML body
   :caption: RESTful Web service: POST request with XML body

   <?xml version="1.0" encoding="UTF-8"?>
   <internet_inc xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
       <iinc_id>2000</iinc_id>
       <summary>New incidence</summary>
       <taxid>B99999999</taxid>
       <specific_field1 xsi:nil="true" />
       <specific_field2 xsi:nil="true" />
   </internet_inc>

By defining the namespace ``http://www.w3.org/2001/XMLSchema-instance``
and adding the attribute ``xsi:nil="true"`` to the ``specific_field1``
and ``specific_field2`` fields, their values will be ``NULL``.

Also, note that the name of the root element is the name of the view we
want to execute the ``INSERT`` query on.

**Example 2 (JSON)**

URL = ``http://localhost:9090/denodo-restfulws/admin/views/internet_inc``

HTTP Method = ``POST``

HTTP Header ``Accept`` = ``application/json``

HTTP Header ``Content-Type`` = ``application/json``

Body of the request:

.. code-block:: json
   :name: RESTful Web service: POST request with JSON body
   :caption: RESTful Web service: POST request with JSON body

   { 
       "iinc_id": 2000
     , "summary": "New incidence"
     , "taxid": "B99999999"
     , "specific_field1": null
     , "specific_field2": null
   }

In both examples, when the Service receives this request, it executes
the following query:

.. code-block:: sql

   INSERT INTO internet_inc SET 
         summary = 'New incidence' 
       , iinc_id = 2000
       , specific_field1 = null
       , specific_field2 = null
       , taxid = 'B99999999';


UPDATE Queries (PUT)
=================================================================================

To execute an ``UPDATE`` query over a view, you have to send a ``PUT``
request to a URL that represents a row of view (not a view).

The syntax of the URL to execute ``UPDATE`` queries is
\http://localhost:9090/denodo-restfulws/<database name>/views/<view name>/<primary key values>

E.g. \http://localhost:9090/denodo-restfulws/admin/views/internet_inc/12

The final segment of the URL (``12``) is the value of the primary key,
which in the view ``internet_inc`` is the field ``IINC_ID``.

If the primary key is formed by two or more fields, you have to separate the
value of each field with a comma (``,``).

If any of the values of the primary key contains a ",", encode it
with ``%2C`` to avoid interpreting it as the separator of primary
key values.

If the Service processes the request correctly, it returns the HTTP code
204 (“No content”).

As the Service uses the value of the primary key to identify the row
that you want to update, the views without primary key cannot be updated
from the RESTful Web service.

|

Below, there two equivalent examples of a ``PUT`` (``UPDATE``) request.
The first one sends the data in an XML document and the second one, in a
JSON document.

**Example 1 (XML)**

URL = ``http://localhost:9090/denodo-restfulws/admin/views/internet_inc/2``

HTTP Method = ``PUT``

HTTP Header ``Accept`` = ``application/xml``

HTTP Header ``Content-Type`` = ``application/xml``

Body of the request:

.. code-block:: xml
   :caption: RESTful Web service: PUT request with XML body
   :name: RESTful Web service: PUT request with XML body
   
   <?xml version="1.0" encoding="UTF-8"?>
   <internet_inc xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
       <summary>Incident in ADSL router ...</summary>
   </internet_inc>


**Example 2 (JSON)**

URL = ``http://localhost:9090/denodo-restfulws/admin/views/internet_inc/2``

HTTP Method = ``PUT``

HTTP Header ``Accept`` = ``application/json``

HTTP Header ``Content-Type`` = ``application/json``

Body of the request:

.. code-block:: json
   :caption: RESTful Web service: PUT request with JSON body
   :name: RESTful Web service: PUT request with JSON body

   { 
     "summary": "Incident in ADSL router ..."
   }

In both examples, when the Service receives this request, it executes
the following query:

.. code-block:: sql

   UPDATE internet_inc SET 
       summary = 'Incident in ADSL router ...'  
   WHERE iinc_id = 2

The ``WHERE`` clause is formed by the field of the primary key
(``IINC_ID``) and the value of the last segment of the URL (``2``).


DELETE Queries
=================================================================================

To execute a ``DELETE`` query over a view, you have to send a ``DELETE``
request to a URL that represents a row of view (not a view).

The syntax of the URL to execute ``DELETE`` queries is
\http://localhost:9090/denodo-restfulws/<database name>/views/<view name>/<primary key values>

E.g. \http://localhost:9090/denodo-restfulws/admin/views/internet_inc/12.

In this example, the final segment of the URL (``12``) is the value of
the primary key, which in the view ``internet_inc`` is formed by the
field ``IINC_ID``. As we have seen in the previous section, if the
primary key is formed by more than one field, you have to put the values
of each field separated by comma (``,``). Therefore, if any of the
primary key values has this character, you have to replace it with
``%2C``.

If the Service processes the request correctly, it returns the HTTP code
204 (“No content”).

As the Service uses the value of the primary key to identify the row
that you want to delete, the views without primary key cannot be deleted
from the RESTful Web service.

The difference with the ``DELETE`` requests and the ``POST`` and ``PUT``
ones, is that the body of the request is empty and you do not need to
add the HTTP headers ``Accept`` nor ``Content-Type``.
