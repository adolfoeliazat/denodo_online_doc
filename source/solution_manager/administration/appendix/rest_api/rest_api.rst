========
REST API
========

The Solution Manager provides a REST API to perform programmatically certain tasks. This is useful to automate the creating and deployment of revisions, automate the management of the licenses, etc.

Considerations to use this API:

-  The base URL for all the operations (except ``externalShutdown`` and ``pingLicenseManager``) is this:

   .. code-block:: none 

      https://<host of the Solution Manager server>:<Solution Manager server port>

   The default port of the Solution Manager is 10090.

-  The requests have to include the HTTP header ``Cookie: JSESSIONID=...``. For example:

   .. code-block:: none
   
      "Cookie: JSESSIONID=A186108C11FFC545FAFF892F3A71F589"
      
   Obtain the ``JSESSIONID`` with the operation :ref:`login <Creating a Session with the Solution Manager>`.

The operations to register and deregister servers from the Solution Manager can be used in combination with the auto scaling capabilities of your cloud provider, to start and stop Denodo servers when necessary. The article `Configuring Auto Scaling of Denodo in AWS <https://community.denodo.com/kb/view/document/Configuring%20Autoscaling%20of%20Denodo%20in%20AWS?category=Operation>`_ of the Knowledge Base explains how to do this.

Lists of operations available:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols


.. _sm_api_login:

Creating a Session with the Solution Manager
============================================

To use any operation of the API of the Solution Manager, first you need to send a request to the operation ``login``. This operation will return a header ``JSESSIONID``, which you have to include this value as an HTTP header of all the requests.

-  URL: ``login``
-  Method: ``GET``

This operation supports two authentication methods: 

#. HTTP Basic
#. Kerberos

For Kerberos authentication, add the header to the following request:

.. code-block:: none

   Authorization: <Kerberos token encoded in base 64>

If the credentials are correct, this operations returns this:

-  The HTTP code 200.
-  The header ``Set-Cookie``. All subsequent requests to the other operations of this API have to include the header ``Cookie`` with the value of this header. 

   For example, the operation "login" will return an HTTP header like this:

   .. code-block:: none
   
      Set-Cookie: JSESSIONID=24319EFA0F45145B64A5F9321F2DA6FB; Path=/; HttpOnly
   
   All the requests to other operations have to include the following header:
   
   .. code-block:: none
    
      Cookie: JSESSIONID=24319EFA0F45145B64A5F9321F2DA6FB

   Note that you have to obtain the value ``JSESSIONID=...`` from the response and ignore everything else after the first ``;``. That is, you have to **ignore** ``; Path=/; HttpOnly``.

-  The body of the response to this operation contains a JSON document with the following syntax:

.. code-block:: JavaScript
   :caption: Syntax of the responses of the operation "login"

   {
       "userName": <text>,
       "promotionAdminEnvironments": null | [ <list_of_environments> ], /* list of environments in which the promotion administrator is able to deploy a revision. Null implies no limitations */
       "promotionEnvironments": null | [ <list_of_environments> ], /* list of environments in which the promotion user is able to deploy a revision. Null implies no limitations */
        "promotion": <boolean>,
        "vdpAdmin": <boolean>,
       "jmxAdmin": <boolean>,
       "solutionManagerAdmin": <boolean>,
       "promotionAdmin": <boolean>
   }

    <environment>::= "DEVELOPMENT" | "PRODUCTION" | "STAGING"
    <list_of_environments>::= <environment> [ ,<environment> ]*

If the request fails, the operations return the HTTP code 401 (Unauthorized) or 500 if there is another error. The body of the response has this format:

.. code-block:: JavaScript
   :caption: Syntax of the error messages returned by the operation ``login``

   {
       "status" : <number>,
       "message" : <text>
   }

For example, if the credentials are invalid, the API returns the code 401 (Unauthorized):

.. code-block:: json

   {
       "status" : 401,
       "message" : "The user name or password is incorrect"
   }

.. rubric:: Example

The following example uses the tool "cURL" to create a session for the user "jsmith" with password "my_password".

.. code-block:: bash
   :caption: Example of a request to the operation "login" using "cURL"

   curl --include --user "jsmith:my_password" "https://solution-manager.acme.com:10090/login"

With the parameter ``--include``, cURL prints the headers of the response.

.. code-block:: none 
   :caption: Example of a response of the operation "login"
   :emphasize-lines: 3

   HTTP/1.1 200
   X-Application-Context: application:30090
   Set-Cookie: JSESSIONID=24319EFA0F45145B64A5F9321F2DA6FB; Path=/; HttpOnly
   Content-Type: application/json;charset=UTF-8
   Transfer-Encoding: chunked
   Date: Fri, 04 Oct 2019 20:11:16 GMT

   {
       "userName" : "admin",
       "promotionAdminEnvironments" : null,
       "promotionEnvironments" : null,
       "promotion" : true,
       "promotionAdmin" : true,
       "solutionManagerAdmin" : true,
       "vdpAdmin" : true,
       "jmxAdmin" : true
   }
   
All the subsequent request to this API have to include the header

.. code-block:: none

   Cookie: JSESSIONID=24319EFA0F45145B64A5F9321F2DA6FB
   
.. _sm_api_logout:

Close a Session with the Solution Manager
=========================================

Operation to close a session.

-  URL: ``/logout``
-  Method: ``GET``
-  HTTP headers:

   + ``Cookie: <JSESSIONID cookie>``

If the session is closed successfully, the API returns the HTTP code 200. The body of the response is empty.

.. rubric:: Example

.. code-block:: bash
   :caption: Example of a request to the operation "logout" using "cURL"

   curl --include --header "Cookie: JSESSIONID=DDD0656CF7F8D259966E443CD81903B5" "https://solution-manager.acme.com:10090/logout"


-  URL: ``/logout``
-  Method: ``GET``
-  HTTP headers:

   + ``Cookie``: JSESSIONID cookie.
   + ``Accept: application/json``

If the session has been successfully closed, the server sends the status code *200* with an empty response body.

.. code-block:: none

   GET https://solution-manager.acme.com:10090/logout HTTP/1.1
   Cookie: JSESSIONID=347C78C0446F03FBE4468A10F1F55A7A
   Accept: application/json

.. note::
   The required headers (``Cookie``, ``Content-Type`` and ``Accept``) are omitted for clarity.

.. _sm_api_get_environments:

Get the List of Environments
============================

You can get the list of environments defined in the Solution Manager executing the following request:

- URL: ``/environments``
- Method: ``GET``

