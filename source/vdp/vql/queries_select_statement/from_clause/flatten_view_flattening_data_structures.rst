=========================================
FLATTEN View (Flattening Data Structures)
=========================================

Virtual DataPort supports modeling hierarchical data types through the
use of the types ``register`` and ``array`` (see section :ref:`Management of
Compound Values`).

In Virtual DataPort, you may think of an ``array`` element as a subview.
An ``array`` always has a ``register`` internally associated and each
subelement of the array belongs to this ``register``. Hence, the fields
of this register may be seen as the schema of the subview being modeled.

You usually need to create Flatten views when processing XML or Web
service sources because they usually return compound fields. This
section describes how to do this.

Imagine that we have a Web service with a ``getAverageMonthlySales``
operation that does not have input parameters and returns an array of
objects with the monthly sales of all the clients of a company. Each
element of the array has two properties: ``taxId`` and ``revenue``.

The base views created over this operation have one attribute of type
``array``, which contains ``register``-type elements and only one row
that contains all the data returned by the Web service.

In order to combine these data with data from other sources, we usually
have to apply the “flatten” operation over the result, in order to have
a view with two attributes (``taxId`` and ``revenue``) and one tuple for
each client.

In the ``FROM`` clause, you can use the constructor ``FLATTEN`` to
define queries over “flattened” views. The constructor ``FLATTEN`` (see
:ref:`below <Syntax of the FLATTEN operation>`) generates tuples from the compound
subfields of an array of a view.

.. code-block:: bnf
   :caption: Syntax of the FLATTEN operation
   :name: Syntax of the FLATTEN operation

   <flatten view> ::=
         FLATTEN ( <view name:identifier>[.<register field>]*.<array field> )
       | FLATTEN ( <view name:identifier> AS <alias>
             [, <alias>[.<register field>]*.<array field> AS <alias> ]*
             /, <alias>[.<register field>]*.<array field> )


As we can see in the figure above, there are two ways of using the
constructor ``FLATTEN``:

#. Specifying the name of an attribute of type ``array``. The output is
   a view which has the schema of the register contained in the array
   passed as parameter. The specified array can be inside one register
   (or even several nested registers), but it cannot be nested inside
   another array.

   .. code-block:: bnf
      :name: Flatten operation: syntax 1
      :caption: Flatten operation: syntax 1
   
      <flatten view> ::= 
      FLATTEN ( <view name:identifier>[.<register field>]*.<array field> )

#. Specifying the name of a view and an alias. The output is the
   flattened representation of an array (even if it is nested inside
   another array) and the other fields of the view.

   The syntax is specified by indicating an alias for the source view and the array element on which the FLATTEN operation has to be applied.   
   
   To apply to an array that is nested inside another, you have to add an alias to the parent array.
   
   To specify which array you want to flatten, you have to indicate a path from the alias.
   
   The resulting schema contains the fields of the source view (except the array on which the operation is carried out) and all the elements of the registers involved in the flattening operation.

   .. code-block:: bnf
      :name: Flatten operation: syntax 2
      :caption: Flatten operation: syntax 2
      
      <flatten view> ::= 
         FLATTEN ( <view name:identifier> AS <alias>
                  [, <alias>[.<register field>]*.<array field> AS <alias> ]*
                  , <alias>[.<register field>]*.<array field> )
      
.. rubric:: Example

Imagine that we have the base view ``AVERAGE_REVENUE_ARRAY``
that has a field of type array of registers called ``RETURN``. Each
register contains two fields: ``TAXID`` and ``REVENUE``. The following
statement returns the “flattened” contents of ``AVERAGE_REVENUE_ARRAY``:

.. code-block:: sql

   SELECT TAXID, REVENUE 
   FROM FLATTEN (AVERAGE_REVENUE_ARRAY AS V, V.RETURN)