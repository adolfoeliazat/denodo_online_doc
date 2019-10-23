===============================
Commands for Selecting Elements
===============================

List of commands for selecting elements:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

FINDACTIVEELEMENTINDOCUMENT
=========================================

Searches for the active element of the document (i.e. the currently focused 
element).

**Parameters**

-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: When the Sequences Generator Toolbar records an action to send text
   to an element which is not an input field accepting text, nor an editable 
   element (attribute *designMode* not set to *on*, and attribute 
   *contenteditable* not set to *true*), it will generate a click on the element,
   a *FindActiveElementInDocument* command to select the active element and a 
   *SetValue* over that element.
   

FINDCHILDELEMENTBYATTRIBUTE
=========================================

Searches for the child element of the currently selected element by type
and attribute.

**Parameters**

-  String tagname: name of the HTML tag associated with the search
   element (should be in capital letters).
-  String attr\_name: name of the attribute which the element must
   contain (not case-sensitive).
-  String attr\_value: value of the attribute specified by the
   *attr\_name* parameter.
-  int position: index in the list of all elements with the specified
   value for the specified attribute (starting in 0).
-  boolean equals: indicates if the parameter *attr-value* should
   exactly match the attribute value (*true*) or if it is enough for it
   to be contained (*false*).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the specified time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: You can specify various attributes using a sequence of pairs
   *attr\_name,attr\_value*

.. note:: The *attr\_value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDCHILDELEMENTBYPOSITION
=========================================

Searches for the child element of the currently selected element, by
position.

**Parameters**

-  String tagname: name of the HTML tag associated with the element (it
   must be written in upper case).
-  int position: element position.
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.


FINDCHILDELEMENTBYTEXT
=========================================

Searches for the child element of the currently selected element, by
type and text.

**Parameters**

-  String tagname: name of the HTML tag associated with the element (it
   must be written in upper case).
-  String text: text the element must contain.
-  int position: order in all those that are of the indicated type and
   contain the specified text.
-  boolean equals: indicates if the value of the indicated text should
   exactly match that of the element (*true*) or if it is enough that it
   is contained (*false*).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: The *text* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDELEMENTBYATTRIBUTE
=========================================

Searches for an HTML element by type and attribute in the contents of
the current frame.

**Parameters**

-  String tagname: name of the HTML tag associated with the element (it
   must be written in upper case).
-  String attr\_name: name of the attribute to evaluate (not
   case-sensitive).
-  String attr\_value: value of the attribute specified in the
   *attr\_name* parameter.
-  int position: index in the list of all elements with the specified
   value for the specified attribute (starting in 0).
-  boolean equals: indicates if the parameter *attr\_value* should
   exactly match the attribute value (*true*) or if it is enough for it
   to be contained (*false*).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: You can specify various attributes using a sequence of pairs
   *attr\_name*, *attr\_value*

.. note:: If the element found by this command is a form, after the
   execution of the command it will be the selected element. Successive
   commands related to form elements will be executed on this form (as if
   it had been selected with a command *FindFormByXXX*).

.. note:: The *attr\_value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.

.. note:: When the evaluated attribute (*attr\_name*) is *href*,
   selected parameters of the URL can be ignored when trying to match
   *attr\_value* with the links of the page. The parameter names have to be
   separated by the ``>`` character.

Example: When executing:


.. code-block:: none

   FindElementByAttribute(A, HREF>deptNo>empName, ^EncodeSeq(@employee_href), 0, true);

With the value employee\_href =
``http://www.acme.com/GetEmployeesInfo?deptNo=5&empName=John+Smith``

The command will return the first link that matches that HREF attribute,
whatever the values of the ``deptNo`` and ``empName`` parameters are.


FINDELEMENTBYATTRIBUTEREC
=========================================

Searches for an HTML element by type and attribute, recursively
searching in all the frames starting by the current one (deep search).

**Parameters**

-  String tagname: name of the HTML tag associated with the element (it
   must be written in upper case).
-  String attr\_name: name of the attribute to be evaluated (not
   case-sensitive).
-  String attr\_value: value of the attribute specified in the
   *attr\_name* parameter.
-  int position: index in the list of all elements with the specified
   value for the specified attribute (starting in 0).
-  boolean equals: indicates if the parameter *attr\_value* should
   exactly match the value of the attribute (*true*) or if it is enough
   for it to be contained (*false*).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: If the element found by this command is a form, after the
   execution of the command it will be the selected element. Successive
   commands related to form elements will be executed on this form (as if
   it had been selected with a command *FindFormByXXX*).

.. note:: The *attr\_value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDELEMENTBYCHILD
=========================================

Searches for an HTML element by type and attribute of a child element in
the contents of the current frame.

**Parameters**

-  String tagname: name of the tag associated with the HTML parent
   element (it must be written in upper case).
