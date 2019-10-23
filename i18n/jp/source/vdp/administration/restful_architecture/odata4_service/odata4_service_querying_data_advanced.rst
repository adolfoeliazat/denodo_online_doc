=================
Advanced Querying
=================

OData defines some query options that allows refining the requests:

-  ``$filter``
-  ``$select``
-  ``$orderby``
-  ``$expand``

Selection: $filter
==================

A URI with a ``$filter`` system query option identifies a subset of the entries 
from the collection that satisfy the ``$filter`` predicate expression. 

Expressions can reference properties and literals. The latter can be strings 
(enclosed in single quotes), the ``null`` literal, numbers or boolean values.
 
The service supports the following operations and functions:

Operators
---------

+----------+-----------------------+-----------------------------------------------------------+
| Operator | Description           | Example                                                   |
+==========+=======================+===========================================================+
| eq       | Equal                 | ``/actor?$filter=first_name eq 'GRACE'``                  |
+----------+-----------------------+-----------------------------------------------------------+
| ne       | Not equal             | ``/actor?$filter=first_name ne 'GRACE'``                  |
+----------+-----------------------+-----------------------------------------------------------+
| gt       | Greater than          | ``/actor?$filter=actor_id gt 5``                          |
+----------+-----------------------+-----------------------------------------------------------+
| ge       | Greater than or equal | ``/actor?$filter=actor_id ge 5``                          |
+----------+-----------------------+-----------------------------------------------------------+
| lt       | Less than             | ``/actor?$filter=actor_id lt 10``                         |
+----------+-----------------------+-----------------------------------------------------------+
| le       | Less than or equal    | ``/actor?$filter=actor_id le 10``                         |
+----------+-----------------------+-----------------------------------------------------------+
| and      | Logical and           | ``/actor?$filter=actor_id gt 5 and actor_id lt 10``       |
+----------+-----------------------+-----------------------------------------------------------+
| or       | Logical or            | ``/actor?$filter=actor_id lt 5 or first_name eq 'GRACE'`` |
+----------+-----------------------+-----------------------------------------------------------+
| not      | Logical negation      | ``/actor?$filter=not (actor_id eq 1)``                    |
+----------+-----------------------+-----------------------------------------------------------+
| add      | Addition              | ``/film?$filter=length add 30 gt 180``                    |
+----------+-----------------------+-----------------------------------------------------------+
| sub      | Subtraction           | ``/film?$filter=length sub 30 gt 120``                    |
+----------+-----------------------+-----------------------------------------------------------+
| mul      | Multiplication        | ``/film?$filter=length mul 2 ge 300``                     |
+----------+-----------------------+-----------------------------------------------------------+
| div      | Division              | ``/film?$filter=length div 3 eq 60``                      |
+----------+-----------------------+-----------------------------------------------------------+
| mod      | Modulo                | ``/film?$filter=length mod 10 eq 8``                      |
+----------+-----------------------+-----------------------------------------------------------+
| ()       | Precedence grouping   | ``/actor?$filter=actor_id lt 7 and                        |
|          |                       | (first_name eq 'NICK' or actor_id gt 3)``                 |
+----------+-----------------------+-----------------------------------------------------------+

String Functions
----------------

The following functions are available for strings operations:

* ``contains(string p0, string p1)`` returns ``true`` when the value of the 
  property name specified in ``p0`` contains the string ``p1``. Otherwise 
  returns ``false``.
* ``startswith(string p0, string p1)`` returns ``true`` when the value of the 
  property name specified in ``p0`` starts with the string ``p1``. Otherwise 
  returns ``false``.
* ``endswith(string p0, string p1)`` returns ``true`` when the value of the 
  property name specified in ``p0`` ends with the string ``p1``. Otherwise 
  returns ``false``.
* ``indexof(string p0, string p1)`` returns the position of the string ``p1`` 
  in the value of the property name specified in ``p0``.
* ``length(p0)`` returns the length of the value of the property name specified 
  in ``p0``.
* ``substring(string p0, int pos)`` returns a new string that is a substring of 
  the value of the property name specified in ``p0``. The substring begins with 
  the character at the specified ``pos`` and extends to the end of this string.
* ``substring(string p0, int pos, int length)`` returns a new string that is a 
  substring of the value of the property name specified in ``p0``. The substring
  begins at the specified ``pos`` and extends to the character at index ``pos``
  + ``length``.
* ``tolower(string p0)`` returns a copy of the value of the property name 
  specified in ``p0`` converted to lowercase.
