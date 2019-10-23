=================
Boolean Operators
=================

Boolean operators allow terms to be combined through logic operators.
Lucene supports AND, +, OR, NOT and - as Boolean
operators (Boolean operators must be ALL CAPS).

-  OR

   The OR operator is the default conjunction operator. This means that if there is no 
   Boolean operator between two terms, the OR operator is used. The OR operator 
   links two terms and finds a matching document, if either of the terms exists in a document.
   This is equivalent to a union using sets. The symbol ``||`` can be used in place of the word OR.
   
   To search for documents that contain either "jakarta apache" or just "jakarta" use the query:

   .. code-block:: none
   
      "jakarta apache" jakarta

   or 
   
   .. code-block:: none
   
      "jakarta apache" OR jakarta
      

-  AND

   The AND operator matches documents, where both terms exist, anywhere in the text of a single document. This is equivalent to an intersection using sets. The symbol ``&&`` can be used in place of the word AND.
   
   To search for documents that contain "jakarta apache" and "jakarta lucene" use the query: 

   .. code-block:: none
   
      "jakarta apache" AND "jakarta lucene"  

-  \+


   The '+' or required operator requires that the term after the '+' symbol exist somewhere in the field of a single document.
   
   To search for documents that must contain "jakarta" and may contain "lucene" use the query:

   .. code-block:: none
   
      +jakarta apache

-  NOT

   The NOT operator excludes documents that contain the term after NOT. This is equivalent to a difference using sets. The symbol ``!`` can be used in place of the word NOT.
   
   To search for documents that contain "jakarta apache" but not "jakarta lucene" use the query: 

   .. code-block:: none
   
      "jakarta apache" NOT "jakarta lucene"


   .. note:: The NOT operator cannot be used with just one term. For example, the following search will return no results:
   
   .. code-block:: none
   
      NOT "jakarta apache"

-  \-

   The '-' or prohibit operator excludes documents that contain the term after the ``-`` symbol.
   
   To search for documents that contain "jakarta apache" but not "jakarta lucene" use the query: 
   
   .. code-block:: none
   
      "jakarta apache" -"jakarta lucene"
