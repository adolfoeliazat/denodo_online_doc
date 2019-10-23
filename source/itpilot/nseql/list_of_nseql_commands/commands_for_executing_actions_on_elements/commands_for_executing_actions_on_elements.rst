=============================================
Commands for Executing on Actions on Elements
=============================================

List of commands for executing on actions on elements:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

CLICKONSELECTEDELEMENT
==========================================

Generates a click on the selected element.

**Parameters**

-  boolean wait: optional attribute (it can be omitted). After the
   generation of the event, this parameter specifies whether ITPilot
   must wait for all event handlers associated to the element to finish
   their executions or not.
-  int asyncRequests: optional attribute (it can be omitted), allows to
   configure how many asynchronous AJAX request executions the command
   must wait before control is returned to ITPilot.

**Returns**

   (boolean): *true* if the click was correct


FIREEVENTONSELECTEDELEMENT
==========================================

Emits an event on the selected element.

**Parameters**


-  String event: The comprehensive list of all the events that can be
   issued can be found in the `Microsoft Developer
   Network <https://msdn.microsoft.com/>`_ (MSDN) under the section “DHTML events”
   of the Library.

   To simulate mouse button interaction, the following modifiers can be
   added next to the event name:
   
      \|L\| : fires the event as if the left mouse button was pressed.
   
      \|R\| : fires the event as if the right mouse button was pressed.
      
   For example,
   
      onmouseover\|L\| will fire a mouseover event with left button pressed
   
      onmousedown\|R\| will fire a mousedown event with right button pressed



   To scroll the selected element, it is also possible to add a modifier to
   specify the scrolling direction:

      onscroll\|D\| : scrolls the element to its bottom position. In this
      case the modifier can be omitted.
   
      onscroll\|U\| : scrolls the element to its top position.
   
      onscroll\|L\| : scrolls the element to its leftmost position.
   
      onscroll\|R\| : scrolls the element to its rightmost position.

-  boolean wait: optional attribute (it can be omitted). After the
   generation of the event, this parameter specifies whether ITPilot must
   wait for all event handlers associated to the element to finish their
   executions or not.

-  int asyncRequests: optional attribute (it can be omitted), allows to
   configure how many asynchronous AJAX request executions the command must
   wait before control is returned to ITPilot.

-  boolean compatibilityMode: optional attribute (it can be omitted). With
   MSIE 9 this parameter specifies if ITPilot has to fire the event using
   the proprietary method *fireEvent* or the standard method
   *dispachtEvent* (added in version 9). In MSIE 9, when an event handler
   has been added to a page element using the proprietary method
   *attachEvent*, then the event should be fired using the proprietary
   method *fireEvent*. On the contrary, when the handler has been added
   using the standard method *addEventListerner*, then it should be fired
   using the standard method *dispatchEvent*. If the *compatibilityMode*
   parameter is omitted or if its value is *false*, ITPilot will
   automatically choose between the *dispatchEvent* method and *fireEvent*
   method, depending on the current document mode of the page loaded in the
   browser (the document mode tells MSIE which version features should be
   used to render the page: MSIE5, MSIE7, MSIE8 or MSIE9). This behavior is
   valid for the majority of web pages because they normally check the
   browser type and version and use the standard methods to handle events
   when MSIE 9 is used (or other browsers supporting the standard methods,
   like for example Firefox), and the MSIE proprietary methods with
   previous versions of MSIE. When the *compatibilityMode* parameter is set
   to *true*, it forces to use the MSIE proprietary methods with MSIE 9,
   which can be useful with pages designed to work only with previous
   versions of MSIE (i.e. if they always use the MSIE proprietary methods
   to handle events without checking the browser type and version).

**Returns**

   (boolean): *true* if the event was issued correctly.

.. note:: You can specify various events using a sequence of event names
   separated by the “>” character. For example, the command:

      FireEventOnSelectedElement(onmousemove>onmouseover>onmousedown>onmouseup>onclick);

   It fires the events in the list in the specified order.

.. note:: The onscroll event only takes effect when fired on scrollable
   elements. Those are the HTML element and any DIV elements with the CSS
   property “overflow” set to “scroll” or “auto”. A command firing the
   onscroll event will fail if the page is already scrolled at the target
   position (i.e.: if firing an onscroll\|D\| and the page is at its bottom
   position).


TRANSPOSESELECTEDTABLE
==========================================

Transposes the contents of a previous selected table (by means of other
NSEQL commands).

**Parameters**

   None.



**Returns**

   Nothing.
