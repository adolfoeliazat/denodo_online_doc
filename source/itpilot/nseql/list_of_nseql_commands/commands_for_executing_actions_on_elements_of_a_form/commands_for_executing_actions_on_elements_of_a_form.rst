====================================================
Commands for Executing Actions on Elements of a Form
====================================================

List of commands for executing actions on elements of a form:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols


CLICKONELEMENT
=========================================

Generates a click on an element of a form.

Elements of type “INPUT type=image” are not taken into account as
INPUT-type elements (they are not returned in the INPUT collection of a
form). The only way to access those are by name. This must also be had
in mind when accessing another input by position, since IMAGE-type
elements will not be counted.

**Parameters**

-  String name: name of the element (OPTIONAL, use \_NULL\_ if no value
   is required).
-  String type: element type.
-  int position: index in the list of all elements of the current form
   with the same name (if the name parameter is specified) and that are
   of the indicated type (if type parameter is specified). When the
   Microsoft Internet Explorer browser is used, the command searches for
   the element in the currently selected form, and, if there is no
   selected form, in the current frame.

**Returns**

   (boolean): *true* if the click was performed correctly.


CLICKONINPUTELEMENT
=========================================

Simulates a click on an INPUT element of the last selected form.

Elements of type “INPUT type=image” are not taken into account as
INPUT-type elements (they are not returned in the INPUT collection of a
form). The only way to access those are by name. This must also be had
in mind when accessing another input by position, since IMAGE-type
elements will not be counted.

**Parameters**

-  String name: name of the element (OPTIONAL, use \_NULL\_ if no value
   is required).
-  String type: element type.
-  int position: index in the list of all elements of the current form
   with the same name (if the name parameter is specified) and with the
   same value. When the Microsoft Internet Explorer browser is used, the
   command searches for the element in the currently selected form, and,
   if there is no selected form, in the current frame.

**Returns**

   (boolean): *true* if the click was performed correctly.


FIREEVENT
=========================================

Fires an event on an element of a form.

**Parameters**


-  String event: name of the event (e.g. onClick, onMouseOver, …). The
   comprehensive list of all the events that can be issued can be found in
   the `Microsoft Developer
   Network <https://msdn.microsoft.com/>`_ (MSDN) web site under the section “DHTML events” of the
   Library. To simulate mouse button interaction, the following modifiers
   can be added next to the event name:

      \|L\| : fires the event as if the left mouse button was pressed.

      \|R\| : fires the event as if the right mouse button was pressed.

   For example,
   
   onmouseover\|L\| will fire a mouseover event with left button pressed
   
   onmousedown\|R\| will fire a mousedown event with right button pressed

-  String type: element type.

-  String name: name of the element.

-  int position: index in the list of all elements of the current form with
   the same name and that are of the indicated type. When the Microsoft
   Internet Explorer browser is used, the command searches for the element
   in the currently selected form, and, if there is no selected form, in
   the current frame.


**Returns**

   (boolean): *true* if the event has fired correctly.


SELECTINDEXBYPOSITION
=========================================

Selects one of the options of a ``select`` field of a form by position
and fires the event onChange.

**Parameters**

-  String name: select name (OPTIONAL, use \_NULL\_ if no value is
   required).
-  int position: index in the list of all select fields with the same
   name (if the name parameter is specified) or index in the list of all
   select fields of the current form (if it is not specified).
-  int position\_option: position of the value to be selected in the
   select field. When the Microsoft Internet Explorer browser is used,
   the command searches for the element in the currently selected form,
   and, if there is no selected form, in the current frame.

**Returns**

   (boolean): *true* if the option was selected correctly.


SELECTINDEXBYTEXT
=========================================

Selects an option from a ``select`` field by text and fires the event
``onChange``.

**Parameters**

-  String name: select name (OPTIONAL, use \_NULL\_ if no value is
   required).
