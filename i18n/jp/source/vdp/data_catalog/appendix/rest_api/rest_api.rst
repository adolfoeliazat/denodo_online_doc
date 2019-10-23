========
REST API
========

The Data Catalog provides a REST API to perform programmatically the following tasks:

.. contents::
   :depth: 1
   :local:
   :backlinks: none

Consider this:

-  The requests and responses of this API are in JSON format.
-  All the operations are stateless. That is, the client applications have to send the user credentials on every request.

.. _dc_api_login:

.. rubric:: Authentication with the REST API of the Data Catalog

The REST API of the Data Catalog supports two authentication methods:

#. HTTP Basic
#. Kerberos

For Kerberos authentication, add the header:

.. code-block:: none

   Authorization: <Kerberos token encoded in base 64>

If the user credentials included in the request are incorrect, the API returns the HTTP code 403 (Forbidden).

.. _dc_api_synchronize_elements:

Synchronize Elements with the Server
====================================

Operation to :ref:`synchronize <data-catalog-synchronize-elements-with-the-server>` the metadata defined in the Data Catalog with the metadata from the Virtual DataPort server:

-  URL: ``/apirest/synchronize``
-  Method: ``POST``
-  Header: ``Content-Type: application/json``
-  Syntax of the body of the request:

   .. code-block:: bnf	

      {
          { "allServers": "true" | "serverName": "<name of the Virtual DataPort server>" }
          , "type": { "all" | "view" | "ws" }
          , "priority": {"server_with_local_changes" | "local" | "server" }
      }

   The body of the request is a JSON document with these elements:
   
   -  ``"allServers": "true"`` or ``serverName``: with ``allServer``, the Data Catalog synchronizes its metadata with the metadata of all the Virtual DataPort servers registered on the Data Catalog. With ``serverName``, it synchronizes its metadata with the metadata of that server.
   -  ``type`` (optional): elements whose metadata will be synchronized. The possible values are:

      1. ``all`` (default value): web services and views
      #. ``view``: only views
      #. ``ws``: only web services
   
   -  ``priority`` (optional): decision in case of conflicts. That is, action to do if the metadata of an element (e.g. the description of a view) has changed in Virtual DataPort and in the Data Catalog since the last time the metadata was synchronized. The possible values are:
   
      1. ``server_with_local_changes`` (default value): Virtual DataPort server but keeping the changes in the Data Catalog.
      2. ``local``: changes in the Data Catalog have a priority over changes in Virtual DataPort.
      3. ``server``: changes in Virtual DataPort override changes on the Data Catalog.

If the request succeeds, the operation returns the HTTP code 200 and the body of the response contains a JSON document with the list of elements (views and/or web services) that have been synchronized (i.e. inserted, removed and modified).

.. code-block:: JavaScript
   :caption: Format of the response of the operation "synchronize"

    {
        "result": "OK",
        "data": {
            "view": {
                "inserted": [ <list_of_inserted_views> ],
                "removed": [ <list_of_removed_views> ],
                "modified": [ <list_of_modified_views> ]
            },
            "ws": {
                "inserted": [ <list_of_inserted_web_services> ],
                "removed": [ <list_of_removed_web_services> ],
                "modified": [ <list_of_modified_web_services> ]
            }
        },
        "message": "OK"
    }


.. rubric:: Example

This example uses the tool "cURL" to send a request to synchronize the metadata:

.. code-block:: bash
   :caption: Example of a request to the operation "synchronize" using "cURL"

   curl --request POST --user "jsmith:my_password" --header "Content-Type: application/json" --data '{ "allServers": "true", "priority": "server_with_local_changes" }' "https://denodo-server.acme.com:9090/denodo-data-catalog/apirest/synchronize"

Consider this:

-  The element ``"allServers": "true"`` of the body of the request indicates the Data Catalog to synchronize the metadata of all the Virtual DataPort servers. This is the most common value since usually you will only have one Virtual DataPort registered on the Data Catalog.

-  The element ``"priority": "server_with_local_changes"`` of the body of the request indicates the Data Catalog what to do in case of conflict.

The API returns a JSON document like this:

.. code-block:: json
   :caption: Example of a response of the operation "synchronize"
	
   {
       "result": "OK",
       "data": {
           "view": {
               "inserted": ["customer360.invoice", "customer360.invoice_line"],
               "removed": [],
               "modified": []
           },
           "ws": {
               "inserted": [],
               "removed": [],
               "modified": []
           }
       },
       "message": "OK"
   }

In this example, Virtual DataPort contains two new views: "invoice" and "invoice_line" of the database "customer360".
	
