===================================
Requests with Input Compound Values
===================================

The Denodo web services provide two ways of adding a condition by a
compound field:


#. Using the regular syntax of VQL, which is something like this:


.. code-block:: vql

   SELECT ... 
   FROM V 
   WHERE input_array_field = { ROW( 1, NULL, 2, NULL, 'a', true, NULL, NULL, NULL
                               , NULL, NULL, NULL, NULL ) }

..

   The URL below uses this syntax to indicate a condition with the array
   field ``input_array_field``:

.. code-block:: none

   http://acme:9090/denodo-restfulws/.../views/V?input_array_field={ ROW( 1, NULL, 2, NULL, 'a', true, NULL, NULL, NULL, NULL, NULL, NULL, NULL ) }

..

   The value
   ``{ ROW( 1, NULL, 2, NULL, 'a', true, NULL, NULL, NULL, NULL, NULL, NULL, NULL ) }``
   represents an array of a register. The braces surrounding ``ROW(...)``
   indicate that the value is an array. Without the braces, it would be a
   register.
   
   The section :ref:`Management of Compound Values` of the VQL Guide
   describes this syntax.


2. The other syntax to express conditions with compound values is using a
   JSON document. For example:

.. code-block:: none

   http://acme:9090/denodo-restfulws/.../views/view1?$filter=input_array_field='[{"id":1, "id2": 2, "field1": "a", "field2": true}]'

..

   The two URLs above are equivalent, but this one is simpler because:
   
   a. You indicate the name of the fields in the register, which makes the
      condition more readable than in the first syntax where you do not
      know the field that correspond to each value.
   b. If a field is not present in the JSON document, it means that its
      value is NULL. The benefit of this is that if you have a lot of NULL
      values, the value of the parameter is much shorter.
   
   In JSON, the register values start and end with braces. For example:

.. code-block:: none

   http://acme:9090/denodo-restfulws/.../views/view1?$filter='register_value={"f1":"value", "boolean_value": true}'                                      

..

   In JSON, the array values start and end with brackets and you separate
   each register of the array with comma. For example:

.. code-block:: none

   http://acme:9090/denodo-restfulws/.../views/view1?$filter='array_value=[{"f1":"value", "boolean_value": true}', {"f1":"value2", "boolean_value": false}']

.. important:: The URLs above are not escaped to make the examples 
   clearer. However, take into account that the HTTP clients have to escape
   the values of all the parameters. For example, if you want to invoke
   this URL:

.. code-block:: none

   http://acme:9090/denodo-restfulws/.../views/view1?$filter='array_value=[{"f1":"value", "boolean_value": true}', {"f1":"value2", "boolean_value": false}']

You have to escape it like this:

.. code-block:: none

   http://acme:9090/denodo-restfulws/.../views/view1?$filter=%27array_value=[{%22f1%22:%22value%22,%20%22boolean_value%22:%20true}%27,%20{%22f1%22:%22value2%22,%20%22boolean_value%22:%20false}%27]

**Restrictions of the JSON syntax**

-  You have to provide the JSON document in the ``$filter`` parameter,
   not as a value of the field. I.e. ``?field_name='<json document>'``
   does not work.
-  Make sure you surround the JSON document in single quotes and you
   escape the single quotes in the JSON document. To escape a single
   quote, prefix it with another single quote.
