=====================
Understanding Tagsets
=====================

Each level in the structure of the data to extract with an Extractor
component (see section :ref:`Extractor`) has an associated tagset. A scanner
is just a set of tagsets that can be used inside of the same Extractor
component: if we want to use tagset A and tagset B in the same
Extractor, we need to have a scanner with at least both tagsets.



A tag can be seen as a regular expression that matches an HTML element
appearing in the page which data is to be extracted. As its name
indicates, a tagset is a group of tags. When ITPilot applies a tagset on
a page, it ignores all HTML elements that do not match any tag in the
set. Besides, it considers tags to act as *separators* between data
elements so, therefore, it assumes that they cannot appear inside the
value of a field to extract.



**Example**: suppose that an electronic bookstore shows the authors of
any book by separating them with a carriage return, using a **<BR>** tag:



**John Smith, <BR> Peter Jones**



In this situation, we want to extract the name of all authors in a
single field of our Extractor structure (‘John Smith, Peter Jones’); to
accomplish it, the tagset used cannot include any tag that matches the
**<BR>** tag.



On the other hand, adding more tags to a set is useful in the following
situations:



#. We need to make the extraction process more specific. When an
   Extractor extracts more data than desired, adding tags to the set
   might be the solution.
#. We wish to extract the attribute value of an HTML element that does
   not match any other tag in our set. The Extractor component only
   allows extracting attributes from HTML elements that match tags of
   the set that is being used (see section :ref:`Configuration of the
   Extractor Component`).

