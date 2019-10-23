=========================
Querying Data: The Basics
=========================

This page subsections show how to query each type of element:

.. contents::
   :depth: 1
   :local:
   :backlinks: none


Querying Collections
====================

Go to following URL to obtain the "Service Metadata Document". It lists the entity set names and the URLs to their data::

  /denodo-odata.svc/<database name>/collectionName

Example::

  /denodo-odata.svc/movies/actor

Response:

.. code-block:: none

    {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor",
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
        },
        
        …
        
       {
          "actor_id": 1000,
          "first_name": "CHRISTIAN",
          "last_name": "GABLE",
          "last_update": "2006-02-15T11:34:33Z"
        }
      ],
      "@odata.nextLink": "/denodo-odata.svc/movies/actor?$skiptoken=1000"
    }

.. note::
  
  For the sake of simplicity, we are removing the server and port from the examples.

Obtaining Items by Primary Key
==============================

For the views that have primary key, each item can be identified using its primary key::

  denodo-odata.svc/<database name>/collectionName(keyvalue)

Examples::

  /denodo-odata.svc/movies/actor(1)
  /denodo-odata.svc/movies/store_category('F0')

If the primary key is made up of several fields, separate the values by commas::

  /denodo-odata.svc/<database name>/collectionName(key1,key2)

Example::

  /denodo-odata.svc/movies/film_actor(actor_id=1,film_id=1)

Accessing Individual Properties
===============================

The properties of an item can be accessed individually::

  /denodo-odata.svc/<database name>/collectionName(key)/propertyName

Example::

  /denodo-odata.svc/movies/actor(1)/first_name

Response:

.. code-block:: json

   {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor/first_name",
      "value": "PENELOPE"
   }
	
Accessing Individual Property Values
====================================

The value of a property is available as a raw value:

.. code-block:: none

   /denodo-odata.svc/<database name>/collectionName(key)/propertyName/$value

Example:

.. code-block:: none

   /denodo-odata.svc/movies/actor(1)/first_name/$value

Response:

.. code-block:: none

   PENELOPE

Accessing Complex Properties
============================

Properties can be complex but they are also available. You must point out the 
property path, from the complex to the simple one::

  /denodo-odata.svc/<database name>/collectionName(key)/propName/complexProp/propName

For example, for the following film_data complex field in a 
``struct_table_film`` entity:

.. code-block:: json

   {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#struct_table_film/$entity",
      "table_id": 1,
      "film_data": {
        "id": 1,
        "title": "ACADEMY DINOSAUR",
        "description": "ELIZABETH"
      }
   }
	
...we could perform the following call:

.. code-block:: none

  /denodo-odata.svc/movies/struct_table_film(1)/film_data/title

Response:

.. code-block:: json

   {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#struct_table_film/title",
      "value": "ACADEMY DINOSAUR"
   }

Counting Elements in a Collection: $count
=========================================

To obtain the number of elements in a collection, add ``$count`` to the URL:

.. code-block:: none

   /denodo-odata.svc/<database name>/collectionName/$count

Example:

.. code-block:: none

   /denodo-odata.svc/movies/actor/$count

Response::

  200

Establishing the Response Format
================================

OData can represent the data in two ways:

1. Atom (XML)
#. JSON. This is the default representation.

To request one representation of the data, add the query parameter ``$format`` to the URL. For example: ``?$format=atom``. Alternatively, add the HTTP header ``Accept`` to the request. If the request has the ``Accept`` header and the ``$format`` query parameter, the query 
parameter has a higher precedence.

JSON Format
-----------

To obtain the JSON representation of the data, add the query parameter ``$format`` to the URL (as defined by
`OData:JSON <http://docs.oasis-open.org/odata/odata-json-format/v4.0/odata-json-format-v4.0.html>`_):

.. code-block:: none

  /denodo-odata.svc/movies/actor?$format=json 

Response:

.. code-block:: none

   {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor",
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
        },
        …
       {
          "actor_id": 1000,
          "first_name": "CHRISTIAN",
          "last_name": "GABLE",
          "last_update": "2006-02-15T11:34:33Z"
        }
      ],
      "@odata.nextLink": "/denodo-odata.svc/movies/actor?$skiptoken=1000"
   }

You can append the parameter ``odata.metadata`` to ``$format`` to select the control information that will be included in the response: 

* ``odata.metadata=minimal`` (default value): whenever possible, the service will remove computable
  control information from the payload.
  
  .. code-block:: none
  
     /denodo-odata.svc/movies/actor?$format=application/json;odata.metadata=minimal

  The response is the actor data, just the same as a request with the abbreviation json and without 
  the parameter ``odata.metadata=minimal``.