-  String child\_tagname: name of the tag associated with the child HTML
   element.
-  String attr\_name: name of the attribute to evaluate in the child
   element (not case-sensitive).
-  String attr\_value: value of the attribute specified in the
   attr\_name parameter.
-  int position: index in the list of all elements that fulfill the
   specified conditions.
-  boolean recursive\_children: indicates if the search is conducted in
   only first-level children (false) or in any descendant (true).
-  boolean equals: specifies if the parameter attr\_value should exactly
   match the value of the attribute (true) or if it is enough for it to
   be contained in it (false).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: If the element found by this command is a form, after the
   execution of the command it will be the selected element. Successive
   commands related to form elements will be executed on this form (as if
   it had been selected with a command *FindFormByXXX*).

.. note:: The *attr\_value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDELEMENTBYCHILDREC
=========================================

Searches for an HTML element by type and attribute of a child element,
recursively searching through all the frames starting with the current
one (deep search).

**Parameters**

-  String tagname: name of the tag of the HTML element (it must be
   written in upper case).
-  String child\_tagname: name of the tag of the HTML child of the
   element.
-  String attr\_name: name of the attribute to evaluate in the children
   of the element (not case-sensitive).
-  String attr\_value: value of the attribute specified by the
   attr\_name parameter.
-  int position: index in the list of all elements that meet the
   specified conditions (starting in 0).
-  boolean recursive\_children: specifies if the search must be limited
   to the direct children of the element (false) or recursively search
   in any descendant (true).
-  boolean equals: indicates if the parameter ``attr_value`` should
   exactly match the value of the attribute (true) or if it is enough
   that it is contained (false).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to find the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: If the element found by this command is a form, after the
   execution of the command it will be the selected element. Successive
   commands related to form elements will be executed on this form (as if
   it had been selected with a command *FindFormByXXX*).

.. note:: The *attr\_value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDELEMENTBYPOSITION
=========================================

Searches for an HTML element of the type by position in the current
frame.

**Parameters**

-  String tagname: name of the HTML tag associated with the element (it
   must be written in upper case).
-  int position: element position.
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: If the element found by this command is a form, after the
   execution of the command it will be the selected element. Successive
   commands related to form elements will be executed on this form (as if
   it had been selected with a command *FindFormByXXX*).
   

FINDELEMENTBYTEXT
=========================================

Searches for an HTML element by type and text in the contents of the
current frame.

**Parameters**

-  String tagname: name of the HTML tag associated with the element (it
   must be written in upper case).
-  String text: text the element must contain.
-  int position: index in the list of all elements that match the tag
   name and contain the specified text (starting in 0).
-  boolean equals: specifies if the value of the *text* parameter should
   exactly match that of the element (*true*) or if it is enough that it
   is contained (*false*).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.



.. note:: If the element found by this command is a form, after the
   execution of the command it will be the selected element. Successive
   commands related to form elements will be executed on this form (as if
   it had been selected with a command *FindFormByXXX*).

.. note:: The *text* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


FINDELEMENTBYTEXTREC
=========================================

Searches for an HTML element by type and text, recursively searching in
all the frames starting with the current one (deep search).

**Parameters**

-  String tagname: name of the HTML tag associated with the element (it
   must be written in upper case).
-  String text: text the element must contain.
-  int position: index in the list of all elements that match the tag
   name and contain the specified text (starting in 0).
-  boolean equals: indicates if the value of the indicated text should
   exactly match that of the element (*true*) or if it is enough that it
   is contained (*false*).
-  int timeout: optional parameter (it can be omitted). If used, ITPilot
   will execute retries in order to locate the element until it is found
   or until the expressed time (in milliseconds) has passed. This
   parameter is useful when the page may change asynchronously.

**Returns**

   (boolean): *true* if the element was found.

.. note:: If the element found by this command is a form, after the
   execution of the command it will be the selected element. Successive
   commands related to form elements will be executed on this form (as if
   it had been selected with a command *FindFormByXXX*).

.. note:: The *text* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.

.. _itpilot_nseql_guide_findelementbyxpath:

FINDELEMENTBYXPATH
=========================================

Searches for an HTML element in the current document using an XPath
expression.

**Parameters**

-  String xpathExpr: XPath expression to be used to locate the element.
-  int position: optional parameter (it can be omitted). If it is not
   specified and the XPath expression returns more than one element, the
   first will be selected. If it is specified, its value will be used to
   select from the list of nodes returned by the XPath expression.
-  int timeout: optional parameter (it can be omitted). If specified,
   ITPilot will execute retries in order to locate the element until it
   is found or until the specified amount of time (in milliseconds) has
   passed. This parameter is useful when the page may change
   asynchronously.

**Returns**

   (boolean): *true* if the element was found, *false* otherwise.

.. note:: If the element found by this command is a form, it will be the
   selected element after the execution of the command. Subsequent commands
   related to form elements will be executed on this form (as if it had
   been selected with a *FindFormByXXX* command).

