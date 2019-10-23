=====
Terms
=====

A query is broken up into terms and operators. There are two types of
terms: single terms and phrases.



A single term is a single word such as "test" or "hello".



A phrase is a group of words surrounded by double quotes such as
"hello world".



Multiple terms can be combined together with Boolean operators to form a
more complex query (see below).



.. note:: The analyzer used to create the index will be used on the terms
   and phrases in the query string. So, it is important to choose an
   analyzer that will not interfere with the terms used in the query string.