* ``toupper(string p0)`` returns a copy of the value of the property name 
  specified in ``p0`` converted to uppercase.
* ``trim(string p0)`` returns a copy of the value of the property name specified
  in ``p0`` with leading and trailing whitespace omitted.
* ``concat(string p0, string p1)`` returns a new string that is a concatenation 
  of the string ``p0`` and the string ``p1``.

The following table shows a summary and examples of these functions:

+------------------------------------------------------+-----------------------------------------------------------+
| Function                                             | Example                                                   |
+======================================================+===========================================================+
| ``bool contains(string p0, string p1)``              | ``/actor?$filter=contains(first_name, 'LO')``             |
+------------------------------------------------------+-----------------------------------------------------------+
| ``bool startswith(string p0, string p1)``            | ``/actor?$filter=startswith(first_name, 'JO')``           |
+------------------------------------------------------+-----------------------------------------------------------+
| ``bool endswith(string p0, string p1)``              | ``/actor?$filter=endswith(first_name,'ER')``              |
+------------------------------------------------------+-----------------------------------------------------------+
| ``int indexof(string p0, string p1)``                | ``/actor?$filter=indexof(last_name, 'LO') eq 3``          |
+------------------------------------------------------+-----------------------------------------------------------+
| ``int length(string p0)``                            | ``/actor?$filter=length(first_name) eq 4``                |
+------------------------------------------------------+-----------------------------------------------------------+
| ``string substring(string p0, int pos)``             | ``/actor?$filter=substring(first_name, 2) eq 'RO'``       |
+------------------------------------------------------+-----------------------------------------------------------+
| ``string substring(string p0, int pos, int length)`` | ``/actor?$filter=substring(first_name, 2,3) eq 'TTH'``    |
+------------------------------------------------------+-----------------------------------------------------------+
| ``string tolower(string p0)``                        | ``/actor?$filter=tolower(first_name) eq 'nick'``          |
+------------------------------------------------------+-----------------------------------------------------------+
| ``string toupper(string p0)``                        | ``/actor?$filter=toupper(first_name) eq 'NICK'``          |
+------------------------------------------------------+-----------------------------------------------------------+
| ``string trim(string p0)``                           | ``/actor?$filter=trim(first_name) eq 'JENNIFER'``         |
+------------------------------------------------------+-----------------------------------------------------------+
| ``string concat(string p0, string p1)``              | ``/actor?$filter=concat( concat(first_name,', '),         |
|                                                      | last_name)  eq 'JENNIFER, DAVIS'``                        |
+------------------------------------------------------+-----------------------------------------------------------+
	
Math Functions
--------------

There are three math functions: ``round``, ``floor``, ``ceiling``. Each one 
allows ``Double`` or ``Decimal`` types as parameters and the returned value is 
of the same type as the parameter.

+----------+-----------------------------------------------------------+
| Function | Example                                                   |
+==========+===========================================================+
| round    | ``/film?$filter=round(replacement_cost) eq 21``           |
+----------+-----------------------------------------------------------+
| floor    | ``/film?$filter=floor(replacement_cost) eq 20``           |
+----------+-----------------------------------------------------------+
| ceiling  | ``/film?$filter=ceiling(replacement_cost) eq 21``         |
+----------+-----------------------------------------------------------+

Date Functions
--------------

+------------------------------------+-----------------------------------------------------------+
| Function                           | Example                                                   |
+====================================+===========================================================+
| ``int year(DateTimeOffset p0)``    | ``/actor?$filter=year(last_update) eq 2016``              |
| ``int year(Date p0)``              |                                                           |
+------------------------------------+-----------------------------------------------------------+
| ``int month(DateTimeOffset p0)``   | ``/actor?$filter=month(last_update) eq 12``               |
| ``int month(Date p0)``             |                                                           |
+------------------------------------+-----------------------------------------------------------+
| ``int day(DateTimeOffset p0)``     | ``/actor?$filter=day(last_update) eq 31``                 |
| ``int day(Date p0)``               |                                                           |
+------------------------------------+-----------------------------------------------------------+
| ``int hour(DateTimeOffset p0)``    | ``/actor?$filter=hour(last_update) eq 3``                 |
| ``int hour(TimeOfDay p0)``         |                                                           |
+------------------------------------+-----------------------------------------------------------+
| ``int minute(DateTimeOffset p0)``  | ``/actor?$filter=minute(last_update) eq 34``              |
| ``int minute(TimeOfDay p0)``       |                                                           |
+------------------------------------+-----------------------------------------------------------+
| ``int second(DateTimeOffset p0)``  | ``/actor?$filter=second(last_update) eq 33``              |
| ``int second(TimeOfDay p0)``       |                                                           |
+------------------------------------+-----------------------------------------------------------+
| ``DateTimeOffset now()``           | ``/actor?$filter=last_update lt now()``                   |
+------------------------------------+-----------------------------------------------------------+

