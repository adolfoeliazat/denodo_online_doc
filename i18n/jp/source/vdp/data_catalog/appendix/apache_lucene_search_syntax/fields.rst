======
Fields
======

Lucene supports searching by several fields of an index. When performing
a search, you can either specify a field or use the default field. The
field names and default field are implementation-specific.



You can search any field by typing the field name followed by a colon
``:`` and then the term you are looking for.



As an example, let us assume a Lucene index contains two fields,
``title`` and ``text``, and ``text`` is the default field. If you want
to find the document entitled "Jakarta Project", which contains
the text "lucene", you can enter:

.. code-block:: none

   title:"Jakarta Project" AND text:lucene
   
or

.. code-block:: none

   title:"Jakarta Project" AND lucene
   


Since ``text`` is the default field, the field indicator is not
required.



.. note:: The field is only valid for the term that it directly precedes, so
   the query

   .. code-block:: none
   
      title:Jakarta lucene

   will only find "Jakarta" in the ``title`` field. It will find
   "lucene" in the default field (in this case the ``text`` field).