If the request has been successfully executed, the server sends the status code *200*.
The response body includes the list of environments in JSON format. Each environment can contain the following information:

.. code-block:: JavaScript  

    {
        "id": <number>,
        "name": <text>,
        "description": <text>,
        "minimumUpdateValue":<number>  /* format yyyymmdd */,
        "minimumUpdateMandatory": <bool> /* the minimum update is mandatory in the Virtual DataPort Administration Tool */,
        "minimumUpdateDownloadUrl": <URL>,
        "licenseAlias": <text>
    }

The following example represents a list of 2 environments.

.. code-block:: JavaScript  

    [
        {
            "id": 1,
            "name": "Environment 1",
            "description": "First environment",
            "minimumUpdateMandatory": false,
            "licenseAlias": "PRODUCTION"
        },
        {
            "id": 2,
            "name": "Environment 2",
            "description": "Second environment",
            "minimumUpdateMandatory": false,
            "licenseAlias": "DEVELOPMENT"
        }
    ]

.. _sm_api_create_environment:

Create an Environment
=====================

Endpoint to create an environment:

- URL: ``/environments``
- Method: ``POST``
- Syntax of the body of the request:

.. code-block:: JavaScript  

   {
       "name": <text>,
       "description": <text>,  /* optional */
       "minimumUpdateValue":<number>  /* /* optional, format yyyymmdd */,
       "minimumUpdateMandatory": <bool> /* optional, the minimum update is mandatory in the Virtual DataPort Administration Tool */,
       "minimumUpdateDownloadUrl": <URL> /* optional */,
       "licenseAlias": <text>    /* optional, assign a license to this environment */
   }
    
.. note:: To assign a license to the environment, indicate the license alias. To obtain it, use the operation :ref:`licenseAlias <sm_api_get_license_alias>`.

If the request succeeds, the service returns the HTTP code 201 and a response that contains the id of the new environment.

.. rubric:: Example

In this example, we create a file "new_environment.json" with the body of the request and then, 
use cURL to send a request with the content of this file.

.. code-block:: json
   :caption: Content of the file "new_environment.json"

   {
       "name": "finance-development",
       "description": "Environment for the development servers allocated to the finance department",
       "minimumUpdateValue": "20190312",
       "minimumUpdateDownloadUrl": "https://denodo-repository.acme.com/denodo-v70-update-20190903.zip",
       "minimumUpdateMandatory": true,
       "licenseAlias": "DEVELOPMENT"
   }

.. code-block:: bash
   :caption: Example of a request to the operation "create environment" using "cURL"

   curl --data @new_environment.json --header "Content-Type: application/json" --header "Cookie: JSESSIONID=C02DD094790C29740C080A8A9202DB61" "https://solution-manager.acme.com:10090/environments"

.. code-block:: json
   :emphasize-lines: 2
   :caption: Example of a response of the operation "create environment"

   {
       "id": 1,
       "name": "finance-development",
       "description": "Environment for the development servers allocated to the finance department",        
       "minimumUpdateValue": "20190312",
       "minimumUpdateDownloadUrl": "https://denodo-repository.acme.com/denodo-v70-update-20190903.zip",
       "minimumUpdateMandatory": true,
       "licenseAlias": "DEVELOPMENT"
   }

.. _sm_api_get_license_alias:
    
Get the List of Licenses Available
==================================

Get the list of licenses available license loaded in the Solution Manager.

-  URL: ``/licenseAlias``
-  Method: ``GET``

If the request succeeds, the service returns the HTTP code 200 and the body of the response contains the list of licenses in JSON format. Each license can contain the following information:

.. code-block:: JavaScript  

   {
       "alias": <text>,  /* license alias name */
       "licenseType": <text>,  /* license type */
       "version": <text>,  /* licence version number, i.e: 7.0 */
       "uniqueLicenseType": boolean /* indicates if there is more than one license with the same type */
   }

.. rubric:: Example of response

.. code-block:: json

   [{
           "alias": "DEVELOPMENT",
           "licenseType": "DEVELOPMENT",
           "version": "7.0",
           "uniqueLicenseType": true
       }, {
           "alias": "PERSONAL_DEVELOPER",
           "licenseType": "PERSONAL_DEVELOPER",
           "version": "7.0",
           "uniqueLicenseType": true
       }, {
           "alias": "PRODUCTION",
           "licenseType": "PRODUCTION",
           "version": "7.0",
           "uniqueLicenseType": true
       }
   ]

    
.. _sm_api_update_environment:
    
Update an Environment
=====================

Operation to modify the configuration of an environment.

-  URL: ``/environments/{number:environmentId}``
-  Method: ``PUT``
-  Syntax of the body of the request:

   .. code-block:: JavaScript  

       {
           "name": <text>,
           "description": <text>,  /* optional */
           "minimumUpdateValue":<number>  /* /* optional, format yyyymmdd */,
           "minimumUpdateMandatory": <bool> /* optional, the minimum update is mandatory in the Virtual DataPort Administration Tool */,
           "minimumUpdateDownloadUrl": <URL> /* optional */,
           "licenseAlias": <text>    /* optional, associate the environemnt with a license */
       }

If the request succeeds, the service returns the HTTP code 200 and the body of the response contains a JSON document with the settings of the environment.

.. rubric:: Example    

Example of a PUT request:

.. code-block:: json

    {
        "name": "env",
        "description": "new description of the environment",
        "minimumUpdateValue": "20190312",
        "minimumUpdateDownloadUrl": "http://denodo.com/update20190312.zip",
        "minimumUpdateMandatory": false,
        "licenseAlias": "DEVELOPMENT"
    }   
    
Sample response:

.. code-block:: json
    
   {
       "id": 1,
       "name": "env",
       "description": "sample updated environment",
       "minimumUpdateValue": "20190312",
       "minimumUpdateMandatory": false,
       "minimumUpdateDownloadUrl": "http://denodo.com/update20190312.zip",
       "licenseAlias": "DEVELOPMENT"
   }


.. _sm_api_delete_environment:  
    
Delete an Environment
=====================

Delete an environment, and all the clusters and servers within the environment:

- URL: ``/environments/{number:environmentId}``
- Method: ``DELETE``

.. rubric:: Example

The following request deletes the environment with identifier ``1`` and its clusters and servers:

.. code-block:: none

    DELETE https://localhost:10090/environments/1

If the request succeeds, the service returns the HTTP code 204.

.. _sm_api_get_environment_properties:

Get the List of Virtual DataPort Properties Associated to an Environment
========================================================================

