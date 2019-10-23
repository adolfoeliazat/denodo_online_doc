===================
RESTful Web Service
===================

.. toctree::
   :hidden:
   
   representations.rst
   input_parameters_of_the_restful_web_service.rst
   idu_requests.rst
   requests_with_input_compound_values.rst
   status_codes.rst
   obtaining_the_number_of_rows_of_a_result_set.rst
   security_and_access_privileges.rst
   configuring_the_restful_web_service.rst  
   

The RESTful Web service of the Denodo Platform is an HTTP service that
publishes the contents of the entire Virtual DataPort server.

Unless explicitly mentioned in the documentation, you can use the RESTful web service in the same way as published REST web services: the input parameters are the same and the output has the same format.

The output of this service can be an XML document, a JSON document or a
more user-friendly HTML document.

This web service is deployed in the URL
\http://<host of the Denodo server>:9090/denodo-restfulws and exposes the following types
of resources:

.. table:: Types of resources exposed by the RESTful Web service
   :name: Types of resources exposed by the RESTful Web service

   +---------------+-------------------------+---------------------------------------------------------------+
   | Type of       | URI Format              | Representation                                                |
   | Resource      |                         |                                                               |
   +===============+=========================+===============================================================+
   | Databases     | /<database              | Shows the database                                            |
   |               | name>                   | description and the                                           |
   |               |                         | list of views in the                                          |
   |               |                         | database, including                                           |
   |               |                         | links to access their                                         |
   |               |                         | schema and their                                              |
   |               |                         | elements.                                                     |
   +---------------+-------------------------+---------------------------------------------------------------+
   | Views         | /<database              | Shows the metadata of                                         |
   |               | name>/views/<view name> | the view and lists the                                        |
   |               |                         | elements of the view.                                         |
   +---------------+-------------------------+---------------------------------------------------------------+
   | Row of        | /<database              | Shows the data of an                                          |
   | a view        | name>/views/<view name> | individual row of a                                           |
   |               | /<pk field 1>,          | view, *if the view has                                        |
   |               | <pk field 2>            | its primary key                                               |
   |               |                         | defined*.                                                     |
   +---------------+-------------------------+---------------------------------------------------------------+
   | Number        | /<database              | Returns the number of                                         |
   | of rows       | name>/views/<view name> | rows of the view.                                             |
   | of a of a     | /$count                 | It can be used in                                             |
   | result        |                         | combination with filters                                      |
   | set           |                         | by field and/or the                                           |
   |               |                         | parameter $filter.                                            |
   |               |                         |                                                               |
   |               |                         | See more about                                                |
   |               |                         | obtaining the number                                          |
   |               |                         | of rows of a result                                           |
   |               |                         | set in the section                                            |
   |               |                         | :ref:`Obtaining the                                           |
   |               |                         | Number of Rows of                                             |
   |               |                         | a Result Set`                                                 |
   +---------------+-------------------------+---------------------------------------------------------------+
   | Metadata      | /<database name>/views  | Returns the metadata of the view.                             |
   | of a view     | /<view name>/$metadata  | This includes the description of the                          |
   |               |                         | view and the URLs to                                          |
   |               |                         | associations.                                                 |
   |               |                         | retrieve its schema and its                                   |
   |               |                         |                                                               |
   |               |                         | This URL is only supported in the XML                         |
   |               |                         | and JSON representations, not the HTML                        |
   |               |                         | one. This means that to obtain the                            |
   |               |                         | to send the appropriate ``Accept``                            |
   |               |                         | metadata of a view, you either have                           |
   |               |                         | header or add ``?$format=xml`` or                             |
   |               |                         | ``?$format=json`` to the URL.                                 |
   +---------------+-------------------------+---------------------------------------------------------------+
   | Schema of     | /<database name>/views  | Returns the schema of the view. This                          |
   | a view        | /<view name>/$schema    | a list of the fields and types of the                         | 
   |               |                         | view.                                                         |
   |               |                         |                                                               |
   |               |                         | This URL is only supported in the XML                         |
   |               |                         | and JSON representations, not the HTML                        |
   |               |                         | one. This means that to obtain the                            |
   |               |                         | schema, you either have                                       |
   |               |                         | to send the appropriate ``Accept``                            |
   |               |                         | header or add ``?$format=xml`` or                             |
   |               |                         | ``?$format=json`` to the URL.                                 |
   |               |                         |                                                               |
   |               |                         | When you request the schema of a                              |
   |               |                         | view of a REST web service, not the                           |
   |               |                         | RESTful web service, consider the                             |
   |               |                         | following:                                                    |
   |               |                         |                                                               |
   |               |                         | -  REST services only return the                              | 
   |               |                         |    fields that are published; in the                          |
   |               |                         |    configuration of the service, you                          |
   |               |                         |    may have removed some of the fields                        | 
   |               |                         |    of the view.                                               |
   |               |                         | -  REST services return the names of                          |
   |               |                         |    the fields as they appear in the                           |
   |               |                         |    web service, not the name of the                           | 
   |               |                         |    fields in the view; in the                                 |
   |               |                         |    configuration of the service, you                          |
   |               |                         |    may have renamed some fields.                              |
   +---------------+-------------------------+---------------------------------------------------------------+
   | Schema of     | /<database name>/views  | Returns the schema of the view in XSD                         |
   | a view (XSD)  | /<view name>/$xsd       | format. This is equivalent to                                 | 
   |               |                         | ``/<database name>/views/<view name>/$schema?$format=xml``.   |
   |               |                         |                                                               |
   |               |                         | This URL is only supported in the XML                         |
   |               |                         | representation.                                               |
   +---------------+-------------------------+---------------------------------------------------------------+
   | Associations  |/<database name>/views   | Returns the associations of the view.                         |
   |               |/<view                   |                                                               |
   |               |name>/$associations      | This URL is only supported in the XML                         |
   |               |                         | and JSON representations, not the HTML                        |
   |               |                         | one. This means that to obtain the                            |
   |               |                         | associations, you either have                                 |
   |               |                         | to send the appropriate ``Accept``                            |
   |               |                         | header or add ``?$format=xml`` or                             |
   |               |                         | ``?$format=json`` to the URL.                                 |
   |               |                         |                                                               |
   |               |                         | When you request the associations of a                        |
   |               |                         | view of a REST web service, not the                           |
   |               |                         | RESTful web service, the service                              |
   |               |                         | will only return the associations that                        | 
   |               |                         | are published with the service, not                           |
   |               |                         | all the associations of the view.                             |
   +---------------+-------------------------+---------------------------------------------------------------+
   | OpenAPI 2 /   |/OpenAPI/swagger.json    | **This is only available for REST web                         |
   | Swagger       |/OpenAPI/swagger.yaml    | services, not the RESTful web                                 |
   | specifications|                         | service**                                                     |
   |               |                         |                                                               |
   |               |                         | Returns the `OpenAPI 2 / Swagger`_                            |
   |               |                         | specification for a published REST web service in JSON or YAML|
   |               |                         | format. The specification is based on the views               |
   |               |                         | published by the web service and describes the available      |
   |               |                         | operations, the parameters that are available for each one and|
   |               |                         | their input and output schemas.                               |   
   |               |                         |                                                               |
   |               |                         | You can load the generated specifications in                  |
   |               |                         | `<https://editor.swagger.io>`_. Here you will be              |
   |               |                         | able to visualize the specification's details, generate       |
   |               |                         | client code automatically and obtain example                  |
   |               |                         | requests for accessing the web service.                       |
   +---------------+-------------------------+---------------------------------------------------------------+   
  
