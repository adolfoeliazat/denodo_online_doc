=========================
Extracting Tag Attributes
=========================

Apart from extracting the text displayed in the HTML page, it is
sometimes useful to extract data from the attributes of HTML tags, such
as, for example, the URL of a link. To extract the value of any
attribute, the following syntax may be used:



.. code-block:: none
   :name: Syntax for HTML attribute extraction
   :caption: Syntax for HTML attribute extraction

   TAG(:NAME=TAG_ATTRIBUTE)

The attributes that can be extracted from the tag are:

-  Any attribute specified in the definition of the tag.
-  Any of the attributes of the HTML tag. These are automatically
   included in the definition of the format tag, even if they are not
   explicitly declared.

In our example, to extract the URL of a link (that is defined in the
href attribute of the HTML tag ‘a’, and thus is implicitly declared in
the ANCHOR tag) into the ‘LINK\_TARGET’ attribute of the relation, the
pattern to be defined is the following:

.. code-block:: none
   :name: Extracting the value of HTML attributes
   :caption: Extracting the value of HTML attributes

   ANCHOR(:LINK_TARGET=HREF)

For more information about extraction of tag attributes, go to the :ref:`next <Tags with Evaluated Attributes>` section.