Obtain the list of the Virtual DataPort properties defined on an environment.

-  URL: ``/environments/{number:environmentId}/vdpProperties``
-  Method: ``GET``
-  URL parameters. All of them are optional and can be combined:

  -  ``propertyName``: this parameter specifies an exact property name. If the URL contains this parameter, the operation ignores the parameters below.
  -  ``databaseName``: filters properties by database name.
  -  ``elementType``: filters properties by element type (for example, view or data source).
  -  ``elementTypeName``: filters properties by element type name (for example, JDBC, XML, etc.).
  -  ``elementName``: filters properties by element name.

The URL path must include the environment identifier.

The response has this format:

.. code-block:: JavaScript

    {
        "environmentProperties": [
            {
                "id": <number>,
                "name": <text>,
                "value": <text>,
                "propertyType": "VDP",
                "parentId": <number>,
                "undefined": <boolean>
            },
            { ... }
       ]
    }

.. rubric:: Example #1

The following example represents a response that returns the properties of the environment with identifier 4.

.. code-block:: bash

   curl --header "Cookie: JSESSIONID=7A97C05B36D5496B0E509AE12E70965C" "https://solution-manager.acme.com:10090/environments/4/vdpProperties"

.. code-block:: JavaScript
   :caption: Response

    {
        "environmentProperties": [
            {
                "id": 30,
                "name": "users.smadmin.PASSWORD",
                "value": "mYgfbAayggeImPPOGfklFLfgt ...",
                "propertyType": "VDP",
                "parentId": 3,
                "undefined":false
            },
            {
                "id": 32,
                "name": "users.smadmin.PASSWORD.ALGORITHM",
                "value": "SHA512",
                "propertyType": "VDP",
                "parentId": 3,
                "undefined":false
            },
            {
                "id": 31,
                "name": "users.smadmin.PASSWORD.ENCRYPTED",
                "value": "ENCRYPTED",
                "propertyType": "VDP",
                "parentId": 3,
                "undefined":false
            },
            {
                "id": 32,
                "name": "undefinedProperty",
                "value": "",
                "propertyType": "VDP",
                "parentId": 1,
                "undefined": true
            }
        ]
   }

.. rubric:: Example #2

The following URL uses the parameter ``propertyName`` to obtain the value of the property ``users.smadmin.PASSWORD.ALGORITHM`` in the environment with id #2.

.. code-block:: none

   GET https://localhost:10090/environments/2/vdpProperties?propertyName=users.smadmin.PASSWORD.ALGORITHM

This parameter cannot be combined with others.

.. rubric:: Example #3

The following URL uses the parameter ``databaseName`` to obtain the properties defined for the database "admin":

.. code-block:: none

   GET https://localhost:10090/environments/2/vdpProperties?databaseName=admin

.. rubric:: Example #4

The following URL uses the parameter ``elementType`` to obtain the properties of the data sources:

.. code-block:: none

   GET https://localhost:10090/environments/2/vdpProperties?elementType=datasources

.. rubric:: Example #5

The following URL uses the parameter ``elementTypeName`` to obtain the properties of all the JDBC data sources:

.. code-block:: none

   GET https://localhost:10090/environments/2/vdpProperties?elementTypeName=jdbc

.. rubric:: Example #6

The following URL uses the parameter ``elementName`` to obtain the properties of the elements whose name is "oracle12c":

.. code-block:: none

   GET https://localhost:10090/environments/2/vdpProperties?elementName=oracle12c


.. rubric:: Example #7

The following URL uses the parameters ``databaseName``, ``elementType`` and ``elementTypeName`` to obtain the properties of the LDAP data sources of the database admin:

.. code-block:: none

  GET https://localhost:10090/environments/2/vdpProperties?databaseName=admin&elementType=datasources&elementTypeName=ldap

.. _sm_api_create_delete_environment_properties:

Create and Delete Environment Properties
========================================

If you execute a POST request to the following URL you can create an environment property:

- URL: ``/environments/{number:environmentId}/vdpProperties``
- Method: ``POST``

The request body must include the list of properties, according to the following format:

.. code-block:: JavaScript  

    [
       {
            "name" : <text>,
            "value" : <text>
            "undefined" : <boolean>
       },
       { ... }
    ]

The *undefined* property is not mandatory, if you do not indicate it, this value is false by default.

The next example shows a POST request which creates 3 properties:

.. code-block:: JavaScript

    [
        {
            "name": "users.smadmin.PASSWORD",
            "value": "mYgfbAayggeImPPOGfklFLfgt2Azb/5TVjDtYQChCE29k6tjDru7 ... "
        },
        {
            "name": "users.smadmin.PASSWORD.ALGORITHM",
            "value": "SHA512"
        },
        {
            "name": "undefinedProperty",
            "value": "",
            "undefined": true
        }
    ]

The response body contains the new properties with their identifiers:

.. code-block:: JavaScript

    [
        {
            "id": 30,
            "name": "users.smadmin.PASSWORD",
            "value": "mYgfbAayggeImPPOGfklFLfgt2Azb/5TVjDtYQChCE29k6tjDru7 ... "
            "propertyType": "VDP",
            "parentId": 4,
            "undefined": false
        },
        {
            "id": 32,
            "name": "users.smadmin.PASSWORD.ALGORITHM",
            "value": "SHA512"
            "propertyType": "VDP",
            "parentId": 4,
            "undefined": false
        },
        {
            "id": 33,
            "name": "undefinedProperty",
            "value": "",
            "propertyType": "VDP",
            "parentId": 4,
            "undefined": true
        }
    ]

If you send a DELETE request to the following URL, you can delete an environment property:

- URL: ``/vdpProperties/{number:vdpPropertyId}``
- Method: ``DELETE``

The next example deletes a property with the identifier 5:

.. code-block:: none

    DELETE https://localhost:10090/vdpProperties/5

If the property has been successfully deleted, the response status code is *204*.

.. _sm_api_get_clusters:

Get the List of Clusters
========================

The following request returns the list of clusters of an environment:

- URL: ``/environments/{number:environmentId}/clusters``
- Method: ``GET``

If the request has been successfully executed, the server sends the status code *200*.
The response body includes the list of clusters in JSON format. Each cluster can contain the following information:

.. code-block:: JavaScript  

    {
        "clusters": [
            {
                "id": <number>,
                "name": <text>,
                "enabled": <bool>
            },
            { ... }
        ]
    }

The following example returns a list with 2 clusters.