.. rubric:: Examples of how to invoke the RESTful Web service

-  ``http://localhost:9090/denodo-restfulws/admin`` returns the list of
   views of the database ``admin``.
-  ``http://localhost:9090/denodo-restfulws/admin/views/customer``
   returns the content of the view ``customer`` of the database
   ``admin``.
-  ``http://localhost:9090/denodo-restfulws/admin/views/customer/1/orders``
   browses through the mapping ``orders`` of the row of the table
   ``customer`` whose primary key is ``1``.

|

.. rubric:: Additional notes about the URI format of the RESTful Web service

-  URLs are case-sensitive. This means that a view ``TestView`` must be
   invoked with ``http://.../views/TestView``, and not
   ``http://.../admin/views/testview``.
-  If the value of the primary key contains the character ``,``,
   replace it with ``%2C``. This is because the character
   ``,`` separates the values of the primary key with more than
   one field. E.g.
   ``http://.../support/views/viewA/1,value%2Cwith%2Ccommas``.

   
-  String literals must not be quoted, except in the ``$filter``
   parameter (see section :ref:`Input Parameters of the RESTful Web service` to
   see additional information about the parameters of the RESTful Web
   service).

Following the RESTful conventions, the statement sent to Virtual
DataPort by the RESTful Web service, depends on the HTTP method of the
request.

The table `RESTful Web service: HTTP methods and their equivalent VQL
statements`_ displays the type of query sent by the Service depending on
the HTTP method of the request and the syntax of the URL depending on
the type. For the GET requests, the syntax can vary a lot and is
explained in the following section.

 
.. table:: RESTful Web service: HTTP methods and their equivalent VQL statements
   :name: RESTful Web service: HTTP methods and their equivalent VQL statements

   +---------------+-------------+-----------------------------------------------+
   | VQL Statement | HTTP Method | Syntax of the Request URL                     |
   +===============+=============+===============================================+
   | SELECT        | GET         | \http://localhost:9090/denodo-restfulws/\     |
   +---------------+-------------+-----------------------------------------------+
   | INSERT        | POST        | \http://localhost:9090/denodo-restfulws/\     |
   |               |             | <database name>/views/<view name>             |
   +---------------+-------------+-----------------------------------------------+
   | UPDATE        | PUT         | \http://localhost:9090/denodo-restfulws/\     |
   |               |             | <database name>/views/<view name>/\           |
   |               |             | <primary key value>                           |
   +---------------+-------------+-----------------------------------------------+
   | DELETE        | DELETE      | \http://localhost:9090/denodo-restfulws/\     |
   |               |             | <database>/views/<view name>/\                |
   |               |             | <primary key value>                           |
   +---------------+-------------+-----------------------------------------------+

   
.. _OpenAPI 2 / Swagger: https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md