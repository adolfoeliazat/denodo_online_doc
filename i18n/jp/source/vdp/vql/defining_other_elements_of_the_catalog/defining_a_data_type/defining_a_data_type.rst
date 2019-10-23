====================
Defining a Data Type
====================

The Virtual DataPort catalog incorporates a series of predefined data
types (see section :ref:`Data Types`). As already mentioned, the data types
included can be divided into two groups:

#. Basic types
#. Compound types.

Virtual DataPort allows new compound data types to be defined through
the statement ``CREATE TYPE``. With this statement you can create the following data
types:

-  array
-  register

The section :ref:`Management of Compound Values` explains in detail
how to handle the compound types ``array`` and ``register``.

.. code-block:: bnf
   :caption: Syntax of the CREATE TYPE statement
   :name: Syntax of the CREATE TYPE statement

   CREATE [ OR REPLACE ] TYPE <name:identifier> AS { <array> | <register>
   }

   <array> ::=
       ARRAY OF <register>

   <register> ::=
       REGISTER OF ( <register element> [, <register element> ]* )

   <register element> ::=
       <name:identifier>:<type:identifier>
       [ ( <property name> = <property value> ) ]


If the ``type`` of a field of a register is ``blob``, you can indicate
the content type of the field with the property ``contenttype``. The
value of the property can be either a constant (e.g.
``application/pdf``) or an expression. See more about this in the
section :ref:`Working with Blob Fields of Base Views` of the Administration
Guide.

To create a new ``register`` type, you need to specify the name and data
type of the elements it contains. In `Creating a register data type`_ a
``register`` data type is created that contains personal data: name
(attribute ``NAME`` of type ``text``), surname (attribute ``SURNAME`` of
type ``text``), telephone (attribute ``PHONE`` of type ``arrayPhone``),
salary (attribute ``PAY`` of type ``decimal``) and birthday (attribute
``BIRTH`` of type ``date``).



.. code-block:: vql
   :caption: Creating a register data type
   :name: Creating a register data type

   CREATE TYPE registerPersonalData AS REGISTER OF (
       NAME:text,
       SURNAME:text,
       PHONE:arrayPhone,
       PAY:decimal,
       BIRTH:date
   );


When defining an ``array`` data type the name of the ``register``
type of the elements it contains must be indicated. In `Creating a data
type array and the register type it contains`_ the ``array`` data type
called ``arrayPhone`` is created, which encapsulates a list of
telephones, where each telephone is represented by an integer. Each
element of ``arrayPhone`` is a ``register`` of type ``registerPhone``.
As you can see, the type ``registerPhone`` encapsulates an element of
the type ``int`` called ``number``.



.. code-block:: vql
   :caption: Creating a data type array and the register type it contains
   :name: Creating a data type array and the register type it contains

   CREATE TYPE registerPhone AS REGISTER OF (
       NUMBER:int
   );

   CREATE TYPE arrayPhone AS ARRAY OF registerPhone;


The use of the ``OR REPLACE`` modifier specifies that, if there is a
type with the name indicated, the existing type will be replaced by the
new type. If the new type is not compatible with the previous one, the
views depending on this type or on any other type using them in its
definition will be marked as erroneous. The types are considered
compatible if the fields of the new type are a “superset” of the fields
of the previous type. I.e. the new type may add new fields but must
maintain the previous ones of the same name and type.

