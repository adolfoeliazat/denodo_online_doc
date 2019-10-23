==============================
Tags with Evaluated Attributes
==============================

In some cases it is useful to be able to define a pattern that matches
the HTML code only in case the HTML tags have a specific value for some
of their attributes. For example, given an HTML page that defines a
select element:


.. code-block:: html
   :name: HTML page with a select
   :caption: HTML page with a select

   <select name="format">
       <option value=" Hardcover ">Hardcover</option>
       <option value=" Paperback " selected>Paperback</option>
    </select>

The value of the selected option can be extracted. To do that, a new
custom option tag can be defined to match only the option element that
includes the selected attribute; but a simpler syntax can be used, thus
avoiding the creation of a new tagset. When defining a DEXTL pattern
that includes a tag, the values of the attributes of the tag can be
added after the tag, in the following form:

.. code-block:: none
   :name: Syntax for the evaluated attributes
   :caption: Syntax for the evaluated attributes

   TAG("value"==attribute)

This way, to obtain the selected option from the page the pattern should
be this one

.. code-block:: none
   :name: Syntax for the pattern that matches the selected option
   :caption: Syntax for the pattern that matches the selected option

   OPTION(""==SELECTED)

It can be seen how the definition of a new tag and tagset has been
avoided in this case, by using this syntax.

Tag attributes can be checked at the same time, by separating them with
commas. In the following example, the pattern matches the HTML code
where the value of the option is " Paperback " AND where the option has
been selected. Therefore, there will be just one match (if the selected
option is a " Paperback " item) or none (if the selected option has
another *value* attribute):

.. code-block:: none
   :name: Syntax for the pattern that matches the selected option, but only if its value is ‘Paperback’
   :caption: Syntax for the pattern that matches the selected option, but only if its value is ‘Paperback’

   OPTION(" Paperback "==value, ""==SELECTED)

Also, this can be combined with the attribute extraction syntax
discussed in the section :ref:`Extracting tag attributes`. For example, to
extract the value (not the text) of the selected option into the FIELD1
attribute:

.. code-block:: none
   :name: Syntax for the pattern that extracts the selected value
   :caption: Syntax for the pattern that extracts the selected value

   OPTION(:FIELD1=value, ""==SELECTED)