.. ESTO ESTA COMENTADO HASTA QUE SE CONVIERTA EN GET        
    .. dc_api_get_statistics:

    Get the Usage Statistics
    ------------------------

    If you execute a POST request to the following URL you get the :ref:`usage statistics <Usage of Views>` of the requested element:

    - URL: ``/apirest/getting-statistics``
    - Method: ``POST``

    The request body must include the server name, the database, the element name and its type, according to the following format:

    .. code-block:: JavaScript	

       {
            "serverName": <text>,
            "database": <text>,
            "elementName": <text>,
            "elementType": <text>
       }
       
    The parameter ``elementType`` can be: ``view``, ``rest`` or ``soap``. The last two ones refer to web services.

    The next example shows a POST request to get the usage statistics of the view "internet_inc" located in the database "admin":

    .. code-block:: JavaScript

       {
            "serverName": "localhost",
            "database": "admin",
            "elementName": "internet_inc",
            "elementType": "view"
       }

    The response body contains its usage statistics, grouped by period (*last day*, *last month* and *all time*):

    .. code-block:: JavaScript

        {
            "result": "OK",
            "data": {
                "LAST_DAY": {
                    "statisticsId": 83,
                    "usagePeriod": "LAST_DAY",
                    "element": "\"admin\".\"internet_inc\"",
                    "elementType": "VIEW",
                    "numberExecutions": 14,
                    "averageTime": 133.14285,
                    "averageResults": 4,
                    "numberUsers": 1,
                    "numberUserAgents": 2,
                    "moreExecutedQueries": [
                        {
                            "first": "SELECT_NAVIGATIONAL iinc_id, summary, ttime, taxid, specific_field1, specific_field2 FROM admin.internet_inc WITH CONDITION MAPPINGS EVALUATION OFFSET 0 LIMIT 25 context ('i18n' = 'us_pst' ) trace",
                            "second": 11
                        },
                        {
                            "first": "SELECT_NAVIGATIONAL iinc_id, summary, ttime, taxid, specific_field1, specific_field2 FROM admin.internet_inc WITH CONDITION MAPPINGS EVALUATION  OFFSET 0 LIMIT 25 context ('i18n' = 'us_pst' )",
                            "second": 2
                        },
                        {
                            "first": "SELECT * FROM internet_inc CONTEXT ('i18n'='es_euro', 'cache_wait_for_load'='true') TRACE",
                            "second": 1
                        }
                    ],
                    "moreFrequentUsers": [
                        {
                            "first": "admin",
                            "second": 14
                        }
                    ],
                    "moreFrequentUserAgents": [
                        {
                            "first": "Denodo-VDP-AdminTool",
                            "second": 12
                        },
                        {
                            "first": "Denodo-Data-Catalog",
                            "second": 2
                        }
                    ]
                },
                "LAST_MONTH": {
                    "statisticsId": 90,
                    "usagePeriod": "LAST_MONTH",
                    "element": "\"admin\".\"internet_inc\"",
                    "elementType": "VIEW",
                    "numberExecutions": 14,
                    "averageTime": 133.14285,
                    "averageResults": 4,
                    "numberUsers": 1,
                    "numberUserAgents": 2,
                    "moreExecutedQueries": [
                        {
                            "first": "SELECT_NAVIGATIONAL iinc_id, summary, ttime, taxid, specific_field1, specific_field2 FROM admin.internet_inc WITH CONDITION MAPPINGS EVALUATION OFFSET 0 LIMIT 25 context ('i18n' = 'us_pst' ) trace",
                            "second": 11
                        },
                        {
                            "first": "SELECT_NAVIGATIONAL iinc_id, summary, ttime, taxid, specific_field1, specific_field2 FROM admin.internet_inc WITH CONDITION MAPPINGS EVALUATION  OFFSET 0 LIMIT 25 context ('i18n' = 'us_pst' )",
                            "second": 2
                        },
                        {
                            "first": "SELECT * FROM internet_inc CONTEXT ('i18n'='es_euro', 'cache_wait_for_load'='true') TRACE",
                            "second": 1
                        }
                    ],
                    "moreFrequentUsers": [
                        {
                            "first": "admin",
                            "second": 14
                        }
                    ],
                    "moreFrequentUserAgents": [
                        {
                            "first": "Denodo-VDP-AdminTool",
                            "second": 12
                        },
                        {
                            "first": "Denodo-Data-Catalog",
                            "second": 2
                        }
                    ]
                },
                "ALL_TIME": {
                    "statisticsId": 97,
                    "usagePeriod": "ALL_TIME",
                    "element": "\"admin\".\"internet_inc\"",
                    "elementType": "VIEW",
                    "numberExecutions": 14,
                    "averageTime": 133.14285,
                    "averageResults": 4,
                    "numberUsers": 1,
                    "numberUserAgents": 2,
                    "moreExecutedQueries": [
                        {
                            "first": "SELECT_NAVIGATIONAL iinc_id, summary, ttime, taxid, specific_field1, specific_field2 FROM admin.internet_inc WITH CONDITION MAPPINGS EVALUATION OFFSET 0 LIMIT 25 context ('i18n' = 'us_pst' ) trace",
                            "second": 11
                        },
                        {
                            "first": "SELECT_NAVIGATIONAL iinc_id, summary, ttime, taxid, specific_field1, specific_field2 FROM admin.internet_inc WITH CONDITION MAPPINGS EVALUATION  OFFSET 0 LIMIT 25 context ('i18n' = 'us_pst' )",
                            "second": 2
                        },
                        {
                            "first": "SELECT * FROM internet_inc CONTEXT ('i18n'='es_euro', 'cache_wait_for_load'='true') TRACE",
                            "second": 1
                        }
                    ],
                    "moreFrequentUsers": [
                        {
                            "first": "admin",
                            "second": 14
                        }
                    ],
                    "moreFrequentUserAgents": [
                        {
                            "first": "Denodo-VDP-AdminTool",
                            "second": 12
                        },
                        {
                            "first": "Denodo-Data-Catalog",
                            "second": 2
                        }
                    ]
                }
            },
            "message": null
        }


