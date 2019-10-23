===========================
Escaping Special Characters
===========================

Lucene supports escaping special characters that are part of the query
syntax. The current special characters list is

.. code-block:: none
   
   + - && \|\| ! ( ) { } [ ] ^ " ~ \* ? : \\



To escape these characters, use the ``\`` before the character. For
example, to search for ``(1+1):2`` use the query:

.. code-block:: none

   \(1\+1\)\:2