.. code-block:: JavaScript  

    {
        "clusters": [
            {
                "id": 7,
                "name": "Cluster 2",
                "enabled": true
            },
            {
                "id": 8,
                "name": "Cluster 3",
                "enabled": false
            }
        ]
    }



.. _sm_api_create_cluster:

Create a Cluster
================

If you execute a POST request to the following URL you can create a cluster:

- URL: ``/clusters``
- Method: ``POST``

The request body has to follow this format:

.. code-block:: JavaScript  
    
    {
        "name": <text>,
        "description": <text>, /* optional */
        "environmentId": <number>,
        "enabled": boolean
    }

The environmentId is the environment identifier that can be obtained in :ref:`sm_api_get_environments`
    
    
If the cluster has been successfully inserted, the response status code is *201*.


The next example shows a POST request which create a cluster:

.. code-block:: JavaScript

    {       
        "name": "clusterSample",
        "description": "A cluster sample",      
        "environmentId": 1,
        "enabled": true
    }
    

The response body contains the added cluster with its identifier:

.. code-block:: JavaScript

    {
        "id": 10,
        "name": "clusterSample",
        "description": "A cluster sample",
        "order": 1,
        "environmentId": 1,
        "enabled": true
    }   
    
The returned order indicates the order of the cluster within the environment.
    
    
.. _sm_api_get_cluster:

Get a Cluster
=============

The following request returns the list of clusters of an environment:

- URL: ``/clusters/{number:clusterId}``
- Method: ``GET``

If the request has been successfully executed, the server sends the status code *200*.
The response body includes the cluster in JSON format with the following information:

.. code-block:: JavaScript  

    {
        "id": <numer>, 
        "name": <text>,
        "description": <text>, 
        "order": <number>, 
        "environmentId": <number>,
        "enabled": boolean
    }    

The following example returns the cluster with id 10.

.. code-block:: JavaScript  
    
    {
        "id": 10,
        "name": "clusterSample",
        "description": "A cluster sample",
        "order": 1,
        "environmentId": 1,
        "enabled": true
    }
    
    
.. _sm_api_update_cluster:
    
Update a Cluster
================

If you execute a PUT request to the following URL you can update a cluster:

- URL: ``/clusters/{number:clusterId}``
- Method: ``PUT``
    
The request body has to follow this format:

.. code-block:: JavaScript  

    {
        "name": <text>,
        "description": <text>,  /* optional */
        "environmentId": <number>,
        "enabled": boolean      
    }

If the cluster has been successfully updated, the response status code is *200*.    
    
The next example shows a PUT request which update the cluster with id 10:

.. code-block:: JavaScript

    {
        "name": "clusterSample updated",
        "description": "A cluster sample updated",
        "environmentId": 1,
        "enabled": true
    }   

    
The response body contains the udpated cluster:

.. code-block:: JavaScript      

      {
          "id": 10,
          "name": "clusterSample updated",
          "description": "A cluster sample updated",
          "order": 1,
          "environmentId": 1,
          "enabled": true
      }


.. _sm_api_delete_cluster:  
    
Delete a Cluster
================

    
If you send a DELETE request to the following URL, you can delete a cluster:

- URL: ``/clusters/{number:clusterId}``
- Method: ``DELETE``

The next example deletes a cluster with the identifier 10:

.. code-block:: none

    DELETE https://localhost:10090/clusters/10

If the cluster has been successfully deleted, the response status code is *204*.
This operation deletes all the servers contained in the cluster.



.. _sm_api_get_scheduler_properties:

Get the List of Scheduler Properties Associated to a Cluster
============================================================

The following URL returns the list of Scheduler properties for a given cluster:

- URL: ``/clusters/{number:clusterId}/schProperties``
- Method: ``GET``
- Parameters:

    + ``propertyName``: this parameter specifies an exact property name.
    + ``projectName``: this parameter filters properties by project name.
    + ``elementType``: this parameter filters properties by element type (for example, data source).
    + ``elementTypeName``: this parameter filters properties by element type name (for example, *VDP*).
    + ``elementName``: this parameter filters properties by element name.

The URL path must include the cluster identifier. The following URL returns the Scheduler properties for the cluster with the identifier 3.

.. code-block:: none

    GET https://localhost:10090/cluster/3/schProperties

Optional Request Parameters to Filter Cluster Properties
==============================================================

The following URL specifies the parameter *propertyName* and returns a Scheduler property with name ``SolutionManager.dataSource.ITP.itpds.dbName``. 

.. code-block:: none

    GET https://localhost:10090/clusters/4/schProperties?propertyName=SolutionManager.dataSource.ITP.itp.dbName

.. note:: 
    If *propertyName* is specified, the other parameters are ignored.

The parameter *projectName* filters properties by project. 
The following URL returns all properties matching ``SolutionManager.*``

.. code-block:: none

    GET https://localhost:10090/clusters/2/schProperties?projectName=SolutionManager

The parameter *elementType* filters properties by element type. 
The following URL returns all properties matching ``*.datasource.*``. Example: ``SolutionManager.dataSource.ITP.itpds.host``. 

.. code-block:: none

    GET https://localhost:10090/clusters/2/schProperties?elementType=datasource

The parameter *elementTypeName* filters properties by element type name.
The following URL returns all properties related with *ITP*. Example: ``SolutionManager.dataSource.ITP.itpds.host``.

.. code-block:: none

    GET https://localhost:10090/clusters/2/schProperties?elementTypeName=ITP

The parameter *elementName* filters properties by element name. 
The following URL returns the properties of the data source named *itpds*.

.. code-block:: none

    GET https://localhost:10090/clusters/2/schProperties?elementName=itpds

Except the parameter *propertyName*, the others can be combined in the same request. 
For example, the following URL returns all data source properties of the project *SolutionManager*.

.. code-block:: none

    GET https://localhost:10090/clusters/2/schProperties?projectName=SolutionManager&elementType=datasource

.. _sm_api_create_delete_cluster_properties:

Create and Delete Cluster Properties
====================================

You can execute a POST request to the following URL to create cluster properties:

- URL: ``/clusters/{number:clusterId}/schProperties``
- Method: ``POST``

The request body must include the list of properties, according to the following format:

.. code-block:: JavaScript  

    [
       {
            "name" : <text>,
            "value" : <text>,
            "undefined" : <boolean>
       },
       { ... }
    ]

The *undefined* property is not mandatory, if you do not indicate it, this value is false by default.

The next example shows a POST request which creates 3 properties:

.. code-block:: JavaScript

    [
        {
            "name": "default.dataSource.VDP.vdp.connectionURI",
            "value": "//localhost:19999/admin"
        },
        {
            "name": "default.dataSource.VDP.vdp.login",
            "value": "admin"
        },
        {
            "name": "undefinedProperty",
            "value": "",
            "undefined": true
        }
    ]

The response body includes the created properties with their identifiers.

.. code-block:: JavaScript

    [
        {
            "id": 34,
            "name": "default.dataSource.VDP.vdp.connectionURI",
            "value": "//localhost:19999/admin",
            "propertyType": "SCHEDULER",
            "parentId": 5,
            "undefined": false
        },
        {
            "id": 35,
            "name": "default.dataSource.VDP.vdp.login",
            "value": "admin",
            "propertyType": "SCHEDULER",
            "parentId": 5,
            "undefined": false
        },
        {
            "id": 36,
            "name": "undefinedProperty",
            "value": "",
            "propertyType": "SCHEDULER",
            "parentId": 5,
            "undefined": true
        }
    ]

If you send a DELETE request to the following URL, you can delete a cluster property:

- URL: ``/schProperties/{number:schPropertyId}``
- Method: ``DELETE``

The next example deletes a property with identifier 5:

.. code-block:: none

    DELETE https://localhost:10090/schProperties/5

If the property has been successfully deleted, the response status code is *204*.


.. _sm_api_create_server:

Create a server
==============================================================

If you execute a POST request to the following URL you can create a server:

- URL: ``/servers``
- Method: ``POST``

The request body has to follow this format:

.. code-block:: JavaScript  
    
    {
        "id": <numer>, 
        "name": <text>,
        "description": <text>,  /* optional */
        "typeNode": <text>, /* the valid values are <VDP || SCHEDULER || ITP_BROWSER_POOL || ITP_VERIFICATION || VDP_DATA_CATALOG> */
        "urlIP": <text>,
        "urlPort": <number>,
        "clusterId": <number>,
        "useKerberos": <boolean>,
        "usePassThrough": <boolean>,
        "username": <text>,
        "password": <text>,
        "defaultDatabase": <text>,
        "enabled": <boolean>,
        "licenseAlias": <text>,  /* optional */ 
        "useDefaultLicenseAlias": <boolean> 
    }

The clusterId must be the cluster identifier that can be obtained in :ref:`sm_api_get_clusters`. 

The password accepts the following values:

- clear password
- password encrypted with the encrypt_password script located in <SOLUTION_MANAGER_HOME>/bin
        
If useDefaultLicenseAlias is true, you can ommit the licenseAlias field or use an empty string. If useDefaultLicenseAlias is false, then you must indicate the license alias that can be obtained in :ref:`sm_api_get_license_alias`.
If the server has been successfully inserted, the response status code is *201*.

The next example shows a POST request which create a server:

.. code-block:: JavaScript

    {       
        "name": "vdpLocal",
        "description": "",
        "typeNode": "VDP",
        "urlIP": "cajun",
        "urlPort": 9999,
        "clusterId": 11,
        "useKerberos": false,
        "usePassThrough": false,
        "username": "admin",
        "password": "admin",
        "defaultDatabase": "admin",
        "enabled": true,
        "licenseAlias": "",
        "useDefaultLicenseAlias": true
    }


    
In this example password is a clear password. 
If the password is generated with the encrypt_password script, the password will be similar to:

.. code-block:: JavaScript

    "password": "8seLz7OWe6SRc9ivmekrkE+NTn8DzGxy",


The response body contains the added server with its identifier:

.. code-block:: JavaScript

    {
        "id": 12,
        "name": "vdpLocal",
        "description": "",
        "typeNode": "VDP",
        "urlIP": "cajun",
        "urlPort": 9999,
        "clusterId": 11,
        "useKerberos": false,
        "usePassThrough": false,
        "username": "admin",
        "defaultDatabase": "admin",
        "enabled": true,
        "licenseAlias": "",
        "useDefaultLicenseAlias": true
    }
    
    
    
    
    
.. _sm_api_get_server:

Get a Server
==============================================================

The following request returns a server:

- URL: ``/servers/{number:serverId}``
- Method: ``GET``

If the request has been successfully executed, the server sends the status code *200*.
The response body includes the server in JSON format with the following information:

.. code-block:: JavaScript  

    {
        "id": <numer>, 
        "name": <text>,
        "description": <text>, 
        "typeNode": <text>, 
        "urlIP": <text>,
        "urlPort": <number>,
        "clusterId": <number>,
        "useKerberos": <boolean>,
        "usePassThrough": <boolean>,
        "username": <text>,
        "defaultDatabase": <text>,
        "enabled": <boolean>,
        "licenseAlias": <text>,
        "useDefaultLicenseAlias": <boolean> 
    }    


Any property with null value will not be included in the JSON.
    
The following example returns the server with id 12.

.. code-block:: JavaScript  
    
    {
        "id": 12,
        "name": "vdpLocal",
        "description": "",
        "typeNode": "VDP",
        "urlIP": "cajun",
        "urlPort": 9999,
        "clusterId": 11,
        "useKerberos": false,
        "usePassThrough": false,
        "username": "admin",
        "defaultDatabase": "admin",
        "enabled": true,
        "licenseAlias": "",
        "useDefaultLicenseAlias": true
    }


    
    
.. _sm_api_update_server:
    
Update a Server
==============================================================

If you execute a PUT request to the following URL you can update a server:

- URL: ``/servers/{number:serverId}``
- Method: ``PUT``
    
The request body has to follow this format:

.. code-block:: JavaScript  

    {
        "name": <text>,
        "description": <text>,  /* optional */
        "typeNode": <text>, /* the valid values are <VDP || SCHEDULER || ITP_BROWSER_POOL || ITP_VERIFICATION || VDP_DATA_CATALOG> */
        "urlIP": <text>,
        "urlPort": <number>,
        "clusterId": <number>,
        "useKerberos": <boolean>,
        "usePassThrough": <boolean>,
        "username": <text>,
        "password": <text>,
        "defaultDatabase": <text>,
        "enabled": <boolean>,
        "licenseAlias": <text>,  /* optional */
        "useDefaultLicenseAlias": <boolean> 
    }

.. note::
    
    The password accepts the following values:
    
    - "************" (8 asterisks), this values does not modify the password value
    - Clear password
    - A password encrypted with the encrypt_password script located in <SOLUTION_MANAGER_HOME>/bin  
    
    
If the server has been successfully updated, the response status code is *200*. 
    
The next example shows a PUT request which update the server with id 12:

.. code-block:: JavaScript

    {
        "name": "vdpLocal-modif",
        "description": "",
        "typeNode": "VDP",
        "urlIP": "cajun",
        "urlPort": 9999,
        "clusterId": 11,
        "useKerberos": false,
        "usePassThrough": false,
        "username": "admin",
        "password": "********",
        "defaultDatabase": "admin",
        "enabled": true,
        "licenseAlias": "",
        "useDefaultLicenseAlias": true
    }   

    
The response body contains the updated server:

.. code-block:: JavaScript  

    {
        "id": 12,
        "name": "vdpLocal-modif",
        "description": "",
        "typeNode": "VDP",
        "urlIP": "cajun",
        "urlPort": 9999,
        "clusterId": 11,
        "useKerberos": false,
        "usePassThrough": false,
        "username": "admin",
        "defaultDatabase": "admin",
        "enabled": true,
        "licenseAlias": "",
        "useDefaultLicenseAlias": true
    }


.. _sm_api_delete_server:   
    
Delete a Server
==============================================================

    
If you send a DELETE request to the following URL, you can delete a server:

- URL: ``/server/{number:serverId}``
- Method: ``DELETE``

The next example deletes a property with the identifier 12:

.. code-block:: none

    DELETE https://localhost:10090/servers/12

If the server has been successfully deleted, the response status code is *204*.




.. _sm_api_get_revisions:

Get the List of Revisions
==============================================================

The following URL returns the list of revisions:

- URL: ``/revisions`` 
- Method: ``GET``
- Parameters:

    + ``start``: this parameter specifies the first element of the collection to be returned.
    + ``count``: this parameter specifies the maximum number of results to be returned.
      
The response body includes the list of revisions according to the following format:

.. code-block:: JavaScript

    {
        "start": <number>, /* specifies the first revision of the collection to be returned */
        "count": <number>, /* maximum number of revisions to be returned */
        "numElements": <number>, /* total number of revisions included in the response */
        "revisions": [
            {
                "id": <number>,
                "description": <text>,
                "name": <text>,
                "type": ["INSERT" | "DELETE"], /* alias for CREATE and DROP */
                "replace": ["DO_NOT_REPLACE_EXISTING" | "REPLACE_EXISTING" | "DROP_ELEMENTS_BEFORE"],
                "creationUser": <text>,
                "creationTime": <date>, /* date format: yyyy-MM-dd'T'HH:mm:ss.SSZ */
                "lastModifiedUser": <admin>,
                "lastModifiedTime": <date>, /* date format: yyyy-MM-dd'T'HH:mm:ss.SSZ */
                "hasVql": <bool>,
                "hasScheduler": <bool>,
                "includeJars": <bool>,
                "includeStatistics": <bool>,
                "useDefaultDatabase": <bool>,
                "deployedOn": [ /* list of environments in which the revision was already deployed */
                    {
                        "id": <number>,
                        "name": <text>,
                        "description": <text>,
                        "minimumUpdateMandatory": <bool>
                    },
                    { ... }
                ]
            },
            { ... }
        ]
    }

The following example represents two revisions in JSON format. 
The first one was deployed on 2 environment, the second one was not deployed on any environment:

.. code-block:: JavaScript

    {
       "start": 0,
       "count": 10,
       "numElements": 2,
       "revisions": [
            {
                "id": 3,
                "description": "Third revision",
                "name": "Revision 3",
                "type": "INSERT",
                "replace": "DO_NOT_REPLACE_EXISTING",
                "creationUser": "smadmin",
                "creationTime": "2017-11-11T00:15:34.507+0000",
                "lastModifiedUser": "smadmin",
                "lastModifiedTime": "2017-11-11T00:15:34.507+0000",
                "hasVql": true,
                "hasScheduler": false,
                "includeJars": false,
                "includeStatistics": false,
                "useDefaultDatabase": true,
                "deployedOn": [
                    {
                        "id": 2,
                        "name": "Environment 2",
                        "description": "Second environment",
                        "minimumUpdateMandatory": false
                    },
                    {
                        "id": 3,
                        "name": "Environment 3",
                        "description": "Third environment",
                        "minimumUpdateMandatory": false
                    }
                ]
            },
            {
                "id": 2,
                "description": "Second revision",
                "name": "Revision 2",
                "type": "DELETE",
                "replace": "DROP_ELEMENTS_BEFORE",
                "creationUser": "smadmin",
                "creationTime": "2017-11-11T00:15:34.429+0000",
                "lastModifiedUser": "smadmin",
                "lastModifiedTime": "2017-11-11T00:15:34.429+0000",
                "hasVql": true,
                "hasScheduler": false,
                "includeJars": false,
                "includeStatistics": false,
                "useDefaultDatabase": true
            }
        ]
    }


.. _sm_api_create_revision_from_vql:

Create a Revision from a VQL file
==============================================================

You can execute a POST request to the following URL to create a revision from a VQL file:

- URL: ``/revisions/loadFromVQL`` 
- Method: ``POST``

The request body has to follow this format:


.. code-block:: JavaScript

    {
        "name": <text>, /* descriptive name for the revision*/ 
        "description": <text>,  /* optional. Extensive description about the revision. */
        "content": <text> /* vql file content as **xsd:base64Binary** encoded in **UTF-8** */
    }



.. note::  The content must the vql content file as **xsd:base64Binary** encoded in **UTF-8**. 
   The revision will be created even if the vql is not parameterized with environment properties. We strongly recommend that you always generate vql files with specific environment properties separately. If the vql is generated using the VDP Admin Tool, you should select the option *Export environment specific properties separately* in the *Export* and *Export database* dialogs, otherwise check that the vql does not contain environment property values. Take into account that deploying a revision created from a vql without separate environment properties can generate unexpected results.


.. note:: The vql files generated when exporting elements using VDP Admin Tool includes the **BOM** mark to indicate that they are encoded in UTF-8. The **BOM** mark must be skipped.


The response body contains the added revision with its identifier:

.. code-block:: JavaScript


    {
        "id": 4,
        "description": "Creating revision using exportTestDB-WithProperties",
        "name": "revFromVQLWithProperties",
        "type": "VQL",
        "replace": "REPLACE_EXISTING",
        "creationUser": "admin",
        "creationTime": "2019-08-27T15:22:59.292+0000",
        "lastModifiedUser": "admin",
        "lastModifiedTime": "2019-08-27T15:22:59.292+0000",
        "hasVql": false,
        "hasScheduler": false,
        "includeJars": false,
        "includeI18N": false,
        "includeStatistics": false,
        "includeServerProperties": false,
        "includeWebContainerProperties": false,
        "includeJdbcWrapperProperties": true,
        "includeVdpDependencies": false,
        "useDefaultDatabase": true
    }





