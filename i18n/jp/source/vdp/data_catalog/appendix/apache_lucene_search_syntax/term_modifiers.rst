==============
Term Modifiers
==============

Lucene supports modifying query terms to provide a wide range of
searching options.



-  Wildcard Searches
   Lucene supports single and multiple character wildcard searches.
   
   To perform a single character wildcard search, use the ``?`` symbol.
   
   To perform a multiple character wildcard search, use the ``*`` symbol.
   
   The single character wildcard search looks for terms that match that with 
   the single character replaced. For example, to search for "text" or "test" 
   you can use the search:


   .. code-block:: none
   
      te?t

   Multiple character wildcard searches look for 0 or more characters. For example, to search for "test", "tests" or "tester" you can use the search: 

   .. code-block:: none
   
      test*

   You can also use the wildcard searches in the middle of a term.


   .. code-block:: none
   
      te*t

   .. note:: You cannot use a ``*`` or ``?`` symbol as the first character of a search.

-  Fuzzy Searches


   Lucene supports fuzzy searches based on the Levenshtein Distance 
   or Edit Distance algorithm. To do a fuzzy search use the tilde 
   (swung dash), ``~``, symbol at the end of a single word Term. 
   For example, to search for a term similar in spelling to "roam" 
   use the fuzzy search: 

   .. code-block:: none
   
      roam~

   This search will find terms like foam and roams.
   
   An additional (optional) parameter can specify the required similarity. 
   The value is between 0 and 1, with a value closer to 1 only terms with a 
   higher similarity will be matched. For example:

   .. code-block:: none
   
      roam~0.8

   The default that is used, if the parameter is not given, is 0.5.

-  Proximity Searches

   Lucene supports finding words that are within a specific distance away. 
   To do a proximity search, use the tilde, ``~``, symbol at the end of a phrase. 
   For example, to search for "apache" and "jakarta" within 10 words of 
   each other in a document use the search: 


   .. code-block:: none
   
      "jakarta apache"~10

-  Range Searches


   Range Queries allow one to match documents whose field(s) values are between 
   the lower and upper bound specified by the Range Query. Range Queries can 
   be inclusive or exclusive of the upper and lower bounds. Sorting is done 
   lexicographically.

   .. code-block:: none
   
      mod\_date:[20020101 TO 20030101]

   This will find documents whose mod_date fields have values between 20020101 and 20030101, inclusive. Note that Range Queries are not reserved for date fields. You could also use range queries with non-date fields:

   .. code-block:: none
   
      title:{Aida TO Carmen}

   This will find all documents whose titles are between "Aida" and "Carmen", but not including "Aida" and "Carmen".
   
   Inclusive range queries are denoted by square brackets. Exclusive range queries are denoted by curly brackets.

-  Boosting a Term

   Lucene provides the relevance level of matching documents based on the terms found. 
   To boost a term, use the caret, ``^``, symbol with a boost factor (a number) at the end of the term you are searching. The higher the boost factor, the more relevant the term will be.
   
   Boosting allows you to control the relevance of a document by boosting its term. For example, if you are searching for

   .. code-block:: none
   
      jakarta apache

   and you want the term "jakarta" to be more relevant, boost it using the ``^`` symbol 
   along with the boost factor next to the term. You would type:

   .. code-block:: none
   
      jakarta^4 apache

   This will make documents with the term jakarta appear more relevant. 
   You can also boost Phrase Terms as in the example: 

   .. code-block:: none
   
      "jakarta apache"^4 "jakarta lucene"

   By default, the boost factor is 1. Although the boost factor must be positive, it can be less than 1 (e.g. 0.2)