.. _dc_api_compute_statistics:

Compute the Usage Statistics
============================

Operation to :ref:`compute and update the usage statistics <Computing usage statistics>` of the Data Catalog.

-  URL: ``/apirest/fetching-statistics``
-  Method: ``POST``
-  Header: ``Content-Type: application/json``
-  Syntax of the body of the request:

   .. code-block:: JavaScript	

      {
          "serverName": <text> | "allServers": "true"
      }

If the request succeeds, the operation returns the HTTP code 200 and the body of the response contains information about this operation, for each Virtual DataPort server registered in the Data Catalog.

.. rubric:: Example

This example uses the tool "cURL" to update the usage statistics of the Data Catalog:

.. code-block:: bash
   :caption: Example of a request to the operation "fetching-statistics" using "cURL"

   curl --request POST --user "jsmith:my_password" --data  '{ "allServers": true }' --header "Content-Type: application/json" "https://denodo-server.acme.com:9090/denodo-data-catalog/apirest/fetching-statistics"

.. code-block:: json
   :caption: Example of a response of the operation "fetching-statistics"

    {
        "result": "OK",
        "data": {
            "responses": [
                {
                    "serverName": "localhost",
                    "serverUrl": "//localhost:9999/admin",
                    "httpStatusCode": 200,
                    "jsonResponse": {
                        "result": "OK",
                        "data": null,
                        "message": null
                    }
                },
                {
                    "serverName": "remote",
                    "serverUrl": "//remoteHost:19999/admin",
                    "httpStatusCode": 500,
                    "jsonResponse": {
                        "result": "ERROR",
                        "data": null,
                        "message": "None of periods was chosen for statistics"
                    }
                }
            ]
        },
        "message": null
    }

In this example, the statistics from server "localhost" were successfully computed but there was an error when doing the same for the server "localhost2".

.. _data-catalog-rest-api-export-metadata:

Export Metadata
===============

Operation to export the Data Catalog’s configuration and metadata (categories, tags, saved queries, etc.) as a collection of JSON files compressed in a single zip file.

-  URL: ``/apirest/admin/export``
-  Method: ``POST``
-  URL parameters (all of them are optional and can be combined):

   -  ``contentSearch=on``:  add it to export Content Search settings. 
   -  ``personalization=on``: add it to export Personalization settings.
   -  ``kerberos=on``: add it to export Kerberos settings.
   -  ``serverName``: Virtual DataPort server. You have to specify this if there is more than one server registered with the Data Catalog. Otherwise, it is optional.

If the request succeeds, the operation returns the HTTP code 200 and a zip file with the result.

.. rubric:: Example

This example uses the tool "cURL" to export the Content Search settings and the Personalization settings:

.. code-block:: bash
   :caption: Example of a request to the operation "export" using "cURL"

   curl --request POST --user "jsmith:my_password" --output "data-catalog-metadata.zip" "https://denodo-server.acme.com:9090/denodo-data-catalog/apirest/admin/export?contentSearch=on&personalization=on"


.. _data-catalog-rest-api-import-metadata:

Import Metadata
===============

Operation to import the Data Catalog’s configuration and metadata (categories, tags, saved queries, etc.). The input is a zip file with the files like the ones generated by the operation "export".

A zip file exported from the web interface (Administration / Export menu) can be loaded from this REST API service.

- URL: ``/apirest/admin/import``
- Method: ``POST``
- Content-Type: ``multipart/form-data``
- URL parameters (all of them are optional and can be combined):

  -  ``override=true``: add it to overwrite the existing metadata and configuration parameters.
  -  ``serverName``: Virtual DataPort server. You have to specify this if there is more than one server registered with 
     the Data Catalog. Otherwise, it is optional.
    
-  Body of the request. Form-data parameter:
  
   -  ``file``: zip file to import. The format of the files inside the zip file has to be same as the one obtained from the operation "export". 

If the request succeeds, the operation returns the HTTP code 200 and a JSON document with the result.

.. rubric:: Example

This example uses the tool "cURL" to import settings of the Data Catalog. In this example, the name of the local file is "data-catalog-metadata.zip" but it can be any name.

.. code-block:: bash
   :caption: Example of a request to the operation "import" using "cURL"

   curl --user "jsmith:my_password" --form file=@"data-catalog-metadata.zip;type=application/zip" "https://denodo-server.acme.com:9090/denodo-data-catalog/apirest/admin/import"

.. code-block:: json
   :caption: Example of a response of the operation "export"

    {
        "Result": "OK",
        "Object": null,
        "Message": "Metadata successfully imported",
        "Code": null
    }
    
 