.. _sm_api_get_deployments:

Get the List of Deployments
==============================================================

The following URL returns the list of deployments:

- URL: ``/deployments`` 
- Method: ``GET``
- Parameters:

    + ``start``: this parameter specifies the first deployment of the collection to be returned.
    + ``count``: this parameter specifies the maximum number of deployments to be returned.
    + ``environmentId``: this parameter filters deployments by target environment.
    + ``status``: this parameter filters deployments by execution status (*OK*, *ERROR*, *IN_PROGRESS*).
    + ``description``: this parameter filters deployments by description.
    + ``user``: this parameter filters deployments by user.
    + ``startDeploymentTime``: this parameter filters deployments by start deployment time. Date format is **yyyy-MM-dd'T'HH:mm:ssZ**.
    + ``endDeploymentTime``: this parameter filters deployments by end deployment time. Date format is **yyyy-MM-dd'T'HH:mm:ssZ**.

The response body includes the deployments in JSON format, according to the following template:

.. code-block:: JavaScript

    {
        "start": <number>,
        "count": <number>,
        "numElements": <number>,
        "deployments": [
            {
                "id": <number>,
                "user": <text>,
                "deploymentTime": <date>, /* date format: yyyy-MM-dd'T'HH:mm:ss.SSZ */
                "description": <text>,
                "state": ["OK" | "ERROR" | "IN_PROGRESS"]
                "targetEnvironment": {
                    "id": <number>,
                    "name": <text>,
                },
                "revisions": [
                    {
                        "id": <number>,
                        "description": <text>,
                        "name": <text>
                    },
                    { ... }
                ],
            },
            { ... }
        ]
    }

The following example returns a list of 2 deployments:

.. code-block:: JavaScript

    {
        "start": 0,
        "count": 10,
        "numElements": 2,
        "deployments": [
            {
                "id": 28,
                "user": "admin",
                "deploymentTime": "2017-11-12T23:05:53.179+0000",
                "description": "",
                "targetEnvironment": {
                   "id": 3,
                   "name": "Environment 3",
                 },
                "revisions": [
                    {
                        "id": 3,
                        "description": "Third revision",
                        "name": "Revision 3"
                    }
                ],
                "state": "OK"
            },
            {
                "id": 27,
                "user": "admin",
                "deploymentTime": "2017-11-12T23:05:40.838+0000",
                "description": "",
                "targetEnvironment": {
                    "id": 3,
                    "name": "Environment 3",
                },
                "revisions": [
                    {
                        "id": 3,
                        "description": "Third revision",
                        "name": "Revision 3"
                    }
                ],
                "state": "OK"
            }
        ]
    }

The following URL returns the list of deployments between ``2017-11-07T16:50:32+0100`` and ``2017-11-15T16:50:33+0100``.
You must specify both parameters, *startDeploymentTime* and *endDeploymentTime*, together.

.. code-block:: none

    GET https://localhost:10090/deployments?startDeploymentTime=2017-11-07T16%3A50%3A32%2B0100&endDeploymentTime=2017-11-15T16%3A50%3A33%2B0100


The following URL returns the list of deployments in progress.

.. code-block:: none

    GET https://localhost:10090/deployments?status=IN_PROGRESS

.. _sm_api_create_deployment:

Start a New Deployment from a List of Revisions
==============================================================

You can execute a new deployment sending a POST request to the following URL:

- URL: ``/deployments`` 
- Method: ``POST``

The request body must follow the template below. 

.. code-block:: JavaScript

    {
        "revisionIds": [ <number>, ... ],
        "environmentId": <number>,
        "description": <text>
    }

This request must include the list of revision identifiers (*revisionIds*) and also the identifier of the target environment (*environmentId*).
The following example creates a deployment from the revisions 1 and 2, on the target environment identified with the id 3.

.. code-block:: JavaScript

    {
        "revisionIds": [1, 2],
        "environmentId": 3,
        "description": "My first deployment"
    }

It the deployment has been successfully created, the response body returns the numeric deployment identifier.

If some property is missing in the target environment, the deployment process fails and the response body returns the result of the validation.

.. code-block:: JavaScript

    {
        "validationResponse": {
            "validation": {
                "resultVqlValidation": "ERROR",
                "outputVqlValidation": "Error validating revision 'Third revision'.\n Missing VQL properties: \ndatabases.admin.datasources.jdbc.oracle_ds.DATABASEURI\ndatabases.admin.datasources.jdbc.oracle_ds.USERNAME\ndatabases.admin.datasources.jdbc.oracle_ds.USERPASSWORD\ndatabases.admin.datasources.jdbc.oracle_ds.USERPASSWORD.ENCRYPTED\nusers.user1.PASSWORD\nusers.user1.PASSWORD.ALGORITHM\nusers.user1.PASSWORD.ENCRYPTED\n\n",
                "errorVqlValidation": "Error validating revision 'Third revision'.\n Some Virtual DataPort properties required by the revision are not defined in the target environment\n\n",
                "missingVqlPropertiesWithServerValues": {
                    "users.user1.PASSWORD": "ISTcnym4dJECzqYlA7+Xv6+RJo5s0VupzxEhXqoXC9XHu+5+M03kn3ftHGwW57FB5Q1d4Glcad4g54l9iBdEm55U431gIeob",
                    "users.user1.PASSWORD.ALGORITHM": "SHA512",
                    "databases.admin.datasources.jdbc.oracle_ds.USERPASSWORD.ENCRYPTED": "ENCRYPTED",
                    "databases.admin.datasources.jdbc.oracle_ds.USERPASSWORD": "hJfznA24IFFPM8Z+6qCDO6Kg2Z3OSjP8W+uooHqFUpp8MoJT1FC3gdgsFe8cyh+Xmvu0dPIkFY21fidp75fqE4euFqx9QpH4Y8gcRE3CaeszFlB97AXg4dJ99eJFbWZ7",
                    "users.user1.PASSWORD.ENCRYPTED": "ENCRYPTED",
                    "databases.admin.datasources.jdbc.oracle_ds.USERNAME": "dblogin",
                    "databases.admin.datasources.jdbc.oracle_ds.DATABASEURI": "jdbc:oracle:thin:@host:port:database"
                },
                "resultSchValidation": "OK",
                "outputSchValidation": "",
                "errorSchValidation": "",
                "targetEnvironment": {
                    "id": 1,
                    "name": "env1",
                    "description": "",
                    "minimumUpdateValue": "",
                    "minimumUpdateMandatory": false,
                    "minimumUpdateDownloadUrl": "",
                    "licenseAlias": "PRODUCTION_1"
                },
                "state": "ERROR",
                "messageResult": "Some of the Virtual DataPort properties included in the revision are not defined in the target environment. "
            },
            "scriptValidationsResult": []
        }
    }