.. note:: The syntax of the XPath expressions supported by this command
   is a subset of the XPath 1.0 specification (https://www.w3.org/TR/xpath).

Supported features are:

-  “/” as a step separator. For example, with the XPath expression
   “/html/body/div/a”, the command returns all the links that are child
   of a DIV element that is a child of the body.
-  “//” as a step separator. This is an abbreviation of the
   “descendant-or-self” axis. For example, with the XPath expression
   “//div//a”, the command returns all the links that are descendants of
   DIV elements that are descendants of the root node.
-  “[ ]” to specify predicates that a node must satisfy in order to be
   included in the output of the command. For example, with the XPath
   expression “//a[@title]”, the command returns all the links that have
   a “title” attribute.
-  The node test text() to retrieve text nodes. For example, the XPath
   expression “//div/text()” return all the text nodes that are child of
   a DIV element. Note that the FindElementByXPath can only select HTML
   elements so the previous expression is not a valid expression for use
   with this command, but you can use expressions like “//div[text()]”
   that returns all the nodes that have at least one text node as a
   child. However, the ITPilot functions XPATH and XPATHLIST can return
   text nodes so you can use text() with no restrictions in the
   expressions passed as arguments of these functions.
-  “@” for making reference to an attribute of the context element in a
   predicate. For example, with the XPath expression “//div/a[@title =
   ‘sometitle’]”, the command returns the links that have the attribute
   “title” set to “sometitle” and that are children of any DIV element
   of the document.
-  Operators “and” and “=” to specify conditions in predicates. For
   example, with the XPath expression “//a[@title and
   @class=’someclass’]”, the command returns the links that have the
   “title” attribute set to any value and the “class” attribute set to
   “someclass”.
-  Literals in predicates can be specified between ‘ ‘ or “ “.
-  Relative expressions in predicates. For example, with the XPath
   expression “//div[a]”, the command returns all DIV elements that have
   a child element of type A.
-  “.” to make reference to the context node when using relative
   expressions in predicates. For example, with the XPath expression
   “//div[.//a]”, the command returns all DIV elements that have a child
   element of type A, and “//a[.=‘somelink’]’ returns the links whose
   text is “somelink”.
-  Use of integers to specify context positions. For example, with the
   XPath expression “//div/a[2]”, the command returns the second link of
   each DIV element in the document; but “//div//a[2]” returns the links
   that are children of any DIV element in the document and that are the
   second of the links of their respective parent elements.
-  Function “position()” that returns the position of the context node.
   For example, with the XPath expression “//div/a[position()=2]”, the
   command returns the same as with “//div/a[2]”.
-  Function “last()” that returns the position of the last node in the
   context. For example, with the XPath expression “//div/a[last()]”,
   the command returns the links that are the last link of each DIV
   element in the document.
-  Function “matches(expr, pattern)”, from the XPath 2.0 specification,
   to make comparisons using regular expressions. For example, with the
   XPath expression “//a[matches(@class,’.\*some.\*’)]”, the command
   returns the links in the document that have a “class” attribute whose
   value matches the specified regular expression.

**How positions work in XPath expressions**. Consider the following
example code:

.. code-block:: html

   <html>
      <body>
         <div>
            <a href="" class="someclass">link1</a>
            <a href="" class="someclass">link2</a>
         </div>
         <div>
            <a href="" class="someclass">link3</a>
            <a href="" >link4</a>
            <a href="" class="someclass">link5</a>
         </div>
         <div>
            <a href="" class="someclass">link6</a>
            <a href="" >link7</a>
         </div>
      </body>
   </html>

-  With the XPath expression “//a[1]”, the command returns “link1”,
   “link3” and “link6”
-  With the XPath expression “//a[last()]”, the command returns “link2”,
   “link5” and “link7”
   
   .. note:: With the XPath expression “//a[@class][2]”, the command
      returns “link2” and “link5” but with “//a[2][@class]”, it returns
      only “link2”. The XPath expression “//a[@class][2]” first filters by
      “@class” returning

.. code-block:: html

   ...
   <div>
      <a href="" class="someclass">link1</a>
      <a href="" class="someclass">link2</a>
   </div>
   <div>
      <a href="" class="someclass">link3</a>
      <a href="" class="someclass">link5</a>
   </div>
   <div>
      <a href="" class="someclass">link6</a>
   </div>
   ...


And then filters by [2], so the final output is "link2", "link5". But the XPath expression “//a[2][@class]” first filters by "[2]" returning 

.. code-block:: html

   ...
   <div>
      <a href="" class="someclass">link2</a>
   </div>
   <div>
      <a href=""                          >link4</a>
   </div>
   <div>
      <a href=""                          >link7</a>
   </div>
   ...

And then filters by [@class] so the final output is "link2".