* ``odata.metadata=full``: the response will include all control 
  information explicitly:
  
  .. code-block:: none

    /denodo-odata.svc/movies/actor?$format=application/json;odata.metadata=full

  Response:
  
  .. code-block:: none
  
     {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor",
      "value": [
        {
          "@odata.type": "#com.denodo.odata4.actor",
              "@odata.id": "/denodo-odata.svc/admin/actor(1)",
              "@odata.readLink": "/denodo-odata.svc/admin/actor(1)",
              "actor_id@odata.type": "#Int16",
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
          "films@odata.navigationLink": "/denodo-odata.svc/admin/actor(1)/films",
              "films@odata.associationLink": "/denodo-odata.svc/admin/actor(1)/films/$ref"
        },
        …
       {
          "@odata.type": "#com.denodo.odata4.actor",
              "@odata.id": "/denodo-odata.svc/admin/actor(1000)",
              "@odata.readLink": "/denodo-odata.svc/admin/actor(1000)",
              "actor_id@odata.type": "#Int16",
          "actor_id": 1000,
          "first_name": "CHRISTIAN",
          "last_name": "GABLE",
          "last_update": "2006-02-15T11:34:33Z"
          "films@odata.navigationLink": "/denodo-odata.svc/admin/actor(1000)/films",
              "films@odata.associationLink": "/denodo-odata.svc/admin/actor(1000)/films/$ref"
        }
      ],
      "@odata.nextLink": "/denodo-odata.svc/movies/actor?$skiptoken=1000"
     }

* ``odata.metadata=none``: the response will not contain any control 
  information other than ``odata.nextLink`` and ``odata.count``:

  .. code-block:: none

     /denodo-odata.svc/movies/actor?$format=application/json;odata.metadata=none

  Response:
  
  .. code-block:: none
  
     {
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
        },
        …
       {
          "actor_id": 1000,
          "first_name": "CHRISTIAN",
          "last_name": "GABLE",
          "last_update": "2006-02-15T11:34:33Z"
        }
      ],
      "@odata.nextLink": "/denodo-odata.svc/movies/actor?$skiptoken=1000"
     }
    
Alternatively, this format can be requested using the HTTP header ``Accept``:

* ``Accept: application/json``
* ``Accept: application/json;odata.metadata=minimal``
* ``Accept: application/json;odata.metadata=full``
* ``Accept: application/json;odata.metadata=none``

If the request has the query parameter ``$format`` and the HTTP header ``Accept`` header, the value of ``$format`` takes precedence.

Atom Format
-----------

To obtain the `Atom <http://docs.oasis-open.org/odata/odata-atom-format/v4.0/odata-atom-format-v4.0.html>`_ representation of the data, add the query parameter ``$format=atom`` to the URL:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$format=atom

Response:

.. code-block:: xml

    <?xml version='1.0' encoding='UTF-8'?>
    <a:feed xmlns:a="http://www.w3.org/2005/Atom" xmlns:m="http://docs.oasis-open.org/odata/ns/metadata" xmlns:d="http://docs.oasis-open.org/odata/ns/data" m:context="/denodo-odata.svc/movies/$metadata#actor">
        <a:id>/denodo-odata.svc/movies/actor</a:id>
        <a:link rel="next" href="/denodo-odata.svc/movies/actor?$format=atom&amp;$skiptoken=1000"/>
        <a:entry>
            <a:id>/denodo-odata.svc/movies/actor(1)</a:id>
            <a:title/>
            <a:summary/>
            <a:updated>2016-04-22T13:07:42Z</a:updated>
            <a:author>
                <a:name/>
            </a:author>
            <a:link rel="edit" href="/denodo-odata.svc/movies/actor(1)"/>
            <a:link rel="http://docs.oasis-open.org/odata/ns/related/actors" type="application/atom+xml;type=feed" title="actors" href="/denodo-odata.svc/movies/actor(1)/actors"/>
            <a:category scheme="http://docs.oasis-open.org/odata/ns/scheme" term="#com.denodo.odata4.actor"/>
            <a:content type="application/xml">
                <m:properties>
                    <d:actor_id m:type="Int16">1</d:actor_id>
                    <d:first_name>PENELOPE</d:first_name>
                    <d:last_name>GUINESS</d:last_name>
                    <d:last_update m:type="DateTimeOffset">2006-02-15T11:34:33Z</d:last_update>
                </m:properties>
            </a:content>
        </a:entry>
    ...
	
Alternatively, this format can be requested using the HTTP header ``Accept``:

.. code-block:: none

  Accept: application/atom+xml

Note that, if specified, ``$format`` overrides any value specified in the 
``Accept`` header.