.. _sm_api_get_deployment_progress:

Get the Progress of a Deployment
==============================================================

The following URL returns the deployment status and the list of deployment tasks:

- URL: ``/deployments/{number:deploymentId}/progress``
- Method: ``GET``
- Parameters

    + ``lastUpdate``: filters deployment tasks by last update date.

The response body includes the deployment progress and the list of deployment tasks according to the following template:

.. code-block:: JavaScript

    {
        "deploymentId": <number>,
        "state": ["OK" | "ERROR" | "IN_PROGRESS"],
        "progress": <number>, /* percent between 0 and 100 */
        "tasks": [
            {
                "id": <number>,
                "name": <text>,
                "state": ["OK" | "ERROR" | "IN_PROGRESS"],
                "startDate": <date>,
                "endDate": <date>,
                "deploymentExecutionType": <text>,
                "output": <text>,
                "cluster": {
                    "id": <number>,
                    "name": <text>
                },
                "server": {
                    "id": <number>,
                    "name": <text>
                }
            },
            { ... }
        ]
    }

The optional request parameter *lastUpdate* filters tasks by last update date. 
If it is specified, the response body only include tasks modified since this date.

The following example shows an example of deployment progress with 1 execution task:

.. code-block:: JavaScript

    {
        "deploymentId": 1,
        "state": "OK",
        "progress": 100,
        "tasks": [
            {
                "id": 1,
                "name": "Revision Revision 1 VQL deployment",
                "state": "OK",
                "startDate": "2017-11-13T11:18:40.999+0000",
                "endDate": "2017-11-13T11:18:41.065+0000",
                "deploymentExecutionType": "VQL",
                "output": " - Executed 1 of 1 VQL commands",
                "cluster": {
                   "id": 7,
                   "name": "Cluster 2"
                },
                "server": {
                    "id": 8,
                    "name": "Server 3"
                }
            }
        ]
    }
 
.. _sm_api_free_license_usage:

Free License Usage
==============================================================

When you stop a Denodo server (e.g. Virtual DataPort server, Scheduler server, etc.), it sends an HTTP request to the License Manager, to notify that is going to stop. When the License Manager receives this request, it frees the corresponding license so it can be used by another server.

If a Denodo server stops working unexpectedly (e.g. the machine where it runs is restarted unexpectedly), you have to manually notify the License Manager that the license that this Denodo server was using, is no longer in use. Otherwise, the license will be considered in use until the *grace period* expires (5 days since the last time this Denodo server renewed its license).

To free up a license, log into the Solution Manager. In the Elements Tree, open the definition of the server whose license you want to free up and copy the field *Host* .

Then, send a request to this endpoint:

-  URL: ``/externalShutdown``
-  Method: ``GET``
-  URL parameters:

   -  ``nodeIp``: if the field *Host* in the Solution Manager is an IP address - not a host name - the value of this parameter has to be this IP address.
   -  ``hostDomainName``: if the field *Host* in the Solution Manager is a host name, the value of this parameter has to be this host name.
   -  ``port``: port of the Denodo server.
   -  ``product``: the value has to be ``VDP``.

Consider these:

-  The names of the parameters are case sensitive.
-  In the Solution Manager 7.0 update 20190312 and previous ones, all the parameters are mandatory. If you provide a value for ``hostDomainName``, the value of the parameter ``nodeIp`` has to be empty but you still have to add it to the URL (``&nodeIp=``); If you provide a value for ``nodeIp``, the value of ``hostDomainName`` has to be empty but you still have to add it to the URL (``&hostDomainName=``).

   With newer updates of the Solution Manager, you have to add ``nodeIp`` **or** ``hostDomainName``, not both.
   
-  If a parameter is missing, the service returns the HTTP code 400 (Bad Request).
-  Unlike with the other endpoints described in this page, do not add the HTTP header ``Cookie`` to the requests. 
-  Unlike the other endpoints described in this page, the endpoint is the License Manager server, not the Solution Manager server. Therefore, the port is different (by default, 10091, not 10090 like the other operations)
   
.. rubric:: Examples

.. rubric:: Example #1

Free the license associated with the Virtual DataPort server that runs on the host ``denodo-dv1-prod.denodo.com``.

.. code-block:: none

   GET https://localhost:10091/externalShutdown?hostDomainName=denodo-dv1-prod.denodo.com&port=9999&product=VDP    
   
.. note:: If you are working with a Solution Manager with the update 20190312 or previous, you also have to add the parameter ``nodeIp`` to the URL, without any value (``&nodeIp=``). 
    
.. rubric:: Example #2

Free the license associated with the Virtual DataPort server that runs on the host with IP ``192.168.0.120``.
    
.. code-block:: none

   GET https://localhost:10091/externalShutdown?nodeIp=192.168.0.120&port=9999&product=VDP

.. note:: If you are working with a Solution Manager update 20190312 or previous, you also have to add the parameter ``hostDomainName`` to the URL, without any value (``&hostDomainName=``). 


The response from the service includes a summary of the operation with the result and a message:

.. code-block:: JavaScript

    {
        "result": ["OK" | "ERROR"],
        "message": <text>
    }

.. _sm_api_ping_license_manager:

Ping License Manager Server
==============================================================

Use the endpoint "ping License Manager" to check the health of the License Manager.

-  URL: ``https://denodo-server.acme.com:10091/pingLicenseManager``
-  Method: ``GET``

Note that you need to send the request to the License Manager (its default port is 10091), not the Solution Manager.

This endpoint returns the HTTP code 200 and the text ``OK`` if the server is up and the connection to the database where its metadata is stored is up. Otherwise, the request fails.

.. _sm_api_ping_solution_manager:

Ping to Solution Manager Server
==============================================================

Use the endpoint "ping Solution manager" to check the health of the Solution Manager server.

-  URL: ``https://denodo-server.acme.com:10090/pingSolutionManager``
-  Method: ``GET``

This endpoint returns the HTTP code 200 and the text ``OK`` if the server is up and the connection to the database where its metadata is stored is up. Otherwise, the request fails.