-  int position: index in the list of all select fields with the same
   name (if the name parameter is specified) or index in the list of all
   select fields of the current form (if it is not specified). When the
   Microsoft Internet Explorer browser is used, the command searches for
   the element in the currently selected form, and, if there is no
   selected form, in the current frame.
-  String text: text associated with the option to be selected; if the
   text is \_NULL\_, the command will have no effect.
-  int position\_option: index in the list of all options with the same
   associated text.
-  boolean equals: indicates if the text specified as a parameter should
   exactly match the required value of the select (true) field or if it
   is enough that it is contained in it (false).

**Returns**

   (boolean): *true* if the option was selected.

.. note:: The *text* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


SELECTINDEXBYVALUE
=========================================

Selects an option from a *select* field text by the “option” value and
fires the event onChange.

**Parameters**

-  String name: select name (OPTIONAL, use \_NULL\_ if no value is
   required)
-  int position: index in the list of all select fields with the same
   name (if the name parameter is specified) or index in the list of all
   select fields of the current form (if it is not specified). When the
   Microsoft Internet Explorer browser is used, the command searches for
   the element in the currently selected form, and, if there is no
   selected form, in the current frame.
-  String value: text associated with the option to be selected; if the
   text is \_NULL\_,the command will have no effect.
-  int position\_option: index in the list of all options with the same
   associated text.
-  boolean equals: indicates if the text specified as a parameter should
   exactly match the required value of the select (true) field or if it
   is enough that it is contained in it (false).

**Returns**

   (boolean): *true* if the option was selected.

.. note:: The *value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


SELECTOPTIONBYPOSITION
=========================================

Selects one option among those contained in a <select> element. The
<select> element must have been singled out beforehand by a command like
``FindElementByXXX`` or ``FindChildElementByXXX``. Unlike the
``SelectIndexByPosition`` command, this command does not fire
``onChange`` events.

**Parameters**

-  int position\_option: position in the *select* field of the value to
   be selected.

**Returns**

   (boolean): ``true`` if the option was correctly selected.

.. note:: You can select more than one option in multiple-option
   ``<select>`` elements using a sequence of ``positions``. For example:
   ``SelectOptionByPosition(1,4,5)``.


SELECTOPTIONBYTEXT
=========================================

Selects one option among those contained in a <select> element. The
<select> element must have been singled out beforehand by a command like
*FindElementByXXX* or *FindChildElementByXXX*. Unlike the
*SelectIndexByText* command, this command does not fire onChange events.

**Parameters**

-  String text: text associated with the option to be selected; if the
   text is \_NULL\_, the command will have no effect.
-  int position\_option: index in the list of all options with the same
   associated text.
-  boolean equals: indicates if the text specified as a parameter should
   exactly match the required value of the select (true) field or if it
   is enough that it is contained in it (false).

**Returns**

   (boolean): *true* if the option was selected.

.. note:: The *text* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.

.. note:: You can select more than one option in multiple-option
   ``<select>`` elements using a sequence of ``text``, ``position`` pairs.
   For example: SelectOptionByText(Spain,0,USA,0,true).


SELECTOPTIONBYVALUE
=========================================

Selects one option among those contained in a <select> element. The
<select> element must have been singled out beforehand by a command like
``FindElementByXXX`` or ``FindChildElementByXXX``. Unlike the
``SelectIndexByValue`` command, this command does not fire
``onChange`` events.

**Parameters**

-  String value: text associated with the option to be selected; if the
   text is \_NULL\_, the command will have no effect.
-  int position\_option: index in the list of all options with the same
   associated text.
-  boolean equals: indicates if the text specified as a parameter should
   exactly match the required value of the select (true) field or if it
   is enough that it is contained in it (false).

**Returns**

   (boolean): *true* if the option was selected.

.. note:: The *value* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.

.. note:: You can select more than one option in multiple-option
   ``<select>`` elements, using a sequence of ``value``, ``position``
   pairs. For example: SelectOptionByValue(somevalue,0,othervalue,0,true).


SETINPUTVALUE
=========================================

Establishes a value for an ``input`` field of a form.