Projection: $select
===================

The ``$select`` system query option returns only the properties explicitly 
requested. ``$select`` expression can be a comma-separated lists of properties 
or the star operator (``*``), which will retrieve all the properties.

Example:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$select=actor_id,first_name,last_name

Response:

.. code-block:: none

    {
      "@odata.context":"/denodo-odata.svc/movies/$metadata#actor(actor_id,first_name,last_name)",
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS"
        },
        
        ...
      ]
    }
	
Another example:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$select=*

Response:

.. code-block:: none

    {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor(*)",
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
        },
        
        ...
      ]
    }
	
.. note::

    Complex properties can be used in ``$select`` expressions:
    
    .. code-block:: none

      denodo-odata.svc/admin/struct_table_film?$select=film_data/title

    Response:

    .. code-block:: none

        {
          "@odata.context":"/denodo-odata.svc/admin/$metadata
                                        #struct_table_film(film_data/title)",
          "value": [
            {
              "@odata.id": "/denodo-odata.svc/admin/struct_table_film(1)",
              "film_data": {
                "title": "ACADEMY DINOSAUR"
              }
              
              ...
           }
           ...
           ]
          }
	
Ordering Results: $orderby
==========================

The ``$orderby`` query parameter specifies the order in which items are 
returned:

.. code-block:: none

   /denodo-odata.svc/<database name>/collectionName?$orderby=attribute [asc|desc]

To order the collection the resource path must identify a collection of 
entries, otherwise this option is unavailable.

The keywords ``asc`` and ``desc`` determine the direction of the sort (ascending
or descending, respectively). If ``asc`` or ``desc`` are not specified, items are
returned in ascending order. Null values come before non-null values when 
sorting in ascending order and vice versa.

You can also sort by multiple attributes:

.. code-block:: none

   /denodo-odata.svc/<database name>/collectionName?$orderby=attribute1 [asc|desc],attribute2 [asc|desc]

Example:

.. code-block:: none

   /denodo-odata.svc/movies/address?$orderby=zip,client_identifier desc

Including Related Resources: $expand
====================================
 
An ``$expand`` expression is a comma-separated list of navigation properties 
that specifies the related entities that should be represented inline.

Example:

.. code-block:: none

  /denodo-odata.svc/movies/country?$expand=cities

Response:

.. code-block:: none

    {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#country",
      "value": [
        {
          "country_id": 1,
          "country": "Afghanistan",
          "last_update": "2006-02-15T11:44:00Z",
          "city": [
            {
              "city_id": 251,
              "city": "Kabul",
              "country_id": 1,
              "last_update": "2006-02-15T11:45:25Z"
            }
          ]
        },
    ...
    ]
    }

The following is an example with two navigation properties. This URI identifies 
the film set as well as the ``film_actor`` (``actors`` is the navigation 
property) and the ``film_category`` (``categories`` is the navigation property)
associated with each film:

.. code-block:: none

  /denodo-odata.svc/movies/film?$expand=actors,categories

Response:

.. code-block:: none

    ...
       {
          "film_id": 2,
          "title": "ACE GOLDFINGER",
          "description": "A Astounding Epistle of a Database Administrator And a Explorer who must Find a Car in Ancient China",
          "release_year": "2005-12-31T23:00:00Z",
          "language_id": 1,
          "original_language_id": null,
          "rental_duration": 3,
          "rental_rate": 4.99,
          "length": 48,
          "replacement_cost": 12.99,
          "rating": "G",
          "special_features": "Trailers,Deleted Scenes",
          "last_update": "2006-02-15T12:03:42Z",
          "categories": [
            {
              "film_id": 2,
              "category_id": 11,
              "last_update": "2006-02-15T12:07:09Z"
            }
          ],
          "actors": [
            {
              "actor_id": 19,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z"
            },
            {
              "actor_id": 85,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z"
            },
            {
              "actor_id": 90,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z"
            },
            {
              "actor_id": 160,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z"
            }
          ]
        },
    ...
	
Expanded entities can be filtered, ordered, paged, projected and expanded. 
Allowed system query options are ``$filter``, ``$select``, ``$orderby``, 
``$skip``, ``$top``, ``$count`` and ``$expand``. These expand options are 
expressed as a semicolon-separated list enclosed in parentheses:

.. code-block:: none

  /denodo-odata.svc/movies/film?$expand=categories($select=last_update)
  /denodo-odata.svc/movies/film?$expand=categories($orderby=last_update asc)
  /denodo-odata.svc/movies/film?$expand=actors($count=true)
  /denodo-odata.svc/movies/film?$expand=actors($expand=categories)

Response:

.. code-block:: none

    ...  
      {
          "film_id": 2,
          "title": "ACE GOLDFINGER",
          "description": "A Astounding Epistle of a Database Administrator And a Explorer who must Find a Car in Ancient China",
          "release_year": "2005-12-31T23:00:00Z",
          "language_id": 1,
          "original_language_id": null,
          "rental_duration": 3,
          "rental_rate": 4.99,
          "length": 48,
          "replacement_cost": 12.99,
          "rating": "G",
          "special_features": "Trailers,Deleted Scenes",
          "last_update": "2006-02-15T12:03:42Z",
          "actors": [
            {
              "actor_id": 19,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z",
              "categories": [
                {
                  "film_id": 2,
                  "category_id": 11,
                  "last_update": "2006-02-15T12:07:09Z"
                }
              ]
            },
            {
              "actor_id": 85,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z",
              "categories": [
                {
                  "film_id": 2,
                  "category_id": 11,
                  "last_update": "2006-02-15T12:07:09Z"
                }
              ]
            },
            {
              "actor_id": 90,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z",
              "categories": [
                {
                  "film_id": 2,
                  "category_id": 11,
                  "last_update": "2006-02-15T12:07:09Z"
                }
              ]
            },
            {
              "actor_id": 160,
              "film_id": 2,
              "last_update": "2006-02-15T12:05:03Z",
              "categories": [
                {
                  "film_id": 2,
                  "category_id": 11,
                  "last_update": "2006-02-15T12:07:09Z"
                }
              ]
            }
          ]
        },
    ...
	
Parameter Aliases
=================

Parameter aliases are identifiers prefixed with an ``@`` sign. They can be used
in query expressions to avoid stating the same literal multiple times, or 
deferring lengthy literals to a place where they are easier to read.

Example:
  
.. code-block:: none
  
  /denodo-odata.svc/movies/film?$filter=contains(title,@p1) and not contains(description,@p1)&@p1='ACADEMY DINOSAUR'

Specifying Maximum Number of Results: $top
==========================================

With the ``$top`` option you can select the ``n`` first entries of the 
collection, being ``n`` a non-negative integer:

.. code-block:: none

  /denodo-odata.svc/<database name>/collectionName?$top=n

Example:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$top=1

Response:

.. code-block:: json

    {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor",
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
        }
      ]
    }
	
Specifying Offset: $skip
========================

With the option ``$skip``, the ``n`` first entries of the collection will not be
shown in the response. ``n`` is a non-negative integer:

.. code-block:: none

  denodo-odata.svc/<database name>/collectionName?$skip=n

Example:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$skip=199

Response:

.. code-block:: json

    {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor",
      "value": [
        {
          "actor_id": 200,
          "first_name": "THORA",
          "last_name": "TEMPLE",
          "last_update": "2006-02-15T11:34:33Z"
        }
      ]
    }
	
Asking for Total Result Count: $count
=====================================

The ``$count`` system query option returns the number of items returned in the 
response along with the result.

The old syntax ``$inlinecount=allpages`` has been shortened in OData 4 to
``$count=true``.

The ``$count`` system query option ignores ``$top``, ``$skip``, and ``$expand``
query options, and returns the total count of results across all pages including
only those results matching any specified ``$filter``.

Example:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$count=true

Response:

.. code-block:: none

    {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor",
      "@odata.count": 200,
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
        },
    ...
    ]
    }

Another example:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$count=true&$filter=actor_id eq 1

Response:

.. code-block:: json

    {
      "@odata.context": "/denodo-odata.svc/movies/$metadata#actor",
      "@odata.count": 1,
      "value": [
        {
          "actor_id": 1,
          "first_name": "PENELOPE",
          "last_name": "GUINESS",
          "last_update": "2006-02-15T11:34:33Z"
        }
      ]
    }
	
Another example:

.. code-block:: none

  /denodo-odata.svc/movies/actor?$count=false

Response:

Actor data, just the same as a request without ``$count`` option:

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
    ...
    ]
   }