**Parameters**

-  String name: name of the input field (OPTIONAL, use \_NULL\_ if no
   value is required).
-  int position: index in the list of all input fields with the same
   name (if the name parameter is specified) or index in the list of all
   input fields of the current form (if it is not specified). When the
   Microsoft Internet Explorer browser is used, the command searches for
   the element in the currently selected form, and, if there is no
   selected form, in the current frame.
-  String value: value to be set in the field; if the value is \_NULL\_
   ,the command will have no effect.

**Returns**

   (boolean): *true* if the value has been established correctly.

.. note:: The *href* parameter accepts `Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.


SETTEXTAREAVALUE
=========================================

Establishes the value of a ``textarea`` field.

**Parameters**

-  String name: name of the textarea field (OPTIONAL, use \_NULL\_ if no
   value is required).
-  int position: index in the list of all textarea fields with the same
   name (if the name parameter is specified) or index in the list of all
   textarea fields of the current form (if it is not specified). When
   the Microsoft Internet Explorer browser is used, the command searches
   for the element in the currently selected form, and, if there is no
   selected form, in the current frame.
-  String value: value to be set in the field; if the value is \_NULL\_
   ,the command will have no effect.

**Returns**

   (boolean): *true* if the value has been established correctly.


SETVALUE
=========================================

Sets the value of editable HTML text-elements (<*text*>, 
<*textarea*>, <*number*>, <*email*>, <*search*>, <*tel*> or <*url*>), 
previously selected with a command that locates an element in the current page 
(*FindElementByXXX*, *FindChildElementByXXX*, *FindActiveElementOnDocument*). 
It can also be used to send a text value to non editable HTML elements.

Parameters:

-  String value: value to be set in the field (OPTIONAL, use \_NULL\_ if
   no value is required; the command will have no effect). This
   parameter can be set in an encrypted form like
   “encrypted:/QBQilPalsqjQpGOj4Ek8Q==”. To encrypt a clear value the
   Generation ToolBar to record the setText action on a password field
   or use the encryption facilities of the Sequence Editor in Wrapper
   Generator Tool.

**Returns**

   (boolean): *true* if the value has been established correctly.

.. note:: The empty string can be specified as a value.


SETCHECKED
=========================================

Checks or unchecks a <input type=”checkbox”> HTML element, previously
selected with a command that locates an element in the current page
(FindElementByXXX, FindChildElementByXXX), by clicking on it if
necessary.

**Parameters**

-  boolean check: if this parameter’s value is *true*, the command will
   check the check box element and if the value is *false* it will
   uncheck it. ITPilot will click on the element in order to change the
   state of the check box only if the current state is different from the
   state being set, firing the html events “onmousemove”, “onmouseover”,
   “onmousedown”, “onmouseup”, “onclick”.
-  boolean wait: optional attribute (it can be omitted). After the
   events generation, this parameter specifies whether ITPilot must wait
   for all event handlers associated to the element to finish their
   executions.
-  int asyncRequests: optional attribute (it can be omitted), allows to
   configure how many asynchronous AJAX request executions the command
   must wait before control is returned to ITPilot.

**Returns**

   (boolean): *true* if executed correctly, *false* otherwise.


SUBMITFORM
=========================================

Generates a submit event on the selected form.

**Parameters**

-  int pages: number of download pages to be waited for after
   submitting.

**Returns**

   (int): time elapsed loading the pages, in milliseconds.


UNSELECTALL
=========================================

Unselects all the options of the indicated ``select`` field.

**Parameters**

-  String name: select name (OPTIONAL, use \_NULL\_ if no value is
   required).
-  int position: index in the list of all select fields with the same
   name (if the name parameter is specified) or index in the list of all
   select fields of the current form (if it is not specified). When the
   Microsoft Internet Explorer browser is used, the command searches for
   the element in the currently selected form, and, if there is no
   selected form, in the current frame.

**Returns**

   (boolean): *true* if executed correctly, otherwise *false*.